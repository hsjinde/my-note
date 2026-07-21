---
title: OpenCode
tags: [工具, AI, 程式助理, MCP]
updated: 2026-07-21
source_count: 2
---

# OpenCode

開源的命令列 AI 程式助理（類似 [[Claude-Code]] / Cursor），在本 vault 有兩個角色：**提供第三方開放模型**，以及**掛載 MCP server 當工具**。

## 角色一：第三方模型來源（透過 oc-go-cc）

`oc-go-cc` 是一套 API 格式轉換 Proxy，讓 [[Claude-Code]] 透過 OpenCode Go/Zen 的開放模型執行——即時把 Anthropic 格式 ↔ OpenAI/Responses/Gemini 格式互轉。

```
Claude Code（Anthropic 格式）→ oc-go-cc（localhost:9090 即時轉換）→ OpenCode Go/Zen（開放模型）
```

- **用途**：避免平台綁定、用開放模型節省成本（呼應 [[Claude-Code]] 的模型槽 remap 思路）。
- **模型情境**：設定檔可依 `default / fast / think / complex / long_context / background` 分別指定模型（如 Kimi、Qwen、GLM、MiniMax 等），並設 fallback。
- **串接**：設 `ANTHROPIC_BASE_URL` 指向 proxy，或用 CC Switch 桌面應用一鍵切換 Provider。

## 角色二：MCP server 宿主

OpenCode 設定檔（`opencode.json`）掛載多個 [[MCP]] server 當原生工具：

| Server | 用途 |
|---|---|
| `obsidian`（mcpvault） | 讀寫 Obsidian vault（本 vault 的筆記操作備選） |
| `notebooklm` | NotebookLM 筆記本管理 |
| `playwright` | 瀏覽器自動化（開頁、填表、截圖） |
| `open-computer-use` | 桌面自動化（click / type / scroll 等） |

> 設定只在啟動時載入一次，改設定需重啟才生效。懶人包來源：mathruffian-dot/opencode-lazy-packs（呼應 [[mathruffian-dot]] 的 [[Claude-Code-Lazy-Packs]]）。

## 相關

- [[Claude-Code]] — 本 vault 主要 AI 介面；oc-go-cc 讓它跑開放模型
- [[MCP]] — OpenCode 掛載的工具協定
- [[Obsidian]] — mcpvault MCP 操作的對象
- [[Claude-Code-Lazy-Packs]] — 同系列懶人包來源

## 來源

- [[好工具推薦/oc-go-cc 設定指南]]（工具設定，API 格式轉換 proxy）
- [[好工具推薦/OpenCode MCP 配置指南]]（工具設定，已配置的 MCP servers）
