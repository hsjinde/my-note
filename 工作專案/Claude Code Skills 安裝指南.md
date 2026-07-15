---
title: Claude Code Skills 安裝指南
updated: 2026-07-15
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

## 方法四：交給 agent 自動安裝（複製即用的 prompt）

把下面整段 prompt 貼給任一具備 shell 能力的自動化 agent（例如常駐的 Hermes），它就能自己把四個 skill 裝好並自我驗證。已內建幾個自動化最容易出錯的點：**用安裝後資料夾名驗證**（避免 `cloudflare-use`／`server-security-audit` 因 repo 名不同被誤判成失敗）、**逐一 npx→git 後備**（成敗以落地的 `SKILL.md` 為準，不看 exit code、不整批重來）、**非互動**（避免 headless shell 卡在 CLI 提問）、**只驗到路徑層級**（skill 需新 session 才掃描，禁止誇稱「已確認觸發」）。

> [!note] 平台
> 這版針對 macOS / Linux（bash、`~/.claude/skills`）。Windows PowerShell 請把路徑改成 `$env:USERPROFILE\.claude\skills\`、指令改用 PowerShell 語法。

````text
# 任務：自動安裝並驗證四個開源 Agent Skills（macOS / Linux）

你要在本機自動安裝下列四個 Claude Code / Agent SDK skills，裝完做「路徑層級」驗證並回報。全程**非互動**，不要卡在任何提示上；任何一步失敗就走後備方案，不要整批放棄。

## 四個 skill（skills.sh id → 安裝後的資料夾名）
1. hsjinde/note-maintain                → note-maintain
2. hsjinde/ui-fix-verify                → ui-fix-verify
3. hsjinde/cloudflare-use-skill         → cloudflare-use          （skill 在子資料夾）
4. hsjinde/server-security-audit-skills → server-security-audit   （skill 在子資料夾）

⚠️ 驗證與檢查一律用「安裝後的資料夾名」，不要用 repo 名——後兩個的 repo 名和實際資料夾名不同，用 repo 名去 test 會誤判成安裝失敗。

## 步驟 0：決定安裝範圍（自行判斷，並在回報裡講明選了哪個）
- 若你此刻正在一個「應該隨這個 repo 一起攜帶這些 skill」的特定專案裡 → 專案層級：`SKILLS_DIR="$PWD/.claude/skills"`（請確認 $PWD 是專案根）
- 否則（常駐個人 agent、跨專案通用）→ 個人層級：`SKILLS_DIR=~/.claude/skills`
之後所有指令都用 `$SKILLS_DIR`。

## 步驟 1：前置檢查
```bash
command -v git >/dev/null || echo "MISSING: git"
command -v npx >/dev/null || echo "MISSING: npx (需要 Node.js)"
mkdir -p "$SKILLS_DIR"
```
git 與 npx 兩者都缺就停下來回報，不要硬跑。

## 步驟 2：逐一安裝——先試 skills.sh 一行（非互動），成敗以「落地的 SKILL.md」為準
```bash
for pair in \
  "note-maintain:hsjinde/note-maintain" \
  "ui-fix-verify:hsjinde/ui-fix-verify" \
  "cloudflare-use:hsjinde/cloudflare-use-skill" \
  "server-security-audit:hsjinde/server-security-audit-skills"; do
  name="${pair%%:*}"; repo="${pair#*:}"
  if [ -f "$SKILLS_DIR/$name/SKILL.md" ]; then echo "SKIP $name（已存在）"; continue; fi
  npx --yes skills add "$repo" < /dev/null || true      # < /dev/null 確保非互動，卡住/失敗都往下走
  if [ -f "$SKILLS_DIR/$name/SKILL.md" ]; then
    echo "OK   $name (npx)"
  else
    echo "NEED_FALLBACK $name"
  fi
done
```
- `npx skills add` 的實際落點由該 CLI 決定；若它沒把 SKILL.md 放進你選的 `$SKILLS_DIR`（或卡住被 /dev/null 中斷），該 skill 會被標成 `NEED_FALLBACK`，交給步驟 3 用 git 補到正確位置。
- 只對被標成 `NEED_FALLBACK` 的那幾個做步驟 3，不要因為一個失敗就重裝全部。

## 步驟 3：git 後備（只跑 NEED_FALLBACK 對應的區塊）
repo 根目錄就是 skill 本體的兩個，直接 clone 進去：
```bash
git clone --depth 1 https://github.com/hsjinde/note-maintain.git "$SKILLS_DIR/note-maintain"
git clone --depth 1 https://github.com/hsjinde/ui-fix-verify.git  "$SKILLS_DIR/ui-fix-verify"
```
skill 在子資料夾的兩個，clone 到暫存區 → 複製子資料夾 → 清掉暫存（別把整包 repo 留在工作目錄）：
```bash
TMP="$(mktemp -d)"
git clone --depth 1 https://github.com/hsjinde/cloudflare-use-skill.git "$TMP/cf"
cp -r "$TMP/cf/cloudflare-use" "$SKILLS_DIR/cloudflare-use"
git clone --depth 1 https://github.com/hsjinde/server-security-audit-skills.git "$TMP/ssa"
cp -r "$TMP/ssa/server-security-audit" "$SKILLS_DIR/server-security-audit"
rm -rf "$TMP"
```

## 步驟 4：驗證（只驗到路徑層級，不要誇大）
```bash
for n in note-maintain ui-fix-verify cloudflare-use server-security-audit; do
  test -f "$SKILLS_DIR/$n/SKILL.md" && echo "OK   $n" || echo "FAIL $n"
done
```
重要誠實邊界：skill 只有在**新 session** 才會被掃描載入。所以你此刻能如實宣稱的只有「已安裝到 `<路徑>`，下個 session 生效」；**不要**宣稱「已確認會觸發／正常運作」——那要重開對話講觸發詞才算數。

## 步驟 5：回報
- 選了哪個安裝範圍（個人／專案）、`$SKILLS_DIR` 的實際絕對路徑。
- 四個 skill 各自：用 npx 還是 git 後備、最終路徑、OK 或 FAIL。
- 列出每個 skill 裝完仍需的設定（你不用替我做，列出來提醒即可）：
  - note-maintain：預設 vault 有 `core_rules.md`、`wiki/index.md`、`Clippings/`、`wiki/log.md`；結構不同要改它的 SKILL.md。
  - ui-fix-verify：需要可跑的 dev server（建議設 `.claude/launch.json`）＋瀏覽器工具。
  - cloudflare-use：專案 `.env` 要放 `CLOUDFLARE_API_TOKEN`（權限 D1 Edit + R2 Storage Edit），需 Node 18+。
  - server-security-audit：先跑一次 `scripts/audit.sh` 看實際拓撲，再填 `references/known-issues.md`。
````

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
