---
title: Obsidian × LLM Wiki 生態與最佳實踐
tags: [知識管理, LLM-Wiki, Obsidian, query]
date: 2026-07-05
---

# 查詢：Obsidian LLM Wiki 相關知識（外部網搜）

## 回答摘要

[[Andrej-Karpathy]] 提出的 **LLM Wiki** 是個人知識管理模式，核心比喻：
原始筆記是 source code，wiki 是編譯後的 binary——**編譯一次、持續維護**，而非每次查詢重新檢索（RAG）。

> 「Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase.」

三大操作 **ingest / query / lint** 構成整個系統；`index.md`＋`log.md` 是骨幹。
**本 vault 即為此模式的實作**（見 [[LLM-Wiki]]、[[知識庫架構設計]]、[[Ingest-工作流]]）。

## 外部實作對照（2026）

| 實作 | 型態 | 特點 |
|---|---|---|
| Karpathy LLM Wiki | Obsidian 官方插件 | entity/concept 頁 + 對話式查詢；官方評分 95/100、10 語系 |
| claude-obsidian | Claude Code plugin | `.raw/` 唯讀、10 skills、hot cache 保留 session context |
| second-brain (NicholasSpisak) | 跨 agent skills | `/second-brain-{ingest,query,lint}`；支援 Claude Code/Codex/Cursor/Gemini |

## 對本 Vault 的啟示

1. **Lint 節奏**：社群建議每 ingest ~10 次或每月一次（本 vault 已納入 [[core_rules]]）。
2. **Query 歸檔**：把有價值答案存回 `wiki/queries/`（本頁即示範，補足先前空的 queries/）。
3. **長上下文優於 chunked RAG**：Karpathy 主張餵完整 wiki context，切塊會破壞跨頁推理（見 [[RAG-vs-LLM-Wiki]]）。

## 引用來源

- [MindStudio — What Is Karpathy's LLM Wiki](https://www.mindstudio.ai/blog/andrej-karpathy-llm-wiki-knowledge-base-claude-code)
- [TheToolNerd — Step-by-Step Build Guide](https://www.thetoolnerd.com/p/step-by-step-guide-build-your-own-second-brain-obsidian-kaparthy)
- [GoPenAI — Local-first LLM + Obsidian KMS](https://blog.gopenai.com/building-a-local-first-knowledge-management-system-with-llm-and-obsidian-1a6be6cd94d7)
- [agricidaniel — claude-obsidian plugin](https://agricidaniel.com/blog/claude-obsidian-ai-second-brain)
- [NicholasSpisak/second-brain (GitHub)](https://github.com/NicholasSpisak/second-brain)
- Karpathy 原始 gist：`gist.github.com/karpathy/442a6bf555914893e9891c11519de94f`
