---
title: Postfix Manager
tags: [專案, mail-server, postfix, django, self-hosted]
updated: 2026-08-17
source_count: 4
---

# Postfix Manager

[[Ethan]] 的自架郵件伺服器管理系統：以 **Django + Docker** 把繁瑣的郵件服務配置封裝成 Web UI，管理 Postfix（SMTP）、Dovecot（IMAP/POP3）、OpenDKIM（DKIM 簽章），並整合 Certbot 憑證與 Nginx 反向代理。

## 核心設計

- 把 Postfix / Dovecot / OpenDKIM 配置封裝成 Web 操作，信箱帳號直接對應 Linux 系統使用者。
- 透過 **Nginx 8443 SSL Stream Proxy** 統一對外的加密郵件接入點。
- 自動整合 Let's Encrypt 憑證簽發（Certbot webroot 驗證）、自動產生 DKIM 金鑰與 SPF/DMARC 記錄。
- 一鍵部署腳本 `deploy.sh`。（附有預設 admin 帳密，部署後須立即修改。）

## 容器架構

模組化容器協同：Postfix（收送信）、Dovecot（存取 + 充當 Postfix 的 SASL 驗證後端）、OpenDKIM（外寄簽章提升信譽）、Certbot（自動簽發/更新 TLS）、Django（Web 管理後台 + SQLite）。

## 安全防禦（Fail2ban）

伺服器暴露公網、常遭 botnet 暴力破解，故部署 Fail2ban：

- **容器日誌整合**：容器 json 日誌軟連結到宿主機 `/var/log`（`postfix-docker.log`、`dovecot-docker.log`），讓宿主機 Fail2ban 讀得到。
- **指數增長封鎖**：10 分鐘內失敗 5 次即封；初始封 1 天，之後 `base × 2^ban_count` 遞增（1→2→4 天…），上限約 5 週避免誤判無限期封鎖。
- 維護指令：`docker exec -it postfix mailq`（查佇列，積壓過多可能被垃圾郵件利用）、`fail2ban-client status <jail>`（查封鎖）、`fail2ban-client set <jail> unbanip <IP>`（誤鎖解封）。

### 兩個「有在跑卻看不到」的真實漏洞

自架服務暴露公網，fail2ban 三個 jail 全綠、封鎖數字也在增加時，仍漏掉大量攻擊——兩個根因都不在儀表板上，得對照原始日誌實際筆數才抓得到：

1. **封鎖規則掛錯鏈（2026-07-05）**：規則掛在 `INPUT` 鏈，卻被 Docker DNAT 繞過——`fail2ban-client status` 有建黑名單，但 `iptables -L INPUT -v -n` 命中計數始終為 0。解法是把 action 強制注入 Docker 預留的 `DOCKER-USER` 鏈頂端（`chain="DOCKER-USER"`），攻擊封包在進入容器前就被 DROP；systemd 加 `After=docker.service` 避免開機順序造成該鏈遺失。同時把 `findtime` 拉到 86400s 對付慢速分散攻擊。
2. **jail regex 匹配不到多數監聽器（2026-08-01）**：filter 寫成 `postfix/\w+\[`，只匹配得到 25 埠的 `postfix/smtpd`。submission／smtps 監聽器叫 `postfix/submission/smtpd`、`postfix/smtps/smtpd`，中間那個 `/` 不是 `\w`——**587 與 465 埠上每一筆 SASL 認證失敗都被靜默漏掉**。陰險之處是封鎖歷史看不出破綻：攻擊者 7/22 打 25 埠時被正常封過，之後整批搬到 587，7/25–8/1 這週 96% 的攻擊 fail2ban 完全沒看到。修法 `postfix/(?:\w+/)?\w+\[`，`fail2ban-regex` 全日誌 13135 → 13972（+837，正好等於漏掉筆數），`reload` 即可不必重啟。

**教訓：「有在跑」不等於「看得到」**——這正是 [[自製-Claude-Code-Skills]] 的 `server-security-audit` skill 要例行對照日誌的原因。

## 相關

- [[Ethan]] — 作者
- [[自製-Claude-Code-Skills]] — `server-security-audit` skill 正源自類似的 mail server 安全巡檢經驗（失明的 fail2ban filter、缺登入日誌）
- [[wiki/entities/KeyLogger-Server|KeyLogger-Server]]、[[wiki/entities/CORE-PULSE|CORE-PULSE]]、[[Quartz-閱讀網站]] — Ethan 的其他專案

## 來源

- [[工作專案/Postfix-Manager/Postfix-Manager-郵件伺服器管理系統]]（工作紀錄，系統總覽與部署）
- [[工作專案/Postfix-Manager/mail_server_guide]]（工作紀錄，架構與 Fail2ban 維護指南）
- [[工作專案/作品集/Postfix_Manager_Portfolio_Presentation|Postfix Manager 作品集簡報]]（2026-08-14，Nginx SSL Stream 繞過封鎖埠、DOCKER-USER 鏈修正，已脫敏）
- [[2026-08 工作紀錄]]（2026-08-01 VPS 安全巡檢：fail2ban jail regex 漏洞）
