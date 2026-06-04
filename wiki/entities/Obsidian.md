---
title: Obsidian
tags: [工具, 筆記軟體]
updated: 2026-06-04
source_count: 3
---

# Obsidian

以本地 Markdown 為基礎的筆記軟體。這個 vault 的主要平台。

## 在這個 Vault 的角色

- 整個 vault 是用 Obsidian 開啟的資料夾
- 所有筆記以純 Markdown 檔案儲存（任何平台可讀）
- 透過 wikilink 串連筆記關係
- 用 [[LLM-Wiki]] 架構管理知識

## 同步策略

本 vault 使用 **Google Drive 串流**作為主要同步方案：
- vault 建在 Google Drive 資料夾內
- 自動同步到雲端
- 換電腦不遺失

## 已裝插件

詳見 [[Obsidian-插件]]：
- Git（版本控制）
- Remotely Save（雲端硬碟同步）
- Claudian（AI 助理）
- Custom Attachment Location（附件管理）

## Vault 結構

```
my-note/
├── Clippings/        ← 網頁剪藏（待 ingest）
├── 個人學習/         ← 學習筆記
├── 工作專案/         ← 工作紀錄
├── 資料結構-鐵人挑戰-35D/
├── wiki/             ← AI 維護的知識提煉層
├── .claude/skills/   ← Claude Code skills
└── core_rules.md     ← 共用 AI 規則
```

## 相關概念

- [[Obsidian-插件]] — 已裝插件清單
- [[Claudian]] — 主要 AI 整合插件
- [[LLM-Wiki]] — 知識管理架構
- [[Claude-Code]] — 搭配使用的 AI 工具

## 來源

- [[插件安裝]]（2026-05-28）
- [[3 層架構打造個人 AI 大腦：從 Raw Data 到持久知識庫 🛠️]]
- [[Karpathy 的 LLM Wiki 火了，我改造了一下，比原版好用十倍]]