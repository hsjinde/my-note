---
title: MCP（Model Context Protocol）
tags: [MCP, AI, 工具, 協定, agent]
updated: 2026-07-21
source_count: 1
---

# MCP（Model Context Protocol）

**MCP 不是取代 API，而是架在 API 之上的語義層**——把靜態的 API 端點，變成模型能自行推理、發現、呼叫的「活介面」。

## API 為什麼餵不飽 LLM

- API 是**程式對程式**的確定性契約：定義端點 → 送 request → 拿 response，傳統軟體很完美。
- 但 LLM 不是只呼叫單一端點：它要串接多個工具、解讀非結構化資料、追問下一步——它需要的是 **context（脈絡）**，不只是工具存取權。
- API 像一個上鎖的櫃子：你得知道開哪個抽屜、鑰匙什麼形狀。模型只能靠人**硬編碼＋反覆 prompt 說明**去猜。

## MCP 翻轉的假設

- API 假設「雙方都事先知道會拿到什麼」；MCP 反過來，給模型一份**機器可讀的即時地圖**，動態發現工具能做什麼、要什麼輸入、回什麼輸出。
- 本質差別：API 是兩個應用間的**程式碼層契約**；MCP 是模型與環境間的**語義協定**。
- 「呼叫哪個工具、何時呼叫」的邏輯，從 app code **移進模型的推理層**——給模型一個工具箱，而非逼它背每個工具怎麼用。

## 底層怎麼運作

- MCP server 是一個輕量程序，貼著你的服務／資料源運行，用 **JSON schema** 自描述能做什麼。
- 模型透過標準介面（WebSocket / HTTP）連上，取 metadata → 直接呼叫函式，不用猜、不必再 prompt engineering schema。
- 效益：不再蓋 100 個客製整合，只蓋**一個 MCP 介面**，所有相容模型即插即用。

## 關鍵定位：MCP on top of API

- API 不會消失，它仍是系統實際運作的基礎。MCP 取代的是模型與 API 之間的**中介層（middleware）**，像翻譯官把現有 API 轉成模型看得懂的格式。
- 真正改變的是「**client 是誰**」：API 的 client 是另一支程式或使用者；MCP 的 client 是**模型本身**。

## 挑戰

- **採用門檻**：需要 server／client／工具生態對齊同一標準。
- **安全與控制**：模型能直接呼叫工具，就得有清楚的權限層（避免誤刪檔、亂寄信、亂改資料庫）。規格已定義 capabilities／scopes／認證，但仍屬早期。
- **開發者心態**：從「端點與路由」轉向「能力與脈絡」——描述系統「能做什麼」而非「怎麼做」。

## 大局類比

如同 HTTP 統一了 FTP／Gopher／Telnet，MCP 想為 AI agent 做同樣的事：連接器蓋一次，任何相容模型（Gemini、Claude、GPT）都能立刻使用，讓**模型成為軟體的一等公民使用者**。

## 在這個 Vault 的應用

- [[OpenCode]] 掛載多個 MCP server（obsidian／NotebookLM／Playwright／桌面自動化）作為原生工具。
- 本 vault 的筆記操作備選方案 `mcpvault` 就是一個 Obsidian MCP server。

## 相關概念

- [[LLM-入門]] — 「為什麼 LLM 需要工具」的心智模型基礎
- [[OpenCode]] — 實際掛載 MCP server 的 AI agent
- [[Claude-Code]] — 同樣支援 MCP 的命令列 agent

## 來源

- [[Clippings/MCP vs API Why traditional APIs are failing AI agents]]（web clipping，Google Cloud Tech / Smitha Kolan）
