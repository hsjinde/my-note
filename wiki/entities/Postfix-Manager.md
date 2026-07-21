---
title: Postfix Manager
tags: [專案, mail-server, postfix, django, self-hosted]
updated: 2026-07-21
source_count: 2
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

## 相關

- [[Ethan]] — 作者
- [[自製-Claude-Code-Skills]] — `server-security-audit` skill 正源自類似的 mail server 安全巡檢經驗（失明的 fail2ban filter、缺登入日誌）
- [[KeyLogger-Server]]、[[CORE-PULSE]]、[[Quartz-閱讀網站]] — Ethan 的其他專案

## 來源

- [[工作專案/Postfix-Manager/Postfix-Manager-郵件伺服器管理系統]]（工作紀錄，系統總覽與部署）
- [[工作專案/Postfix-Manager/mail_server_guide]]（工作紀錄，架構與 Fail2ban 維護指南）
