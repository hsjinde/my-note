---
title: LLM 入門（心智模型）
tags: [LLM, AI, 教學, 心智模型]
updated: 2026-07-21
source_count: 1
---

# LLM 入門（心智模型）

整理自 [[Andrej-Karpathy]] 的經典演講「Intro to Large Language Models」，並補上他 2025 年回顧的新階段。目標：建立 LLM 的完整心智模型，用它推導出所有「怪行為」。

## 一句話心智模型

**LLM ＝ 對網路做有損壓縮 ＋ 訓練成預測下個 token ＋ 被塑形成助手。**

- **預測下個 token**：訓練目標極簡，但要壓低 loss 只能真的理解上下文的所有結構（文法、常識、邏輯、程式）——被迫學會一切。
- **壓縮即理解**：越能精準預測 → 越能用越少 bit 編碼 → 內部必然學到資料結構，看似湧現智慧。不要想成「模仿人類思考」，要想成「極致壓縮器」。

## 訓練管道：心智模型怎麼被造出來

| 階段 | 做法 | 產出 |
|---|---|---|
| **Pretraining** | 數 TB 網路文字，預測下個 token（數月 GPU） | Base Model：**文件續寫器**，還不是助手 |
| **SFT**（監督微調） | 數萬筆 `(prompt, response)` 對 | 從續寫器變**助手**，切換模仿對象 |
| **RLHF**（人類回饋） | 訓 reward model → PPO 對齊 helpful/harmless/honest | 會拒絕、會承認不知道 |
| **RLVR** 🆕（可驗證獎勵） | 用可自動驗證的客觀 reward（數學有解、程式可測） | **推理模型**：自發長出拆解、自我修正策略 |

> RLVR 是 2025 年新增的第 4 階段（源自 Karpathy「2025 LLM Year in Review」）。帶來新 scaling law：**test-time compute**——生成更長推理鏈＝更多思考時間＝更強能力。代表：OpenAI o1、DeepSeek R1、Claude thinking 等。2025 年進步主要來自「想更久」而非「更大」。

## 用心智模型解釋 LLM 行為

- **幻覺**：LLM 沒記憶、沒 state、沒資料庫，只算下個 token 的機率。不知道時不會說「不知道」，只會輸出「看起來像答案」的高機率 token。幻覺是 base behavior，不是 bug。
- **為什麼需要工具**：文字接龍不擅長算術（計算機）、沒有最新資訊（瀏覽器）、寫程式跑比推理準（code interpreter）。SFT/RLHF 教模型在需要時呼叫工具，把輸出塞回 context。
- **Context window ＝ RAM**：不在 context 裡對模型就不存在。In-context learning（few-shot / zero-shot）＝把範例放進 context 現場學。
- **Chain of Thought**：先寫推理再回答，把推理外部化成 context；reasoning models 已把它內化。
- **Self-correction 不可靠**：模型無法真的檢查自己，只是再產生一個看似合理的答案；真修正需外部回饋（執行結果、工具、人類）。

## LLM as OS（Karpathy 的比喻）

| LLM | OS 對應 |
|---|---|
| Context window | RAM |
| 模型權重 | CPU |
| 工具（搜尋 / code / API） | 周邊 |
| System prompt | 開機設定 |

## 風險

- **Prompt Injection**：攻擊者把「忽略前面指令」藏在**資料**裡；模型無法可靠區分指令層 vs 資料層（都是 token），如同 SQL injection 的 LLM 版。
- **Jailbreak**：角色扮演／編碼／翻譯繞道，顯示 RLHF 對齊是「偏置機率分布」而非硬規則。
- **偏見與誤資訊**：訓練資料決定世界觀；高機率 ≠ 正確。

## 相關概念

- [[Andrej-Karpathy]] — 演講者，也是 [[LLM-Wiki]] 概念的作者（不同主題，同一人）
- [[RAG-vs-LLM-Wiki]] — 長上下文 vs 檢索切塊的取捨，呼應本篇 context window 概念
- [[MCP]] — 模型呼叫工具的標準協定，呼應「為什麼需要工具」

## 來源

- [[個人學習/LLM與AI/Karpathy LLM 入門 - 從心智模型理解 LLM]]（學習筆記，整理自 Karpathy Intro to LLMs 演講 + 2025 回顧）
