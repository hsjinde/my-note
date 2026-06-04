---
title: Claudian
tags: [工具, Obsidian 插件, AI]
updated: 2026-06-04
source_count: 1
---

# Claudian

[[Obsidian]] 內建的 AI 助理插件，提供在 Obsidian 內直接與 AI 對話、讀寫筆記、執行 [[Claude-Code-Skills]] 的功能。

## 開啟方式

`Ctrl+P` → 搜尋「chat view」

## 主要功能

- 直接在 Obsidian 內與 AI 對話
- 可讀取 vault 內所有筆記作為 context
- 整合 [[Claude-Code]] 的 Skills 系統
- 可執行檔案讀寫、搜尋、編輯

## API 配置範例（搭配 DeepSeek）

```bash
ANTHROPIC_API_KEY=
ANTHROPIC_BASE_URL=https://cli.19980803.xyz/v1
ANTHROPIC_DEFAULT_SONNET_MODEL=claude-sonnet-4-6
ANTHROPIC_DEFAULT_HAIKU_MODEL=gemini-3.1-pro-low
ANTHROPIC_DEFAULT_OPUS_MODEL=gpt-5.5
```

> [!warning] 安全提示
> `ANTHROPIC_AUTH_TOKEN` 是敏感資訊，請妥善保管，不要上傳到公開的 GitHub 儲存庫。

## 相關概念

- [[Obsidian]] — 運行的平台
- [[Claude-Code]] — 後端 AI 引擎
- [[Claude-Code-Skills]] — Claudian 內可用的 Skills
- [[LLM-Wiki]] — Claudian 維護的知識庫

## 來源

- [[插件安裝]]（2026-05-28）