---
title: "我如何以一人產品團隊的身份運用 AI 進行開發"
source: "https://georgexing.substack.com/p/how-i-build-with-ai-as-a-1-person?just_subscribed=true"
author:
  - "[[George Xing]]"
published: 2026-05-07
created: 2026-06-18
description: "我用來以 10 倍速度打造更高品質產品的工具與技巧"
tags:
  - "clippings"
---
過去幾個月，我嘗試了許多不同的 AI 寫程式工具與技巧，目的是想簡化我作為 1 人團隊在個人專案上的產品開發工作流程。我的目標是建立一個系統，讓開發軟體變得有效率、不費力且有趣。雖然隨著新模型、功能和工具的推出，總是有進步的空間，但我的工作流程已經比剛開始時有了顯著的改善。

有 3 類事物帶來了真正的改變：使用代理技能框架 (agent skills framework)、建立測試自動化，以及設計遠端優先 (remote-first)、符合人體工學的工作流程。實施這些改變帶來的生產力提升是相輔相成的，綜合起來，我估計它們讓我生產力至少提升了 10 倍，同時產出的品質也比原本更好。

## 1. 使用 /superpowers 進行開發

當我第一次開始用 Claude Code 建立個人應用程式時，我非常依賴計畫模式 (Plan Mode)：我會開一個運行最新 Opus 模型的全新對話，按 Shift+tab 進入計畫模式，把我覺得有用的產品和技術脈絡盡可能倒進去，反覆修改直到計畫看起來正確，然後按下綠燈執行並看著它運作。

這對於簡單的功能很管用，但對於稍微複雜一點的東西，結果就時好時壞。我有一個名為 LemmeCook 的烹飪應用程式，裡面的新手引導流程、建議餐點邏輯、食譜匯入以及庫存管理，都會以微妙的方式交互影響。當這些互動變得重要時，Claude Code 總是會做出反直覺的產品和 UX 選擇。

我無法判斷問題是出在計畫細節不足、執行偏離了計畫，還是兩者皆有。我發現自己花了數小時在修復 bug 和非預期的行為上。這讓人感到疲憊，也讓開發變得不那麼有趣。

為我改變遊戲規則的是使用了 [/superpowers](https://github.com/obra/superpowers)，這是一個由 Jesse Vincent 創建的 Claude 擴充功能，你可以輕鬆地從 Anthropic 官方的擴充功能市場安裝。這個套件 [^1] 綁定了四項關鍵技能，分別對應傳統產品開發的步驟：brainstorming (腦力激盪)、writing-plans (撰寫計畫)、executing-plans (執行計畫) 和 code-review (程式碼審查) [^2]。這四項技能可以作為獨立步驟直接呼叫，或者作為一個鏈結的序列一起執行。

### brainstorming (腦力激盪)

brainstorming 技能幫助你從產品想法生成結構化的 PRD (產品需求文件)。當我有個想法——通常是關於某個功能應該如何運作的半成型想法——我會將大腦中的想法傾倒給 Claude，並讓 brainstorming 技能接手。它會向我總結它所聽到的內容，提出釐清的問題，並浮現我沒考慮到的產品缺口和衝突。對於前端工作，它會生成線框圖 (wireframes)，讓我在瀏覽器中決定視覺和 UX 的方向。感覺就像同時擁有一個 PM 討論對象和設計師。

作為一個開發者，我非常享受腦力激盪階段。我發現從一組選項中回答問題（尤其是搭配視覺輔助時），比從零開始想出正確的問題和潛在答案要容易得多。我也能夠管理這個過程並注入我個人的品味：我可以很容易地引導 Claude 探索更多選項並發揮創意，或者要求它更快地收斂到一個夠好的解決方案。

![/superpowers brainstorming](https://substackcdn.com/image/fetch/$s_!CHW_!,w_720,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb0d58a81-1d89-455a-b714-65d08eee8963_1432x1182.png)

/superpowers 會引導你進行互動式的腦力激盪過程，以做出關鍵的產品和設計決策

### writing-plans (撰寫計畫)

一旦 brainstorming 技能產出了我滿意的產品規格，writing-plans 技能就會將規格轉化為詳細的實作計畫。這個計畫是一個 markdown 檔案，包含離散的任務，每個任務都包含詳細的指示。以下是它為我們剛剛激盪出來的規格所寫的計畫片段：

```markup
# Cook Tab — "Tonight" Trio Implementation Plan  

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Replace the Cook tab's filtered 6+ card grid with a daily-ritual surface showing exactly 3 AI-curated picks — each with a personalization reason chip — plus a single reroll button, refreshing once per local calendar day.

**Architecture:** Mobile rewrites `cook.tsx` to render three `TonightCard` rows (new) + one reroll button. `usePoolSuggestions` is simplified to a 3-pick query with a `reroll` action; filter state is removed. The API engine `generateSuggestionsForUser` accepts an `excludeTitles` array (used to avoid repeats on reroll) and produces a `reason: { type, text }` field per suggestion via a new post-composition reasoning step. The `suggestion_pool` table gains a small `reason` JSONB column.

**Tech Stack:** React Native / Expo Router / TanStack Query / NativeWind on mobile · Express / Zod / Anthropic SDK / Supabase on API · TypeScript end-to-end · Maestro for E2E.

**Spec reference:** `docs/superpowers/specs/2026-05-02-cook-tab-tonight-trio-design.md`
---
## File Structure

| File | Action | Responsibility |
|------|--------|----------------|
| `packages/shared/src/types/suggestion.ts` | Modify | Add `ReasonType` union and optional `reason` field on `PreGeneratedSuggestion` |
| `supabase/migrations/00031_suggestion_pool_reason.sql` | Create | Add nullable `reason` JSONB column to `suggestion_pool` |
| `apps/api/src/services/preGenerationEngine.ts` | Modify | Accept `excludeTitles` option; replace feasibility-only selection with `(cuisine, protein-class)` diversity-aware selection; add post-composition reason selection step; persist `reason` to row |
...
Total: 7 new files, 6 modifies, 2 deletes, 4 Maestro flows. ~700-900 LOC delta.
---
### Task 1: Add `ReasonType` and `reason` field to shared types

**Files:**
- Modify: `packages/shared/src/types/suggestion.ts`
- [ ] **Step 1: Add the `ReasonType` union and `Reason` interface**

In `packages/shared/src/types/suggestion.ts`, add **before** `PreGeneratedSuggestion`:
export type ReasonType = 'history' | 'inventory' | 'variety' | 'library'
export interface Reason {
  type: ReasonType
  text: string
}
...
```

取決於我正在做什麼，我通常會花幾分鐘檢查整體計畫和任務是否合理。我通常不會花時間自己深入探究每個任務的實作細節。然而，我經常會試探 Claude，了解計畫如何處理特定的邊緣情況，並驗證它會實作出預期的產品行為。

例如，我在這裡問 Claude 計畫是否確保所有建議的餐點都足夠不同（它並沒有），並建議了一個修復方案：

![](https://substackcdn.com/image/fetch/$s_!GIop!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe01d74af-d629-4c4f-bd9e-b40f796f4241_1642x1160.png)

Claude 針對我在計畫中發現的不想要的產品行為建議了一個修復方案

### executing-plans (執行計畫)

一旦實作計畫看起來不錯，executing-plans 技能就會讀取它並撰寫程式碼。我通常可以選擇要序列執行，還是為每個任務啟動一個全新的子代理，這會用到另一個名為 subagent-driven development (子代理驅動開發) 的綁定技能。我通常會選擇較快的平行處理選項。Claude 會自動辨識哪些任務可以平行處理，哪些任務彼此有相依性，以及哪些任務可以由能力較弱的模型處理（例如用 Sonnet 或 Haiku 代替 Opus）。

根據我的經驗，執行通常是最長的步驟，可以自主運行 10 分鐘到幾個小時不等，取決於任務。這是一個離開電腦的好時機。我通常會啟動執行階段，並把它當作一個去散步或喝杯咖啡的休息點。

![](https://substackcdn.com/image/fetch/$s_!kbHr!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffe8687a8-ea7b-412d-b8a0-e6ccf8fee382_1638x1048.png)

/superpowers 提供不同的執行選項

### code-review (程式碼審查)

第四個技能，code-review，會在建置後執行並檢查成果。它會識別並修復 bug，標出實作偏離計畫的地方，並抓出你會希望更有經驗的工程師抓出的問題。

這一步驟可以與 executing-plans 串接，這樣 Claude 就能自動執行程式碼審查並在一個循環中進行編輯，直到所有被審查者標記的問題都解決為止，完全不需要我的介入。

用 superpowers 進行開發幫助我體認到，AI 尚未消除識別問題、使用者、使用案例以及要完成的工作 (jobs-to-be-done) 的需求。在透過代理開發產品時，產品思維是不可或缺的，但現在我們可以在 AI 輔助、高度壓縮的方式下完成這項工作——而擁有一個代理技能框架來*強制執行*這些最佳實踐，會帶來很大的不同。

事實上，在一個生成程式碼既便宜又容易的世界裡，上游的工作其實變得*更*重要，而不是更不重要。我們很容易受誘惑直接跳進執行階段，並在最終產出上進行迭代，但我一次又一次地發現，在開始時認真進行腦力激盪、審查規格、並仔細檢視實作計畫，最終能縮短獲得滿意成果的時間，並帶來更好的產品品質。

![](https://substackcdn.com/image/fetch/$s_!iNdU!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4c5a5d27-f0c0-4902-8855-1a08aee4608b_1979x807.png)

透過前期投入更多時間在產品定義上，你可以省下大量的人力和日曆時間！

## 2. 測試自動化

使用像 superpowers 這樣結構化的產品開發框架顯著提升了我的產出品品質，但它遠非完美無缺。舉例來說，Claude Code 有時會產生重疊的抽屜面板和彈出視窗，導致我的烹飪應用程式中某些 UI 元素無法點擊。在後端，它有時會漏掉競態條件 (race conditions)，導致載入錯誤資料或無法載入資料。這些類型的 bug 很難在產品規格的測試計畫中輕易測試出來。

### Codex 審查

雖然我比較喜歡將 Claude Code 作為我主要的寫程式夥伴，但我也嘗試過 Codex，並發現它作為一個模型/框架通常更為嚴謹和有條理。相較於 Claude Code，Codex 較缺乏創意，但會更嚴格地遵循指示。我已經開始讓 Codex 去檢視 Claude 所做的大型提交。

在三月下旬，OpenAI 發布了適用於 Claude Code 的 Codex 擴充功能，我決定將其納入我的工作流程。我會讓 Codex 使用 codex:rescue 技能來審查 Claude 撰寫的實作計畫。在實作步驟之後，Claude Code 會啟動 Codex 作為子代理來審查程式碼。接著，Claude 會處理 Codex 標記的任何問題，並再次提交審查。這個循環會持續到所有問題都解決為止，有時需要多達 3 或 4 次迭代。

雖然額外的審查週期會花費時間（有時超過 30 分鐘），而且 Codex 有時在審查時可能會過於賣弄學問（例如將一個小問題歸類為嚴重問題），但它通常能抓出 Claude Code 漏掉的真實 bug 或不一致之處。我發現納入 Codex 審查顯著提升了整體的產品品質，尤其是在後端功能和資料密集型的流程中。

![](https://substackcdn.com/image/fetch/$s_!uiLD!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbbc55a87-85ce-4fbf-a8df-303e62a38d85_1644x1060.png)

Codex 標記出 Claude 實作中的問題，然後由 Claude 進行修復

### implementation-review (實作審查) 技能

無論程式碼審查做得多好，都無法取代針對實際使用者行為和產品流程的端到端測試 (end-to-end testing)。在 Stripe，實作審查是產品開發中最重要的一個發布前步驟，一位訓練有素的人類審查員會審核你的產品發布，並整理出一份問題清單（包含會阻礙發布的反饋）讓團隊解決。對於具有許多流程的新產品或複雜產品，這可能會花上好幾週的來回溝通，但這是維持產品品質標準的關鍵部分。

為了模擬這個過程，我創建了自己的 implementation-review Claude 技能。對於我的行動應用程式，它會跟我一起生成一組要測試的情境。對於每個情境，它會運行一個類似人類審查員測試應用程式的循環：對當前狀態截圖、分析截圖、記錄發現、決定並執行下一個動作、再截圖以了解行為。最後，它會生成一份按嚴重程度組織的發現摘要。

在底層，這個技能使用不同的工具來審查不同類型的產品。對於行動應用程式，它使用一個由 Sentry 維護的名為 XcodeBuildMCP 的 MCP 伺服器，讓 Claude 可以建置並測試 iOS 行動應用程式。XcodeBuildMCP 將許多底層的 AppleScript 和 simctl Xcode 模擬器工具包裝成對代理友好的介面。對於網路應用程式，它使用 playwright-mcp 來導航瀏覽器。

當你將這些串聯在一起時，這個設定會變得非常強大：

1. 每一個實作計畫和每一行程式碼都由 Codex 審查。
2. 每個關鍵的使用者情境都由 implementation-review 技能自動審查。
3. 每個實作審查的問題都會被捕捉並送回給 Claude 解決（接著再由 Codex 進行程式碼審查）。

加入實作審查可能會使我的開發週期增加數小時，但如果設定得當，它會自動運行，絕對值得這項投資。

![](https://substackcdn.com/image/fetch/$s_!Uuyn!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e5884e6-ef77-446a-9ae0-7e404fc8f496_1543x1680.png)

結合 implementation-review 技能和 Codex 審查的自動化、迭代測試，可以省下好幾天手動除錯的時間

## 3. 遠端優先的人體工學

在開始使用 superpowers 和我的測試自動化設定後，我與 Claude 的工作模式從主要是互動式對話，轉變為短暫密集的互動式腦力激盪和產品定義，接著是 Claude 自主運行的長時間對話。這讓我的工作流程變得更有效率，因為我可以做像是在睡前啟動一項工作，然後在早上檢查結果的事。

但我開始遇到一些其他的問題：

1. 打字成為一個主要的瓶頸。輸入更多脈絡給 LLM 總是能帶來更好的結果，但在我的新工作流程中，脈絡的數量成了限制因素——更多的脈絡意味著更好的腦力激盪，從而產生更好的規格，進而帶來更好的執行和產出。但我不想為我想建立的每個新功能打一篇小論文。
2. 監控和保持長時間運行的對話。我需要開著筆電來讓長時間運行的執行對話繼續，這當我在共同工作空間工作需要回家時很難做到。有時候，任務需要做決定，且會在我離開鍵盤時等我的輸入等上好幾個小時。我希望能隨時隨地查看進度並幫我的代理排除障礙。
3. 管理平行運作的代理。當我在等待 Claude 時，我開始同時處理多個專案，以及同一個專案中的多個功能。要在 Ghostty 的分頁和視窗中管理所有東西變得很困難 [^3]

這些是新穎的開發者人體工學問題，而我找到了不錯的解決方案：

### 語音輸入

我開始預設使用 WisprFlow 用說的而不是打字的，這對我腦力激盪迭代的品質和速度產生了戲劇性的正面影響 [^4]。我能傳達資訊的吞吐量至少是打字的 3-4 倍，尤其是當我在手機上工作時。雖然我意識流的表達遠不如打字那樣結構良好或文法正確，但在達成目標上其實非常有效：現今的尖端模型非常寬容，而且很擅長從雜訊中提取訊號。

在過去幾個月中，無論是在桌面端還是行動端，我與 Claude Code 的絕大多數互動都是透過 WisprFlow 進行的。小提示：花幾分鐘將常用的詞彙（例如 Claude、Tailscale、Codex）加入它的字典裡是一項很棒的投資，它會為你省下許多不必要的錯誤和日後浪費的循環。

![](https://substackcdn.com/image/fetch/$s_!7VSq!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb49a9247-a0e1-4102-ade2-1c8507ea9f7b_1072x446.png)

在家工作意想不到的好處之一就是你可以自由地口述，不用像瘋子一樣小聲說話 :)

### 遠端開發

為了讓對話持續進行，我將所有開發工作轉移到我的 Mac Mini 上 [^5]。我考慮過走雲端虛擬機 (VM) 的路線，但為了我的 iOS 開發，我需要運行 MacOS，而大多數具成本效益的雲端選項運行的都是 Linux。

我改編了[這份指南](https://kareemf.com/on-agentic-coding-from-anywhere)，並設定了 tmux (用於持久對話)、Tailscale (用於在我的手機、Macbook 和 Mac Mini 之間建立可靠的網路)，以及 [Claude remote control](https://code.claude.com/docs/en/remote-control) (用於同步 Claude 對話)。假設你有一台 Mac Mini，設定非常快。tmux 和 Tailscale 是免費的，我還花了 20 美元購買了 Blink 的 iOS 應用程式，這讓我在手機上有一個終端機並可以直接進行 ssh 存取。最後這一部分不是絕對必要，但我想要一個備案，以防遠端控制連線中斷且我需要重啟它們時使用。

現在，當我在家或在共同工作空間用筆電工作時，我會 ssh 連進我的 Mac Mini，這樣當我蓋上筆電需要回家時，我的代理會繼續運行。當我在通勤時，我會繼續在火車上或車裡透過 Claude 行動應用程式監控我的對話。

偶爾我會被系統權限擋住，必須轉而使用筆電上的原生「螢幕分享」應用程式（或者比較不理想的情況下，用手機上的 RVNC Viewer iOS 應用程式），這讓我可以去接管控制我的 Mac Mini。

![Claude Code sessions via remote control](https://substackcdn.com/image/fetch/$s_!KeGR!,w_720,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe8da3244-9056-4c6a-ab35-c20a142d5ca4_1810x1372.png)

透過遠端控制，在 Claude 桌面應用程式、行動應用程式以及 TUI (未顯示) 上處理同一個 Claude Code 對話

### 代理程式開發環境 (Agentic coding environment)

在過去 6 個月裡出現了一個全新的產品類別，專門用來更好地服務這些新興的代理寫程式工作流程。

為更妥善地管理我平行處理的寫程式對話，我嘗試了幾個工具：[^6]

- [Superset.sh](https://superset.sh/) 是一個代理原生的終端機產品，內建了與 Github 和 Git worktrees 的整合，讓你能輕鬆開啟新的分支和功能。你可以設定 setup 和 teardown 腳本，處理像設定 `.env` 檔案這類的維運任務。它配備了 diff 和 markdown 檢視器，以及一個內建瀏覽器。當 Claude 需要我輸入資訊時，我會收到通知要切換到那個分頁。Claude Code 的文字介面 (TUI) 是 Claude 產品介面中最敏捷、功能最齊全的，所以這是使用這個工具的一大好處。
- Claude Desktop (桌面應用程式) 在過去幾個月裡得到了很多關注，現在不僅支援 SSH 連線，還支援多個專案及平行對話，同時具備 Git worktree 整合。我也很喜歡能在同一個應用程式裡使用 Claude Cowork 和 Chat，這讓透過鍵盤快捷鍵進行切換變得很容易：仍然有些產品研究和一般知識型的工作，我傾向在非程式碼優先的介面中完成。[^7]
- [cmux](https://cmux.com/) 是一個開源終端機，由 libghostty (Ghostty 所建構的終端機函式庫) 驅動，內建垂直分頁以及針對平行對話的通知功能。與 Superset 不同，它專注於純粹的終端機體驗，而不是端到端的寫程式工作流程。我喜歡 cmux 的地方在於它的速度，還有能處理自動通訊埠轉發 (port forwarding) 的[原生 ssh 支援](https://cmux.com/blog/cmux-ssh)——這在腦力激盪期間測試並將模擬畫面視覺化時超級實用。

我最終選擇了一個混合式的設定。我喜歡在 Macbook 上透過 SSH 連接 Mac Mini 使用 Claude 桌面應用程式，但我同時也透過 Ghostty 中的 TUI 保持相同的互動對話開啟。每個對話都設定成與 `/remote-control` 一起運行，這會將我所有對話跨兩個介面同步。這也讓我的對話可以透過 Claude 行動應用程式存取，這給了我一個很好的介面，能在移動中進行監控、解除阻礙及引導方向。

![](https://substackcdn.com/image/fetch/$s_!62dn!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F751da239-6908-4089-b380-3577166c94a7_2120x1650.png)

我的遠端開發設定，將我的 Mac Mini 作為一個始終開機的樞紐

至於我實際的工作流程，我比較喜歡與 TUI 互動（更準確地說是「說話」），因為它反應更快、更可靠，而且總是第一個獲得新功能的介面。但這個混合式設定讓我可以透過 Claude 桌面應用程式滾動瀏覽執行歷史紀錄，或在不同對話間快速切換，而不會打斷 CLI。要閱讀規格和計畫的 markdown 檔案，這比終端機的介面好多了。在桌面應用程式中，複製貼上和圖片輸入的運作也順暢得多：例如，你沒辦法直接把一張圖片從本機拖曳到遠端的 CLI 對話中。

我的設定的確還有一些不完美的地方，我認為這反映了現狀：TUI、桌面端、行動端和網頁介面之間在功能和使用者體驗上仍缺乏一致性。我測試過的所有產品都還相對較新且尚未成熟，從每週都有重大功能推出的開發步調就能看出來。好消息是，這些產品正在快速成熟，[^8] 我很樂觀地認為，半年內大部分的缺陷都會被解決。也許我們會統一到一個不那麼破碎的設定（理想情況下是不受限於單一平台的設定！）。就目前而言，我對現有的設定還算滿意。

## 結語

像 `/superpowers` 這樣的代理技能框架、測試自動化，以及更好的遠端人體工學，這些要素是相輔相成的。代理框架將我主動敲擊鍵盤（或口述）的時間壓縮成高效率的衝刺。測試自動化讓我的寫程式代理能夠自主運行更長的時間，同時產生更好的產出。而遠端優先的人體工學讓我可以從任何地方查看這些代理的狀況，讓我的閒置時間也能發揮作用。

在用不同的工具和技巧不斷建構和重建的過程中，我越來越確信，用 AI 來開發產品是一門技術，就像軟體工程或產品管理一樣。我們每個人都能以同樣的 [每月 100 美元](https://georgexing.substack.com/p/all-you-need-is-a-claude-max-subscription) 取用相同的尖端模型、框架和寫程式工具，但運用它們來打造優秀產品的能力，卻是非常不平均的。

就我的經驗來看，AI 開發者最重要的能力包含兩方面：(1) 能夠在腦力激盪時，透過做出具體的產品和設計決策來定義你想開發什麼；(2) 能夠清楚地定義何謂成功，讓你的寫程式代理可以帶著正確的目標，執行一個「寫程式+驗證」的迴圈。為了在今天做到這兩點，了解資料塑模 (data modeling)、API 設計和主從架構 (client/server architecture) 等軟體工程基礎知識仍然很有幫助。我感覺一年後，模型、框架和工具可能會將這些細節抽象化。到那時，我們或許只需要憑藉對產品的品味與信念就能成事了。[^9]

學習這些技能最好的方式就是直接開始動手做點什麼——沒有任何事物能取代第一手的經驗與知識。由於工具和技術的演進速度極快，靜態的資源（包含這篇部落格文章）很快就會過時。事實上，我分享的這個設定在幾個月前根本無法實現，我也完全預期它在幾個月後就會變得落伍。

最後，身為一個開發者，能夠乘上這股巨大的浪潮，感覺無比興奮且充滿力量，但同時也因為它移動的速度太快而令人感到頭暈目眩。我幾個月前學到的東西，感覺已經像是舊時代的產物了。每個禮拜我都會遇到正在嘗試更具野心事物的人——像是經營完全自動化的公司，或是協調成千上萬個代理組成的蜂群——僅僅是要跟上這些發展，感覺就像是一份全職工作了。

如果你也正在開發產品，請與我聯絡（聯絡資訊在我的 about 頁面）。我很樂意交流、學習並交換心得！

[^1]: Garry Tan 也有一個名為 [gstack](https://github.com/garrytan/gstack) 的擴充功能用來解決相同的問題。它在概念上是同一種方法，只是實作方式不同。我個人還沒試過 gstack，但我知道有些朋友很喜歡它。

[^2]: 還有一些額外的選用技能可以附加到核心技能上，像是管理 git worktrees 和測試驅動開發 (test-driven development)。

[^3]: Ghostty 是一個終端機模擬器，速度明顯比 macOS 預設的 Terminal 快——一旦你用過它，預設 Terminal 應用程式的延遲感就再也回不去了。

[^4]: 我也試過 Claude Code 原生的 Voice Mode，但發現轉錄效果沒那麼好。此外，因為我在桌面和手機上的所有其他應用程式中都使用 WisprFlow，所以集中使用一組全域鍵盤快捷鍵會比較方便。

[^5]: 我最初買 Mac Mini 是為了設定 OpenClaw 和其他個人 AI 助理，但這成為了一個很棒的附帶好處。一台舊的 MacBook 也可以達到同樣的效果。

[^6]: Conductor、Cursor 和 Warp 似乎都收斂在同一個問題空間上。我確信我漏掉了一些名字。

[^7]: 給 Anthropic 團隊的回饋：如果在 Chat、Cowork 和 Code 之間有更無縫的體驗會更好。

[^8]: 在撰寫本文時，Superset 有支援遠端連線的 alpha 版本，Codex 的桌面應用程式也是。Claude 在過去幾週內修復了其桌面應用程式的一堆 bug。

[^9]: 用 AI 來開發產品確實讓我對軟體和軟體工程的實際意義產生了一些存在主義式的疑問。在未來的文章中會分享更多關於這點的想法。