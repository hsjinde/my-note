---
title: RAG vs LLM Wiki
tags: [知識管理, AI, 比較]
updated: 2026-07-05
source_count: 2
---

# RAG vs LLM Wiki

兩種讓 AI 使用知識的主流做法，核心差異在於**知識是否持久累積**。

## 對比表

| 比較項目 | RAG | LLM Wiki |
|---|---|---|
| 知識累積 | 每次重新檢索，不累積 | 持久保存，持續累積 |
| 跨文件引用 | 不穩定（依賴 embedding 品質） | 穩定（同一份 Wiki 頁面）|
| 矛盾標記 | 難以確保每次都被觸發 | 直接寫入 Wiki，永久有效 |
| 平台綁定 | 綁定特定 embedding model | 純 Markdown，任何模型可讀 |
| 大量資料處理 | 有優勢（向量化） | 受 Context Window 限制 |
| 維護成本 | 低（自動索引） | 需要 AI 定期維護（Lint） |
| 回答一致性 | 每次可能不同 | 高度一致（同一份 Wiki）|

## 何時選 RAG

- 資料量極大（數千份文件以上）
- 資料是結構化查詢場景（企業搜尋）
- 不需要跨文件合成與累積

## 何時選 LLM Wiki

- 個人知識庫（數十到數百份來源）
- 需要跨文件合成與持久記憶
- 重視回答一致性與知識脈絡
- 希望不被特定平台綁定

## 為什麼 Karpathy 反對 chunked RAG

Karpathy 的核心主張：wiki 查詢應**餵完整 wiki context 給長上下文模型**，而非用 RAG 切塊檢索。
理由是 chunked RAG 會**切碎知識**，破壞 LLM 跨頁面推理、走訪知識圖譜的能力——這正是 LLM Wiki 相對 RAG 的關鍵優勢。
因此社群實作普遍**建議長上下文模型**：Wiki 越大，越需要長 context。

## 混合使用

當 Wiki 成長到 Context Window 壓力很大時，可以在 LLM Wiki 上層疊加 RAG，用向量檢索定位到相關 Wiki 頁面，再讓 AI 讀取頁面回答。

## 相關概念

- [[LLM-Wiki]] — LLM Wiki 完整介紹
- [[知識庫架構設計]] — 不同架構的設計比較

## 來源

- [[3 層架構打造個人 AI 大腦：從 Raw Data 到持久知識庫 🛠️]]（HC AI說人話，2026-04）
- [[Karpathy 的 LLM Wiki 火了，我改造了一下，比原版好用十倍]]（范凱說AI，2026-04）