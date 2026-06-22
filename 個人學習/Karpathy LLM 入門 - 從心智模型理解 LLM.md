---
title: Karpathy LLM 入門 - 從心智模型理解 LLM
tags: [個人學習, LLM, AI, 教學, Karpathy]
date: 2026-06-22
updated: 2026-06-22
source: Karpathy "Intro to Large Language Models" 演講 + 2025 Year in Review 補充
---

# Karpathy LLM 入門：從心智模型理解 LLM

> 本文整理自 [[Andrej-Karpathy]] 的經典演講「Intro to Large Language Models」（1 小時入門，2023-11）。目標：**沒看過演講的人也能建立 LLM 的完整心智模型**。
> 
> [!info] 2025 年重要補充
> Karpathy 在 2025-02 發布了 3.5 小時的升級版「[Deep Dive into LLMs like ChatGPT](https://www.youtube.com/watch?v=7xTGNNLPyMI)」，並在 2025-12 的「[LLM Year in Review](https://karpathy.bearblog.dev/year-in-review-2025)」中指出：**RLVR（Reinforcement Learning from Verifiable Rewards）已成為訓練管道的新第 4 階段**，催生了 reasoning models（如 OpenAI o1、DeepSeek R1）。本文以 1hr Intro 為骨架，並在 §2 補充 2025 年的 RLVR 新階段。

## 0. 一句話心智模型

**LLM ＝ 對網路做有損壓縮 ＋ 訓練成預測下個 token ＋ 被塑形成助手。**

把這句話記住，後面所有細節都是在解釋這三件事怎麼發生、為什麼會造成你看到的各種現象。

## 1. 核心心智模型：LLM 到底在做什麼？

### 1.1 預測下一個 token

LLM 的訓練目標極度簡單：**給前面一段文字，預測下一個 token**。

- Token 是文字片段（不是字、不是詞），由 tokenizer 把文字切成約 5 萬～10 萬種 token
- 例：`Hello world` 可能變成 `[Hello][ world]` 兩個 token
- 模型輸出：對每個候選 token 給一個機率分布

> [!important] 這個看似無聊的目標，等價於「理解整個世界」。

### 1.2 為什麼「預測下個 token」等於理解世界？

要準確預測下個 token，模型必須理解上下文背後的所有結構：

- 文法、詞義、修辭
- 物理常識、社會常識
- 數學、邏輯、推理
- 程式、論文、對話風格

**預測下個 token 是一個被迫學會一切的任務**——因為唯一能壓低 loss 的方法就是真的理解。

### 1.3 壓縮即理解

預測機率分布 ≈ 對資料做壓縮。

- 越能精準預測 → 越能用越少 bit 編碼 → loss 越低
- 訓練就是在「擬合出網路這份資料的最優壓縮器」
- 壓縮好 → 內部必然學到了資料的結構 → 看似湧現出「智慧」

> [!note] Karpathy 反覆強調：不要把 LLM 想成「模仿人類思考」，要把它想成「極致的壓縮器」。

## 2. 3 階段管道：心智模型是怎麼被造出來的

### Stage 1｜Pretraining（預訓練）— 造出「文件續寫器」

- **資料**：爬取網路 → 過濾 → 數 TB 文字
- **Tokenizer**：把文字切成 token 序列
- **訓練**：在 GPU 叢集上跑數週到數月，預測下個 token
- **產出**：Base Model（基礎模型）

Base model **不是助手**。給它 `Q: 法國首都? A:`，它不會回答巴黎，而是會繼續生成「下一個問題」或別的東西——因為它在模仿「文件長什麼樣」。

### Stage 2｜SFT（Supervised Fine-Tuning）— 把續寫器變成助手

- 人類標註者寫數萬筆 `(prompt, response)` 對
- 用這些對去微調 base model
- 模型學到：「在這種 prompt 格式下，應該輸出助手式回答」

> [!important] SFT 之後，模型從「文件續寫器」變成「助手」。本質是切換了「模仿的對象」。

### Stage 2.5｜RLHF（Reinforcement Learning from Human Feedback）— 對齊

為什麼需要 RLHF？SFT 只教了「怎麼回答」，沒教「什麼是好的回答」。

- **訓練 Reward Model**：讓人類比較兩個回答哪個好，訓練一個打分模型
- **Policy Gradient（PPO）**：用 reward model 當教練，調整 LLM 讓它產生高分回答
- 對齊目標：**helpful（有用）/ harmless（無害）/ honest（誠實）**

RLHF 之後才有 ChatGPT 那種「會拒絕、會承認不知道」的行為。

### Stage 3｜RLVR（Reinforcement Learning from Verifiable Rewards）— 推理能力湧現 🆕

> [!info] 2025 年新增階段。源自 Karpathy「2025 LLM Year in Review」。

- **動機**：RLHF 的 reward model 是「人類偏好」，可被 game；SFT/RLHF 都只是薄薄的微調
- **RLVR 做法**：用**可自動驗證的客觀 reward**訓練（如數學題有正確答案、程式題可執行測試）
- **湧現現象**：模型**自發發展出「推理策略」**——拆解問題、中間計算、來回嘗試、自我修正
  - 這些策略很難用 SFT 教，因為「最佳推理軌跡」長什麼樣沒人知道
  - RL 讓模型自己摸索出來
- **新 scaling law**：**test-time compute**——生成更長推理鏈 ＝ 更多「思考時間」＝ 更強能力
- **代表**：OpenAI o1（2024 末）、DeepSeek R1、o3、Claude 3.7 Sonnet thinking 等

> [!important] 2025 年能力進步主要來自 RLVR 的長 RL 跑時，而非更大的 pretraining。模型大小差不多，但「想更久」。

### 管道全圖

```
網路文字 → [Pretraining] → Base Model → [SFT] → 助手 → [RLHF] → 對齊助手 → [RLVR] → 推理模型
           (數月 GPU)                  (數萬對)            (人類回饋)              (可驗證 reward)
                                                                                    + test-time 思考
```

## 3. 用心智模型解釋 LLM 的行為

這一節是核心：**所有 LLM 的怪行為，都能從「它只是在預測下個 token」推導出來**。

### 3.1 為什麼會幻覺（Hallucination）

- LLM **沒有記憶、沒有 state、沒有資料庫**
- 它只是根據 context 算下個 token 的條件機率
- 當它「不知道」時，它沒辦法說「我不知道」——它只會輸出**看起來像答案的高機率 token**
- 結果：自信地講出錯誤事實

> [!note] 幻覺不是 bug，是 base behavior。要降幻覺只能靠：工具、RAG、RLHF 教它說「不知道」、prompt 限制。

### 3.2 為什麼需要工具

模型本身是「文字接龍」，所以：

- **計算機**：算術本來就不擅長（要用 token 預測模擬加法）
- **瀏覽器**：沒有最新資訊
- **Code Interpreter**：寫程式跑出來比自己推理準

SFT / RLHF 教模型：在需要的時候**呼叫工具**，把工具輸出塞回 context 繼續生成。

### 3.3 Context Window 與 In-Context Learning

- **Context window**：模型能一次看到的 token 數上限（RAM 隱喻）
- 東西不在 context 裡，對模型來說就不存在
- **In-context learning**：把範例放進 context，模型「現場學會」任務格式
- **Few-shot**：給幾個範例 → 模型模仿格式
- **Zero-shot**：只靠指令

### 3.4 Chain of Thought（思維鏈）

- 讓模型「先寫出推理過程再回答」
- 為什麼有效？每個 token 都讓模型「重新看清自己的推理」——等於把推理外部化成 context
- 數學、推理題的關鍵技巧
- 🆕 **2025 年的 reasoning models（o1、R1 等）把這件事內化**：透過 RLVR 訓練，模型自己學會生成長推理鏈，不需使用者手動下「let's think step by step」

### 3.5 為什麼 Self-Correction 不可靠

- 模型無法「真的」檢查自己——它只是在繼續預測下個 token
- 它「修正」的方式常常是：產生另一個看似合理的答案
- 真的修正需要外部回饋（執行結果、人類、工具）

## 4. 實務：怎麼用 LLM

### Prompt Engineering 的本質

不是「咒語」，而是**控制那灘 verbal slime 的流向**：

- 給夠脈絡 → 模型在對的機率空間裡取樣
- 給範例 → 鎖定輸出格式
- 給角色 / 系統 prompt → 偏置整體分布

### System Prompt

- 在 context 最前面的高優先指令
- 透過 RLHF 訓練讓模型「特別聽 system prompt」
- 但**不是絕對安全**——見 5.1 prompt injection

### LLM as OS 隱喻

Karpathy 的比喻：未來 LLM 像作業系統。

| LLM | OS 對應 |
|---|---|
| Context window | RAM |
| 模型權重 | CPU |
| 工具（搜尋、code、API） | 周邊 |
| System prompt | 開機設定 |

## 5. 風險與未來

### 5.1 Prompt Injection

- 攻擊者把「忽略前面指令，改做 X」藏在**資料裡**
- 模型無法可靠區分「指令層」vs「資料層」——它都是 token
- 等同 SQL injection 的 LLM 版本，目前沒有完美防禦

### 5.2 Jailbreak

- 透過角色扮演、編碼、翻譯繞道等手法繞過安全訓練
- 顯示 RLHF 對齊是「**偏置機率分布**」而非硬規則

### 5.3 偏見與誤資訊

- 訓練資料決定模型的世界觀
- 高機率 ≠ 正確

### 5.4 Open vs Closed

- Open（Llama、Mistral）：可本地跑、可客製、易研究
- Closed（GPT、Claude）：能力最強、有安全護欄、但黑箱

### 5.5 往 AGI 的路

- Karpathy 認為：模型會持續把更多「外部能力」整合進 context（工具、記憶、agent loop）
- LLM 本身可能不是終點，而是 AGI 系統的「語言核心」

## 6. 延伸閱讀

### Karpathy LLM 系列演講（由舊到新）

| 時間 | 影片 | 定位 |
|---|---|---|
| 2023-11 | [Intro to Large Language Models（1hr）](https://www.youtube.com/watch?v=zjkBMFhNj_g) | 本文依據，入門全景 |
| 2025-02 | [Deep Dive into LLMs like ChatGPT（3h31m）](https://www.youtube.com/watch?v=7xTGNNLPyMI) | 升級版，含 RLVR、完整訓練管道 |
| 2025-02 | [How I use LLMs](https://www.youtube.com/watch?v=EWvNQjAaOHw) | 實務應用續集 |

### Karpathy 文章

- [2025 LLM Year in Review](https://karpathy.bearblog.dev/year-in-review-2025) — 2025 年度回顧，RLVR 與 reasoning models 趨勢
- [LLM Wiki Gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — 他另一個重要想法：用 LLM 維護個人知識庫

### Vault 內相關

- [[Andrej-Karpathy]] — 演講者簡介，與他在知識管理領域的另一個想法
- [[LLM-Wiki]] — Karpathy 提出的個人知識庫模式（與本篇的 LLM 技術是不同主題，但同一作者）

---

> 想消化這篇進 wiki 知識庫，跟我說一聲（會觸發 `/llm-wiki` ingest）。
