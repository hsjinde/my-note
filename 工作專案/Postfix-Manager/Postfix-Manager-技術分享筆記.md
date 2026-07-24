---
tags:
  - mail-server
  - postfix
  - dovecot
  - opendkim
  - django
  - docker
  - security
  - iptables
  - fail2ban
  - tech-sharing
created: '2026-07-24'
project: Postfix Manager
source: 'D:\mail-server'
---
# 技術分享筆記：自建多網域郵件伺服器與 Docker 架構硬化實踐 (Postfix Manager)

> **分享主題**：基於 Django + Docker Compose 的企業級自建 Mail Server 管理系統架構、 Port 封鎖繞過技巧與 Fail2ban/iptables 容器化防護實戰踩坑紀錄。

---

## 📌 1. 背景與實務痛點

自建郵件伺服器（Mail Server）長期以來被視為系統維運的噩夢，主要面臨三大挑戰：
1. **繁瑣配置與維護成本高**：Postfix（SMTP）、Dovecot（IMAP/POP3）、OpenDKIM（簽章）設定檔極其複雜，多網域切換容易出錯。
2. **VPS 主機商 Port 封鎖**：許多雲端廠商（如 RackNerd、Linode 等）預設封鎖 Port 25、465、587 或 995，導致客戶端無法正常收發信。
3. **垃圾郵件與安全性攻擊**：未設定正確的 SPF/DKIM/DMARC 會被 Gmail/Outlook 直接退信或歸類為垃圾郵件；同時伺服器會面臨大量的 SSH 與 SMTP-AUTH 慢速暴力破解。

---

## 🏗️ 2. 系統架構與容器化設計

本專案採用 **Django 為控管核心**，透過 Docker Compose 編排各郵件服務，並配合 VPS 主機上的 **Host Nginx (Stream Proxy)** 打造靈活且安全的郵件系統。

### 系統整體架構圖

```
                       ┌─────────────────────────────────────────────┐
 Internet              │ VPS 主機 (Host)                             │
    │                  │                                             │
    │  80/8080         │  Host Nginx (獨立 Web 專案)                  │
    ├─────────────────►│  ├─ 80   : ACME 驗證 + HTTP 轉址             │
    │                  │  ├─ 8080 : Django Web 管理介面               │
    │  8443/2525       │  ├─ 8443 : SSL Stream 終結 ➔ 127.0.0.1:110  │
    ├─────────────────►│  └─ 2525 : SSL Stream 終結 ➔ 127.0.0.1:25   │
    │                  │                                             │
    │                  │  Mail Stack (Docker Compose)                │
    │  25/465/587      │  ├─ postfix  : SMTP 服務 (25, 465, 587)      │
    ├─────────────────►│  ├─ dovecot  : IMAP/POP3 服務 (110, 143, 993)  │
    │  110/143/993     │  ├─ django   : 管理UI (綁定 127.0.0.1:8000) │
    └─────────────────►│  ├─ opendkim : 內部 DKIM 簽章服務 (8891)    │
                       │  └─ certbot  : 12h 週期 Let's Encrypt 簽發  │
                       └─────────────────────────────────────────────┘
```

### 架構設計亮點
- **職責分離**：Host Nginx 負責對外 SSL 終結與 HTTP 反向代理；Docker 容器專注於郵件核心邏輯與管理。
- **帳號資料持久化**：Dovecot 與 Postfix 的虛擬帳號/密碼透過 `passwd.backup` 與 `shadow.backup` 掛載備份，容器重建後會透過 `entrypoint.sh` 自動復原。
- **動態管理與自動化**：Django 容器掛載 Docker Socket 與 Nginx 設定目錄，建立新網域時自動完成憑證複製、OpenDKIM 金鑰生成與 Nginx Reload。

---

## 💡 3. 關鍵技術突破與實戰解決方案

### 🔑 亮點一：Nginx SSL Stream Proxy 繞過 Port 封鎖
針對主機商封鎖 995（POP3S）與 465/587（SMTPS）的問題，採用 **TCP 流量代理（Nginx Stream Module）**：
- **POP3 加密連線**：外部存取 `Port 8443 (SSL)` ➔ Host Nginx 進行 SSL 終結 ➔ 解密後轉發至 `127.0.0.1:110` (Dovecot 容器)。
- **SMTP 備用發信**：外部存取 `Port 2525 (SSL)` ➔ Host Nginx 進行 SSL 終結 ➔ 轉發至 `127.0.0.1:25` (Postfix 容器)。
- **效益**：客戶端只需改用 8443 / 2525 連接埠，即可完美繞過封鎖並確保全鏈路加密。

### 📜 亮點二：Certbot Webroot + Shared Volume 憑證自動續簽
- Certbot 容器掛載 Host Nginx 的 `/home/web/letsencrypt` 目錄作為 Webroot (`/.well-known/acme-challenge/`)。
- 容器內以背景迴圈運作：`certbot renew --webroot -w /var/www/certbot --quiet; sleep 12h`。
- Django 在新增網域時呼叫 Certbot 簽發，並複製到 Nginx 掛載區，無縫銜接憑證更新。

### 🛡️ 亮點三：多網域 OpenDKIM 動態配置
- 建立網域時，Django 透過 `extModule/Util.py` 自動產生 2048-bit RSA 金鑰對。
- 動態更新 OpenDKIM 的三組映射檔：
  - `KeyTable`：網域與私鑰路徑映射
  - `SigningTable`：寄件者 Email 與 Selector 映射
  - `TrustedHosts`：信任可簽章的 IP 清單

---

## ⚠️ 4. 重大踩坑防禦：Docker + Fail2ban / iptables 的連線攔截陷阱

在生產環境稽核中，發現一項極為隱蔽且危險的 **Docker 網路機制陷阱**：

### ❌ 問題現象
Fail2ban 的 `postfix-docker` 規則成功解析了 11,000+ 筆攻擊日誌，且 Fail2ban 顯示已將惡意 IP 封鎖，但**攻擊者依然能持續嘗試登入**，`iptables` 中的封鎖計數器永遠為 0。

### 🔍 根本原因分析 (Root Cause)
1. **DNAT 繞過 INPUT 鏈**：Docker 容器的 Port 映射（如 `-p 25:25`）是在 `iptables` 的 `PREROUTING` 階段進行 DNAT 轉換。
2. **流量走向**：經 DNAT 轉向容器 IP (`172.19.0.5`) 的封包會直接走 `PREROUTING ➔ FORWARD ➔ DOCKER-USER` 鏈，**完全繞過了 Linux 傳統的 `INPUT` 鏈**！
3. Fail2ban 預設把封鎖規則寫入 `INPUT` 鏈，導致「**Fail2ban 抓到了攻擊，卻在 `INPUT` 鏈封鎖空包，攻擊流量從 `DOCKER-USER` 暢行無阻**」。

### 🛠️ 正確解決方案
1. **修改 Fail2ban Action 綁定 `DOCKER-USER` 鏈**：
   在 `/etc/fail2ban/jail.local` 中明確指定封鎖作用鏈：
   ```ini
   [postfix-docker]
   enabled = true
   action = iptables-multiport[name=postfix-docker, port="smtp,ssmtp,submission", protocol="tcp", chain="DOCKER-USER"]
   findtime = 86400  ; 擴大至 24 小時以捕捉慢速分散式爆破
   maxretry = 5
   ```
2. **Docker JSON 日誌解析**：
   Docker 容器日誌格式包裝了 JSON 結構，需撰寫自訂 Filter (`/etc/fail2ban/filter.d/postfix-docker.conf`) 解析內層 Syslog 時間戳與遠端 IP，並在 Jail 設置 `backend = polling` 以防止 Symlink 監聽失效。
3. **系統啟動順序依賴**：
   新增 Systemd Override (`After=docker.service`)，確保開機時 `DOCKER-USER` 鏈已由 Docker 建立，避免 Fail2ban 載入失敗。

---

## ✉️ 5. 郵件可信度 (Deliverability) 最佳實踐

為達到 mail-tester **10/10 完美評分** 並防止被 Gmail/Outlook 擋信：

| 驗證機制 | 設定重點 | 作用 |
|---|---|---|
| **Cloudflare A 記錄** | 設定 `mail.domain.com` 且 **必須為 DNS Only（灰雲）** | 郵件協定非 HTTP，開橘雲代理會導致全盤失效 |
| **雙重 SPF (TXT)** | 主網域與 HELO `mail` 子網域**皆需設定** `v=spf1 ip4:伺服器IP -all` | 防止收信端因 HELO 檢查失敗而被扣分 |
| **DMARC (TXT)** | `_dmarc.domain.com` 設為 `v=DMARC1; p=reject; ...` | 強制退回未通過驗證的偽造郵件 |
| **DKIM (TXT)** | `mail._domainkey.domain.com` 填入 2048-bit 公鑰 | 確保郵件內文未被竄改 |

---

## 🎯 6. 總結與心得

1. **容器化不等於免維運**：Docker 簡化了部署，但帶來了網路層 (`iptables`/`DNAT`) 與日誌層（JSON log）的額外複雜度。
2. **資安硬化必須驗證**：Fail2ban 顯示 Banned 不代表成功封鎖，必須透過 `iptables -L -n -v` 觀察**封包命中計數**以確定防禦生效。
3. **郵件系統重在信任鏈**：SPF、DKIM、DMARC、TLS 與憑證自動續簽是自建 Mail Server 能否順利通訊的生命線。

---

## 🔗 相關資源
- 專案程式碼與備份規畫：[[Postfix-Manager-郵件伺服器管理系統]]
- 伺服器安全變更紀錄：`D:\mail-server\SECURITY_CHANGELOG.md`
- 用戶與客戶端設定指南：`D:\mail-server\SETUP_GUIDE.md`
