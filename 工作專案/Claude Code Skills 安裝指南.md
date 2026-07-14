---
title: Claude Code Skills 安裝指南
updated: 2026-07-14
tags:
  - claude-code
  - skills
  - 安裝指南
---

# Claude Code Skills 安裝指南

適用於 [[自製 Claude Code Skills 總覽]] 中的四個 skill，以及任何標準 Agent Skill。

## Skill 是什麼

一個 skill 就是**一個資料夾 + 一個 `SKILL.md`**，沒有編譯、沒有註冊步驟：

```
skill-name/
├── SKILL.md        # 必要：YAML frontmatter（name、description）+ Markdown 指示
├── scripts/        # 選配：可執行腳本（Claude 可直接執行，不必載入內容）
├── references/     # 選配：需要時才載入 context 的文件
└── assets/         # 選配：輸出用素材（範本、圖示）
```

Claude Code 採**三層漸進載入**：`name + description` 常駐 context（決定是否觸發）→ 觸發時載入 SKILL.md 全文 → 需要時才讀 references／跑 scripts。所以 skill 平常幾乎不佔 token。

## 方法零：skills.sh 一行安裝（最快，推薦）

四個 skill 都已上架 [skills.sh](https://www.skills.sh)（Vercel 開源的 Agent Skills 市集，相容 Claude Code、Cursor、GitHub Copilot 等多種 agent），直接用 `npx skills add` 安裝，不用手動 clone：

```bash
npx skills add hsjinde/note-maintain
npx skills add hsjinde/ui-fix-verify
npx skills add hsjinde/cloudflare-use-skill
npx skills add hsjinde/server-security-audit-skills
```

各 skill 頁面：[note-maintain](https://www.skills.sh/hsjinde/note-maintain)｜[ui-fix-verify](https://www.skills.sh/hsjinde/ui-fix-verify)｜[cloudflare-use-skill](https://www.skills.sh/hsjinde/cloudflare-use-skill)｜[server-security-audit-skills](https://www.skills.sh/hsjinde/server-security-audit-skills)

## 安裝位置（二選一）

| 層級 | 路徑 | 適用 |
|---|---|---|
| 個人 | `~/.claude/skills/<name>/` | 所有專案都能用 |
| 專案 | `<專案>/.claude/skills/<name>/` | 只在該專案生效，可隨 repo 分享給團隊 |

Windows 上個人層級即 `%USERPROFILE%\.claude\skills\`（本機為 `C:\Users\hsjin\.claude\skills\`）。

## 方法一：git clone（推薦，之後 `git pull` 就能更新）

```powershell
# Windows PowerShell — 個人層級
git clone https://github.com/hsjinde/note-maintain.git "$env:USERPROFILE\.claude\skills\note-maintain"
git clone https://github.com/hsjinde/ui-fix-verify.git "$env:USERPROFILE\.claude\skills\ui-fix-verify"
```

```bash
# macOS / Linux — 個人層級
git clone https://github.com/hsjinde/note-maintain.git ~/.claude/skills/note-maintain
git clone https://github.com/hsjinde/ui-fix-verify.git ~/.claude/skills/ui-fix-verify
```

專案層級改 clone 到 `<專案>/.claude/skills/<name>` 即可。

> [!note] repo 結構差異
> `note-maintain`、`ui-fix-verify` 的 repo 根目錄就是 skill 本體，直接 clone 進目標資料夾。
> `cloudflare-use-skill`、`server-security-audit-skills` 的 skill 在**子資料夾**裡，clone 後要複製子資料夾：
>
> ```bash
> git clone https://github.com/hsjinde/server-security-audit-skills.git
> cp -r server-security-audit-skills/server-security-audit ~/.claude/skills/server-security-audit
>
> git clone https://github.com/hsjinde/cloudflare-use-skill.git
> cp -r cloudflare-use-skill/cloudflare-use <專案>/.claude/skills/cloudflare-use
> ```

## 方法二：直接複製資料夾

不想用 git 就把含 `SKILL.md` 的資料夾整個複製到上述目錄，效果相同。

## 方法三：打包成 `.skill` 檔

部分 Claude 介面（如 Claude.ai）用單一 `.skill` 壓縮檔安裝。用 Anthropic `skill-creator` skill 附的打包腳本：

```bash
# 在 skill-creator 的目錄下執行
python -m scripts.package_skill /path/to/server-security-audit
```

產出 `server-security-audit.skill`，交給支援匯入的 UI 即可。

## 安裝後：驗證與觸發

1. **重啟 Claude Code 或開新對話**（新 session 才會掃描 skills 目錄）。
2. skill 是否觸發完全由 `SKILL.md` frontmatter 的 **description** 決定——Claude 比對使用者的話與 description 來判斷。所以驗證方式就是講觸發詞：

| Skill | 試講 |
|---|---|
| note-maintain | 「lint」「筆記維護」「消化 Clippings」 |
| ui-fix-verify | 「這個區塊太高」「手機破版了」 |
| cloudflare-use | 「查一下 D1 的 users 表」「列出 R2 的檔案」 |
| server-security-audit | 「check this server's security」「看一下 mail server 有沒有被盜用」 |

太簡單的一句話請求可能不觸發 skill（Claude 覺得自己直接做就好）；問題描述得越具體、越多步驟，越會觸發。

## 各 skill 的安裝後設定

- **note-maintain**：假設 vault 有 `core_rules.md`、`wiki/index.md`、`Clippings/`、`wiki/log.md`；結構不同直接改 SKILL.md 的「設定」節。
- **ui-fix-verify**：需要可跑的 dev server（建議設定 `.claude/launch.json` 讓 Claude 能自己啟動預覽）+ 瀏覽器工具。
- **cloudflare-use**：專案根目錄 `.env` 放 `CLOUDFLARE_API_TOKEN`（Dashboard → My Profile → API Tokens，權限 D1 Edit + Workers R2 Storage Edit）。需 Node.js 18+，零 npm 依賴。
- **server-security-audit**：先跑一次 `scripts/audit.sh` 看實際拓撲，再填 `references/known-issues.md` 的 Topology 節；之後持續維護該檔。

## 疑難排解

- **skill 沒觸發**：確認路徑是 `.claude/skills/<name>/SKILL.md`（多一層或少一層都抓不到）→ 開新對話 → 把請求講得更具體。
- **想手動強制觸發**：直接輸入 `/<skill-name>`。
- **同名衝突**：專案層級蓋過個人層級；避免兩邊裝同一個 skill 的不同版本。
