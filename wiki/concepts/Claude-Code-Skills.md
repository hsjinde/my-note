---
title: Claude Code Skills
tags: [Claude Code, Skills, 工具]
updated: 2026-06-04
source_count: 1
---

# Claude Code Skills

這個 vault 與 Claude Code 系統中可用的 Skills 整理。詳見 [[Claude-Code]] 實體頁。

## Obsidian 專屬技能

| Skill | 用途 |
|---|---|
| `obsidian-markdown` | 建立/編輯 Obsidian Flavored Markdown（wikilink、callout、frontmatter、tag）|
| `json-canvas` | 建立/編輯 `.canvas` 檔案（node、edge、group）|
| `obsidian-canvas-creator` | 從文字內容生成 Canvas（MindMap/freeform）|
| `obsidian-bases` | 建立/編輯 `.base` 資料庫視圖（filter、formula、view）|
| `obsidian-cli` | 深度操作 vault：讀寫搜尋、plugin/theme 開發、DOM 檢查、截圖 |

## 圖表與視覺化

| Skill | 用途 |
|---|---|
| `excalidraw-diagram` | 從文字生成 Excalidraw 圖（.md/.excalidraw/animated 三種模式）|
| `mermaid-visualizer` | 文字轉 Mermaid 圖，內建語法錯誤預防 |

## 網頁與資料處理

| Skill | 用途 |
|---|---|
| `defuddle` | 從網址萃取乾淨 Markdown，去除導覽列/側邊欄 |

## 開發與系統調試

| Skill | 用途 |
|---|---|
| `claude-api` | 建立/除錯/最佳化 Claude API 或 Anthropic SDK 應用 |
| `run` | 啟動並執行當前 project app 以驗證變更 |
| `verify` | 跑測試或手動驗證 code 變更 |
| `code-review` | 檢查 code 品質、重用性、效率，直接修復 |
| `security-review` | 對待合併分支做安全稽核 |
| `review` | 檢視 GitHub Pull Request |
| `init` | 在 project 根目錄初始化標準 `CLAUDE.md` |

## 系統設定與自動化

| Skill | 用途 |
|---|---|
| `update-config` | 透過 `settings.json` 設定 Claude Code 環境與 Hooks |
| `keybindings-help` | 透過 `~/.claude/keybindings.json` 自訂快捷鍵 |
| `fewer-permission-prompts` | 掃描對話紀錄，把常見唯讀指令加入白名單以減少權限彈窗 |
| `loop` | 定期或模型節奏重複執行某 prompt / slash command |

## 觸發方式

`/skill-name`（例如 `/obsidian-markdown`）

## 安裝位置原則

- 建議只裝當前 Project 需要的 Skills
- Skills 會佔用 Context Window 頭部（最精準的位置）
- 全域技能太多會降低模型表現
- 這個 vault 採用 vault 專用安裝（`.claude/skills/`）

## 相關概念

- [[Claude-Code]] — Claude Code 工具本身
- [[LLM-Wiki]] — 其中 `llm-wiki` skill 維護整個 Wiki 系統

## 來源

- [[目前的skills]]（2026）