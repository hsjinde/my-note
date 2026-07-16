# note-maintain

Claude Code / Claude Agent SDK 用的 **Agent Skill**：一個指令完成 Obsidian／Markdown 筆記庫（vault）的例行維護——健康檢查 → 自動修安全項 → 消化剪藏（Clippings）→ 規則檔同步檢查 → 寫維護日誌。

核心設計：**能自動修的直接修，需要人決策的整理成一份編號清單一次問完**，不讓使用者來回回覆好幾輪。

## 安裝

Skill 就是一個資料夾加一個 `SKILL.md`，放進 Claude Code 的 skills 目錄即可。

### 個人層級（所有專案都能用）

```bash
# macOS / Linux
git clone https://github.com/hsjinde/note-maintain.git ~/.claude/skills/note-maintain
```

```powershell
# Windows (PowerShell)
git clone https://github.com/hsjinde/note-maintain.git "$env:USERPROFILE\.claude\skills\note-maintain"
```

### 專案層級（只在某個專案生效，可隨 repo 分享給團隊）

```bash
git clone https://github.com/hsjinde/note-maintain.git <你的專案>/.claude/skills/note-maintain
```

不想用 git clone 的話，直接把 `SKILL.md` 複製到上述目錄下的 `note-maintain/` 資料夾也一樣。

安裝後重啟 Claude Code（或開新對話），對 Claude 說「lint」「筆記維護」「消化 Clippings」等關鍵字就會自動觸發。

## 使用前請先調整

這個 skill 假設你的 vault 有以下結構（詳見 `SKILL.md` 的「設定」一節），不同的話直接改 `SKILL.md`：

- `core_rules.md` — 規則的唯一真相來源（`CLAUDE.md`／`AGENTS.md` 只是指向它的指標檔）
- `wiki/` — 提煉後的知識庫，含 `wiki/index.md` 總索引
- `Clippings/` — 待消化的網頁剪藏（唯讀）
- `wiki/log.md` — 維護日誌

## 使用方式

| 你說 | Claude 做 |
|------|-----------|
| `lint`／`筆記維護`／`健康檢查` | 全面檢查：孤立頁、斷鏈、矛盾過時、未消化剪藏；安全項直接修，其餘出編號決策清單 |
| `全部修`／`直接修掉 1–4` | 執行清單上指定項目 |
| `消化 Clippings` | 把剪藏提煉進 wiki（不動原文） |
| `更新 core rules` | 改規則檔並檢查指標檔同步 |

## 授權

MIT — 見 [LICENSE](LICENSE)。
