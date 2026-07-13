---
title: 自製 Claude Code Skills 總覽
updated: 2026-07-13
tags:
  - claude-code
  - skills
  - 自製工具
---

# 自製 Claude Code Skills 總覽

我自己開發並開源的四個 Claude Code / Claude Agent SDK **Agent Skills**，全部 MIT 授權。
安裝方式見 [[Claude Code Skills 安裝指南]]。

| Skill | 一句話 | Repo |
|---|---|---|
| note-maintain | 一個指令完成 Obsidian vault 例行維護 | [GitHub](https://github.com/hsjinde/note-maintain) |
| ui-fix-verify | 強制 UI 修改「先量測、後截圖驗證」才准回報完成 | [GitHub](https://github.com/hsjinde/ui-fix-verify) |
| cloudflare-use | 繞開 wrangler、直打 REST API 操作 Cloudflare D1 + R2 | [GitHub](https://github.com/hsjinde/cloudflare-use-skill) |
| server-security-audit | Docker Linux 伺服器可重複執行的安全巡檢 | [GitHub](https://github.com/hsjinde/server-security-audit-skills) |

---

## note-maintain — 筆記庫例行維護

一個指令跑完整條維護流水線：**健康檢查 → 自動修安全項 → 消化剪藏（Clippings）→ 規則檔同步檢查 → 寫維護日誌**。

- **核心設計**：能自動修的直接修；需要人決策的整理成一份編號清單一次問完，不讓使用者來回好幾輪。
- **觸發詞**：`lint`、`筆記維護`、`健康檢查`、`消化 Clippings`、`更新 core rules`。
- **前提假設**（不同就改 SKILL.md）：vault 有 `core_rules.md`（規則唯一真相來源）、`wiki/`（含 `index.md`）、`Clippings/`（唯讀）、`wiki/log.md`。
- 本 vault 就是它的原生環境，`.claude/skills/note-maintain/` 已安裝。

## ui-fix-verify — UI 修改驗證流程

解決的痛點：AI 改完 CSS 就說「已修正」，使用者連續回「還是太高」「還是一樣」——每一輪都在讓使用者當人肉驗證器。這個 skill 把驗證變成**不可跳過**的步驟。

流程六步：

1. **重現**——截圖確認問題真的看得到
2. **量測**——把「太高」翻成數字（現在幾 px？目標多少？）
3. **Checkpoint commit**——未提交改動先 `wip:` commit，「不好看，退回」一步完成
4. **修改**——用 class／id 精確定位
5. **驗證（不可跳過）**——重新截圖 + 重新量測 + 手機視口（375px）必驗
6. **回報**——附截圖、量測數字、commit hash

- **需求**：可跑的 dev server（建議設 `.claude/launch.json`）+ Claude Code 瀏覽器工具。
- **觸發詞**：「太高」「跑版」「破圖」「手機破版」等視覺問題回報。

## cloudflare-use — D1 + R2 直連操作

讓 Claude Code 在任何專案操作 Cloudflare D1 資料庫與 R2 儲存。**零設定**：資料庫、bucket、帳號自動探測（環境變數 → `.env` → wrangler 設定檔），唯一必填是 `.env` 裡的 `CLOUDFLARE_API_TOKEN`。

為什麼繞開 `npx wrangler`（Windows 實戰踩過的坑）：

1. PowerShell 5.1 寫檔預設帶 BOM → 寫進 D1 的中文全變亂碼
2. PowerShell console codepage 非 UTF-8 → 查詢結果亂碼，agent 誤判資料壞掉反覆重試燒 token
3. wrangler v4 `--file` 改走上傳匯入 → SELECT 不回結果列；Node < 22 直接報錯
4. 每次 `npx wrangler` 冷啟動近 10 秒

解法：三支 Node 腳本（`_lib.cjs`／`d1.cjs`／`r2.cjs`）直打 Cloudflare REST API，SQL 放 UTF-8 JSON body 傳輸，單次操作 <1 秒。中文／emoji／引號／多語句都安全。

- **需求**：Node.js 18+（內建 fetch），零 npm 依賴。
- **加分做法**：在 SKILL.md 補上專案事實（table schema、R2 key 慣例），Claude 操作時就帶著這些知識。實例見 [Osaka-web 的 SKILL.md](https://github.com/hsjinde/Osaka-web/blob/main/.claude/skills/cloudflare-use/SKILL.md)。

## server-security-audit — 伺服器安全巡檢

Docker-based Linux 伺服器的可重複安全巡檢，源自一次真實 incident-response：當時找到（並修好）一個對多小時暴力破解**完全失明**的 fail2ban filter，以及一台完全沒有登入日誌的 mail server。這些教訓固化成 `references/known-issues.md` 範本和一支唯讀收集腳本 `scripts/audit.sh`。

檢查三塊：

- **Docker inventory**——每個 container 做什麼、是否對外暴露、暴露是否刻意
- **Security posture**——防火牆、SSH 設定（密碼登入關了嗎）、fail2ban jail 是否真的抓得到東西（零命中的 jail 未必是好消息，可能是 jail 瞎了）
- **Mail server**——從網路背景噪音（字典攻擊）中分離真訊號：不明成功登入、外寄郵件暴增

安全模型：

| 層級 | 行為 |
|---|---|
| 唯讀診斷 | 不問直接跑 |
| 低風險修復（filter regex、log 設定、重啟服務） | 直接套用並回報 |
| 影響對外連線（防火牆、port binding） | 無論多有把握，一律先問 |

使用前務必用自己伺服器的實際拓撲填寫 `references/known-issues.md`，並持續維護 `Fixes already applied`／`Open questions`／`Deferred` 三節——這是讓未來巡檢「確認修復還在」而非「從頭重新診斷」的關鍵。

---

## 共同設計理念

四個 skill 反覆出現的原則，之後寫新 skill 可延用：

- **減少人機往返**：批次提問（note-maintain 的編號清單）、不可跳過的自我驗證（ui-fix-verify）。
- **狀態外置**：把「上次修了什麼、哪些不要再問」寫進檔案（known-issues.md），讓 skill 跨 session 有記憶。
- **繞開不可靠的中間層**：shell 編碼、慢 CLI 直接跳過，改打 API（cloudflare-use）。
- **安全分層**：唯讀自動跑、低風險直接修、高風險必問（server-security-audit）。
