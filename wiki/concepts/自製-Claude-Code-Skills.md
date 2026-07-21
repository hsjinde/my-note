---
title: 自製 Claude Code Skills
tags: [Claude-Code, skills, 自製工具, 開源]
updated: 2026-07-21
source_count: 2
---

# 自製 Claude Code Skills

[[Ethan]] 自行開發並開源的四個 Claude Code / Agent SDK **Agent Skills**（全 MIT），皆上架 [skills.sh](https://www.skills.sh)（Vercel 的 Agent Skills 市集），可 `npx skills add hsjinde/<name>` 一行安裝。與官方／社群 skills 清單見 [[Claude-Code-Skills]]。

## 四個 Skill

| Skill | 一句話 | 觸發 |
|---|---|---|
| **note-maintain** | 一個指令跑完 Obsidian vault 維護流水線（健康檢查 → 自動修 → 消化 Clippings → 規則同步 → 寫日誌） | 「lint」「筆記維護」「消化 Clippings」 |
| **ui-fix-verify** | 強制 UI 修改「先量測、後截圖驗證」才准回報完成 | 「太高」「跑版」「手機破版」 |
| **cloudflare-use** | 繞開 wrangler、直打 REST API 操作 Cloudflare D1 + R2 | 「查 D1 的 users 表」「列 R2 檔案」 |
| **server-security-audit** | Docker Linux 伺服器可重複執行的安全巡檢 | 「check server security」 |

> **note-maintain** 就是本 vault 的原生環境（`.claude/skills/note-maintain/` 已裝，你現在跑的 lint 就是它）。

### 各 skill 重點

- **note-maintain**：能自動修的直接修；需決策的整理成一份**編號清單一次問完**，不讓使用者來回好幾輪。前提假設 vault 有 `core_rules.md`、`wiki/`、`Clippings/`、`wiki/log.md`。
- **ui-fix-verify**：解決「AI 改完 CSS 就說已修正，使用者連續回還是太高」的痛點。六步：重現截圖 → 量測（翻成 px 數字）→ checkpoint commit → 修改 → **驗證不可跳過**（重截 + 重量 + 手機 375px 必驗）→ 回報附截圖/數字/commit。
- **cloudflare-use**：零設定自動探測 D1/R2/帳號，唯一必填 `.env` 的 `CLOUDFLARE_API_TOKEN`。繞開 `npx wrangler` 是為避開 Windows 實戰坑（PowerShell BOM 讓中文亂碼、console codepage 非 UTF-8、wrangler v4 SELECT 不回列、冷啟動近 10 秒）。改用三支 Node 腳本直打 REST API，單次 <1 秒。
- **server-security-audit**：源自一次真實 incident-response（找到對暴力破解**完全失明**的 fail2ban filter、一台沒有登入日誌的 mail server）。查 Docker inventory / security posture / mail server 三塊；教訓固化成 `references/known-issues.md`。

## 共同設計理念（寫新 skill 可延用）

- **減少人機往返**：批次提問（note-maintain 編號清單）、不可跳過的自我驗證（ui-fix-verify）。
- **狀態外置**：把「上次修了什麼、哪些別再問」寫進檔案（`known-issues.md`），讓 skill 跨 session 有記憶。
- **繞開不可靠的中間層**：shell 編碼、慢 CLI 直接跳過，改打 API（cloudflare-use）。
- **安全分層**：唯讀自動跑、低風險直接修、高風險必問（server-security-audit）。

## Skill 機制與安裝

- 一個 skill ＝**一個資料夾 + 一個 `SKILL.md`**（YAML frontmatter 的 `name`＋`description` 決定觸發），無編譯無註冊。Claude Code 採**三層漸進載入**：name+description 常駐 → 觸發載入全文 → 需要才讀 references／跑 scripts，平常幾乎不佔 token。
- 安裝：`npx skills add`（最快）或 `git clone` 到 `~/.claude/skills/<name>/`（個人）或 `<專案>/.claude/skills/`（專案，可隨 repo 分享）。**新 session 才會掃描載入**；裝好只能宣稱「已安裝、下個 session 生效」，不能宣稱「已確認觸發」。

## 相關概念

- [[Claude-Code-Skills]] — 官方與社群 skills 總覽
- [[Claude-Code]] — skills 的執行環境
- [[AI-產品開發工作流]] — 同樣「用 skill 強制最佳實踐」的思路（superpowers 框架、不可跳過的驗證）
- [[LLM-Wiki-搭建指南]] — note-maintain 把其中的維護工作流一鍵化

## 來源

- [[工作專案/自製 Claude Code Skills 總覽]]（工作紀錄，四個 skill 的設計理念與踩坑）
- [[工作專案/Claude Code Skills 安裝指南]]（工作紀錄，安裝／觸發驗證／疑難排解）
