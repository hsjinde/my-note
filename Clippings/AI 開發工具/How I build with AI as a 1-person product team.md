---
title: "我如何以一人團隊之姿，運用 AI 打造產品"
source: "https://georgexing.substack.com/p/how-i-build-with-ai-as-a-1-person?just_subscribed=true"
author:
  - "[[George Xing]]"
published: 2026-05-07
created: 2026-06-18
description: "這是我用來讓產品品質更好、開發速度快上 10 倍的工具與技巧"
tags:
  - "clippings"
---
過去幾個月來，我嘗試了各種不同的 AI 寫程式工具和技巧，目的是想幫我的個人專案建立一套流暢的工作流程。身為一人團隊，我的目標是打造出一個系統，讓寫軟體這件事變得更有效率、更輕鬆，而且充滿樂趣。雖然隨著新模型、新功能和新工具不斷推出，這個系統總有進步空間，但比起剛起步時，我的工作流程已經有了突飛猛進的改善。

真正帶來改變的關鍵有三個：使用「代理技能框架 (agent skills framework)」、建立「測試自動化」，以及設計一套「遠端優先且符合人體工學」的工作流程。這三項改變帶來的生產力提升是會互相加乘的。整體算下來，我估計這套做法不僅讓我的生產力飆升了至少 10 倍，做出來的產品品質也比以前好上許多。

## 1. 透過 `/superpowers` 進行開發

剛開始用 Claude Code 開發個人應用程式時，我非常依賴它的「計畫模式 (Plan Mode)」。我的做法通常是：開一個使用最新 Opus 模型的全新對話，按下 Shift+tab 進入計畫模式，然後把所有我覺得有幫助的產品概念和技術背景一股腦地丟進去。接著，我會跟 AI 反覆討論、修改，直到計畫看起來沒問題，再按下執行鍵，看著它自動寫扣。

這種做法對簡單的功能很有效，但只要遇到稍微複雜一點的需求，結果往往碰運氣。我開發過一款名叫 LemmeCook 的烹飪 App，裡面包含了新手教學、餐點推薦邏輯、食譜匯入以及食材庫存管理，這些功能之間有著微妙的互動。每當這些互動變得關鍵時，Claude Code 總是會做出一些不符合直覺的產品和使用者體驗 (UX) 決策。

我實在很難判斷，問題到底出在計畫寫得不夠詳細、執行時偏離了計畫，還是兩者都有。我發現自己常常得花好幾個小時修 bug 和處理預期外的狀況。這不僅讓人心力交瘁，也抹煞了寫程式的樂趣。

真正讓我豁然開朗的轉捩點，是開始使用 [/superpowers](https://github.com/obra/superpowers)。這是一個由 Jesse Vincent 開發的 Claude 外掛，你可以很輕易地從 Anthropic 官方的外掛市場安裝。這個套件 [^1] 綁定了四項核心技能，剛好對應了傳統產品開發的幾個階段：腦力激盪 (brainstorming)、撰寫計畫 (writing-plans)、執行計畫 (executing-plans) 以及程式碼審查 (code-review) [^2]。這四項技能可以分開單獨使用，也可以串聯起來一口氣執行。

### 腦力激盪 (brainstorming)

腦力激盪技能可以幫你把腦中的點子轉化為結構化的產品需求文件 (PRD)。當我有個點子——通常只是個「這功能大概要怎麼運作」的半成品想法時——我會把這些想法一股腦地倒給 Claude，剩下的就交給腦力激盪技能。它會先總結它聽到的內容，接著問我一些問題來釐清細節，並幫我找出我沒考慮到的產品盲點或矛盾。如果遇到前端的任務，它甚至會生成線框圖 (wireframes)，讓我在瀏覽器裡直接決定視覺和 UX 的走向。這感覺就像是同時擁有一個能跟你討論的 PM (產品經理) 以及一位設計師。

身為開發者，我非常享受這個階段。我發現，只要有選項讓我選（尤其是搭配視覺畫面的時候），回答問題遠比自己從零開始想問題和答案來得容易多了。此外，我也能掌控整個流程，並加入自己的品味：我可以引導 Claude 探索更多可能性、發揮創意，或是請它快點收斂，找出一個「夠好」的解決方案就行。

![/superpowers brainstorming](https://substackcdn.com/image/fetch/$s_!CHW_!,w_720,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb0d58a81-1d89-455a-b714-65d08eee8963_1432x1182.png)

/superpowers 會引導你進行互動式的腦力激盪，幫你做出關鍵的產品與設計決策。

### 撰寫計畫 (writing-plans)

當腦力激盪階段產出了我滿意的產品規格後，「撰寫計畫」技能就會把這份規格變成詳細的實作計畫。這份計畫會是一份 Markdown 文件，裡面把任務拆解成一個個獨立的項目，每個任務都附有詳細的指示。以下是它為我們剛剛討論出來的規格所寫的計畫片段：

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

這時候，我通常會花個幾分鐘，快速確認一下整體的計畫和任務合不合理。我不會花時間去細看每個任務的程式碼細節。不過，我會試探性地問 Claude 一些問題，看看這份計畫有沒有考慮到某些極端情況 (edge cases)，並確認它做出來的東西會符合我預期的行為。

舉例來說，下面這個例子裡，我問 Claude 這個計畫有沒有確保所有推薦的餐點都不一樣（結果它沒有考慮到），然後我給了它一個修正建議：

![](https://substackcdn.com/image/fetch/$s_!GIop!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe01d74af-d629-4c4f-bd9e-b40f796f4241_1642x1160.png)

當我在計畫中發現不符合預期的產品行為時，Claude 提出了一個修正方案。

### 執行計畫 (executing-plans)

只要實作計畫確認沒問題，「執行計畫」技能就會開始讀取它並著手寫程式碼。這時我通常有兩個選擇：一個任務一個任務慢慢執行（序列執行），或是為每個任務開一個全新的子代理（平行處理），這會用到另一個名叫「子代理驅動開發 (subagent-driven development)」的綁定技能。為了求快，我通常會選平行處理。Claude 會自動判斷哪些任務可以平行處理、哪些有先後相依性，甚至還能決定哪些簡單的任務可以交給比較輕量級的模型（例如用 Sonnet 或 Haiku 代替 Opus）。

根據我的經驗，執行的過程通常最花時間，短則 10 分鐘，長則好幾個小時，端看任務的複雜度。這時候最適合離開電腦休息一下。我通常會在按下執行鍵後，去散個步或泡杯咖啡。

![](https://substackcdn.com/image/fetch/$s_!kbHr!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffe8687a8-ea7b-412d-b8a0-e6ccf8fee382_1638x1048.png)

/superpowers 提供不同的執行模式讓你選擇。

### 程式碼審查 (code-review)

第四項技能是程式碼審查。它會在程式碼寫完後自動執行檢查。它會抓出並修復 bug、標記出沒有照著計畫實作的地方，還能找出那些「你會希望資深工程師幫你看出來」的問題。

這個步驟還可以跟「執行計畫」串聯在一起，讓 Claude 自動進行審查並修改，直到所有被抓出來的問題都解決為止，整個過程完全不需要我操心。

透過 superpowers 進行開發，讓我深刻體會到：雖然有了 AI，但我們還是得親自釐清問題、了解使用者、分析使用情境，以及確定產品要完成的任務 (jobs-to-be-done)。在使用 AI 代理開發產品時，「產品思維」依然不可或缺，只是現在我們能以 AI 輔助、高度濃縮的方式來完成這些事。而擁有一個能**強制你遵循這些最佳實踐**的代理技能框架，真的會帶來天壤之別。

事實上，在現在這個生成程式碼既便宜又簡單的時代，「寫程式前」的準備工作反而變得**更**重要了。雖然直接跳下去寫扣、再慢慢修改產出看起來很誘人，但我一次又一次的經驗證明：在專案一開始認真做好腦力激盪、仔細審視規格，並嚴格把關實作計畫，最終不僅能更快拿到滿意的成果，產品品質也會好上許多。

![](https://substackcdn.com/image/fetch/$s_!iNdU!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4c5a5d27-f0c0-4902-8855-1a08aee4608b_1979x807.png)

在前期花時間把產品定義清楚，能為你省下大量的人力和開發時間！

## 2. 測試自動化

雖然使用了 superpowers 這類結構化的產品開發框架，讓我的產出品質大幅提升，但它並非完美無缺。舉例來說，Claude Code 在開發我的烹飪 App 時，有時會生出會互相重疊的側邊欄或彈出視窗，導致畫面上的按鈕點不到；在後端，它有時會沒考慮到競態條件 (race conditions)，導致資料載入錯誤或是根本載入不出來。這類型的 bug 很難單靠產品規格裡的測試計畫抓出來。

### 引入 Codex 進行審查

雖然我比較習慣把 Claude Code 當作主要的寫程式助理，但我也試過 Codex，並發現它在邏輯和嚴謹度上表現得更好。相較於 Claude Code，Codex 比較不知變通，但它能更精準地聽從指示。所以我開始讓 Codex 幫忙檢查 Claude 寫出的大量程式碼。

三月底時，OpenAI 推出了給 Claude Code 用的 Codex 外掛，我決定把它整合進我的工作流程中。我會讓 Codex 透過 `codex:rescue` 技能，來審查 Claude 寫好的實作計畫。在執行階段結束後，Claude Code 會呼叫 Codex 作為子代理來檢查程式碼。接著，Claude 會去修復 Codex 抓出來的問題，然後再重新提交審查。這個循環會不斷重複直到問題都解決為止，有時甚至需要來回修改 3 到 4 次。

雖然這層額外的審查會花上不少時間（有時要等超過 30 分鐘），而且 Codex 有時會有點吹毛求疵（像是把小瑕疵標記成嚴重錯誤），但它通常都能抓出 Claude Code 漏掉的真實 bug 和邏輯不一致的地方。我發現，加入 Codex 的審查，讓我的產品品質有了顯著的提升，尤其是在後端功能和資料處理密集的流程上。

![](https://substackcdn.com/image/fetch/$s_!uiLD!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbbc55a87-85ce-4fbf-a8df-303e62a38d85_1644x1060.png)

Codex 標記出 Claude 實作中的問題，接著 Claude 就會進行修復。

### 實作審查 (implementation-review) 技能

不過，就算程式碼審查做得再好，也無法取代直接測試真實使用者行為的「端到端測試 (end-to-end testing)」。在 Stripe 工作時，產品上線前最重要的步驟之一就是「實作審查」。會有受過訓練的人員親自測試你的產品，並整理出一份問題清單（包含不修好就不能上線的致命錯誤）給團隊去解決。對於流程複雜的新產品來說，這來來回回可能要花上好幾個禮拜，但這也是確保產品品質的關鍵步驟。

為了模擬這個過程，我寫了一個專屬的 Claude 技能：`implementation-review`。在開發手機 App 時，它會跟我一起生出一組測試情境。接著，它會像真人測試員一樣跑測試流程：截圖看目前的畫面、分析畫面、記錄發現的問題、決定下一步要點什麼，然後再截一張圖看畫面的變化。最後，它會產出一份按照嚴重程度排序的測試報告。

在底層，這個技能會根據產品類型使用不同的工具。測試手機 App 時，它會用到 Sentry 維護的 `XcodeBuildMCP` 伺服器，讓 Claude 可以建置並測試 iOS 應用程式（它把很多複雜的 AppleScript 和 Xcode 模擬器指令包裝成 AI 容易操作的介面）。測試網頁應用程式時，它則會使用 `playwright-mcp` 來操作瀏覽器。

把這些流程全部串起來後，這套系統變得非常強大：

1. 每一份實作計畫和每一行程式碼，都會經過 Codex 的審查。
2. 每個關鍵的使用者情境，都會由 `implementation-review` 技能自動跑過一遍。
3. 測試抓出來的所有問題，都會送回給 Claude 修復（修復完後一樣要再過一次 Codex 的程式碼審查）。

雖然加入實作審查會讓開發時間增加好幾個小時，但只要設定好，它就會自動在背景跑，這筆時間投資絕對划算。

![](https://substackcdn.com/image/fetch/$s_!Uuyn!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e5884e6-ef77-446a-9ae0-7e404fc8f496_1543x1680.png)

結合實作審查與 Codex 審查的自動化測試循環，能幫你省下好幾天手動抓 bug 的時間。

## 3. 遠端優先且符合人體工學的開發環境

自從開始使用 superpowers 和自動化測試後，我跟 Claude 的合作模式改變了：從以前那種一直盯著螢幕、不斷互動的開發方式，變成了先進行短暫而密集的腦力激盪跟產品定義，接著就放給 Claude 自己跑好幾個小時。這讓我的工作變得非常有效率，因為我可以在睡前把任務丟給它，隔天早上起床直接收割成果。

但這也衍生出一些新問題：

1. **打字太慢了**。我們都知道，餵給 AI 的脈絡越詳細，出來的結果就越好。但在我新的工作流程裡，我要講的話太多了：講得越詳細，腦力激盪的效果越好；規格越清楚，最後寫出來的程式就越棒。但我總不可能每做一個新功能，都要打一長篇像論文一樣的規格書吧？
2. **監控與維持連線**。為了讓 AI 跑好幾個小時的任務不中斷，我的筆電必須一直開著。如果我人在外面工作，這就很麻煩了。有時候任務跑到一半需要我做決定，它就會在那邊傻等好幾個小時，直到我回到電腦前。我需要一個能在移動中隨時查看進度並下達指令的方法。
3. **管理多工的 AI 代理**。在等待 Claude 寫扣的空檔，我開始同時處理多個專案，甚至同時開發同一個專案的不同功能。要在終端機 (Ghostty) 裡開一堆分頁來管理這些任務，簡直是一場災難。[^3]

為了解決這些「開發者人體工學」的新問題，我找到了幾個不錯的方法：

### 語音輸入取代打字

我開始習慣用語音輸入工具 WisprFlow 來說出我的想法，而不是用打字的。這對我腦力激盪的品質和速度有著驚人的提升。[^4] 我說話輸出的資訊量至少是打字的 3 到 4 倍，尤其是在用手機操作的時候。雖然我講話有時會跳躍思考、文法也不完全正確，但其實一點也不影響：現在的頂尖 AI 模型非常聰明，很懂得從一堆廢話中抓出重點。

過去幾個月裡，不管是在電腦還是手機上，我跟 Claude Code 的溝通絕大多數都是用 WisprFlow 語音輸入的。給大家一個小建議：花個幾分鐘把常用的專有名詞（例如 Claude、Tailscale、Codex）加到它的字典裡，這能幫你省下很多後續修改錯字的麻煩。

![](https://substackcdn.com/image/fetch/$s_!7VSq!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb49a9247-a0e1-4102-ade2-1c8507ea9f7b_1072x446.png)

在家工作的意外好處之一：你可以大聲地用語音輸入，不用像在辦公室一樣像個怪人偷偷摸摸地碎碎念 :)

### 遠端開發設定

為了解決連線中斷的問題，我把所有的開發環境都移到家裡的一台 Mac Mini 上。[^5] 我有考慮過租雲端虛擬機 (VM)，但因為我要開發 iOS App，必須使用 macOS 系統，而便宜的雲端機器通常都只跑 Linux。

我參考了[這篇教學](https://kareemf.com/on-agentic-coding-from-anywhere)，並設定了 tmux（用來維持終端機對話不中斷）、Tailscale（讓我的手機、筆電和 Mac Mini 之間有穩定的內部網路），以及 [Claude 的遠端控制功能](https://code.claude.com/docs/en/remote-control)（用來同步不同裝置上的 Claude 對話）。只要你有一台 Mac，這個設定非常快就能搞定。tmux 和 Tailscale 都是免費的，我另外花了 20 美金買了 Blink 這個 iOS App，讓我在手機上也能有終端機可以直接 SSH 連回電腦。這一步不是絕對必要，但我希望能有個備案，萬一遠端控制斷線時，我還能自己連回去重啟服務。

現在，不管我是在家裡用筆電，還是在外面的咖啡廳，我都是透過 SSH 連回我的 Mac Mini 工作。當我需要蓋上筆電移動時，AI 還是會在遠端繼續跑。在通勤的車上，我也能隨時打開手機上的 Claude App 查看進度。

偶爾遇到系統權限的問題時，我就會用筆電內建的「螢幕分享」功能（如果只帶手機，就用 RVNC Viewer App），直接連回 Mac Mini 操作畫面。

![Claude Code sessions via remote control](https://substackcdn.com/image/fetch/$s_!KeGR!,w_720,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe8da3244-9056-4c6a-ab35-c20a142d5ca4_1810x1372.png)

透過遠端控制功能，你可以同時在電腦版、手機版和文字介面 (TUI) 上，同步看到同一個 Claude Code 對話的進度。

### 專為 AI 打造的開發環境

過去半年來，市場上出現了一整批全新的工具，專門用來解決這類「AI 輔助開發」的工作流程。

為了解決多工管理的問題，我試用了幾款工具：[^6]

- [Superset.sh](https://superset.sh/) 是一款主打「AI 代理原生」的終端機產品。它內建了 Github 和 Git worktree 的整合，讓你切換分支和開新功能非常方便。你還可以寫腳本讓它自動處理像設定 `.env` 檔案這種瑣碎的維運工作。它裡面直接內建了程式碼比對 (diff) 工具、Markdown 閱讀器和瀏覽器。當 Claude 跑一跑需要你確認時，它還會跳通知提醒你切換分頁。用它的好處是，它能完美支援 Claude Code 那套好用又流暢的文字介面 (TUI)。
- Claude 電腦版 (Desktop) 最近更新得很頻繁，現在不僅支援 SSH 連線，還支援多專案和平行對話，同時也整合了 Git worktree。我也很喜歡它把 Chat (聊天)、Cowork (協作) 和 Code (寫程式) 功能全部整合在同一個 App 裡，用快捷鍵切換超級順手：畢竟有些偏向研究或知識整理的工作，用一般的對話介面還是比較好用。[^7]
- [cmux](https://cmux.com/) 是一個開源的終端機工具，核心跟 Ghostty 一樣採用 libghostty 打造。它內建了垂直的標籤頁，也會提醒你哪個平行任務完成了。跟 Superset 不同，它只專注在把終端機體驗做到最好，而不是包山包海的開發工具。我喜歡 cmux 的原因是它速度飛快，而且它有[很棒的原生 SSH 支援](https://cmux.com/blog/cmux-ssh)，會自動幫你處理通訊埠轉發 (port forwarding) 的問題——這在腦力激盪時用來預覽網頁畫面非常實用。

我最後選擇了混合搭配的方式。我會用 MacBook 透過 SSH 連線到 Mac Mini，然後打開 Claude 桌面版使用；同時，我也會在終端機 (Ghostty) 裡保持同樣的對話開啟。我讓每個對話都加上 `/remote-control` 指令，這樣所有裝置和介面上的對話進度都會同步。這讓我不管是用電腦還是手機，都能隨時隨地監控進度、幫 AI 解除卡關或是調整方向。

![](https://substackcdn.com/image/fetch/$s_!62dn!,w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F751da239-6908-4089-b380-3577166c94a7_2120x1650.png)

我的遠端開發配置圖：把 Mac Mini 當作一台永遠開機的 AI 運算大腦。

在實際工作時，我比較喜歡對著終端機 (TUI) 講話下指令，因為它反應最快、最穩，而且通常是最早推出新功能的介面。但這種混合配置的優點是，當我想回頭看之前跑過的程式碼歷史，或是在不同對話間切換時，我可以用 Claude 桌面版滑滑鼠看，不用在終端機裡按方向鍵。桌面版在看規格書或 Markdown 文件時的排版也漂亮多了。另外，在桌面版複製貼上或傳圖片也順手得多：畢竟你沒辦法直接把一張圖片從你的電腦拖進遠端連線的終端機視窗裡。

當然，我這套設定還是有一些缺點。我覺得這反映了現在的產業現況：終端機、電腦版、手機版和網頁版之間的功能和體驗還沒有完全統一。我測試的這些產品都還很新，發展速度快到每個禮拜都有新功能。好消息是，這些工具成熟得非常快，[^8] 我很樂觀地認為，半年後這些小痛點都會被解決。也許到時候我們就會有一套更統一、更好用的標準配置（希望是不會被單一廠商綁死的！）。但就目前來說，我對現在這套工作流程已經相當滿意了。

## 結語

像 `/superpowers` 這樣的代理技能框架、自動化測試，以及遠端優先的無縫環境，這三個要素組合起來會產生強大的化學反應。代理框架讓我把敲鍵盤（或講話）的時間壓縮成最高效的衝刺；自動化測試讓 AI 能夠自主跑得更久，並交出更好的成果；而遠端優先的設定讓我隨時隨地都能檢查進度，把零碎的時間也利用到極致。

在不斷嘗試各種新工具和新方法的過程中，我越來越確信：**使用 AI 來開發產品，本身就是一門專業技能**，就跟軟體工程或產品管理一樣。雖然現在每個人只要花 [每個月 100 美金](https://georgexing.substack.com/p/all-you-need-is-a-claude-max-subscription)，就能用到最頂級的 AI 模型和開發工具，但能不能用這些工具做出好產品，結果卻是天差地遠。

就我的經驗來看，AI 開發者最重要的能力有兩個：(1) 在腦力激盪時，有能力做出具體的產品和設計決策，清楚定義你要做什麼；(2) 有能力清楚定義「成功的標準是什麼」，這樣你的 AI 代理才能帶著明確的目標，去執行「寫扣、測試、修正」的循環。在現階段，如果想做好這兩點，懂一些軟體工程的基礎（像是資料庫設計、API 規劃、Client/Server 架構）還是非常有幫助的。不過我預感，大概一年後，這些底層細節也會被工具給抽象化掉。到那個時候，我們剩下的核心競爭力，大概就只剩下對產品的品味與信念了。[^9]

想學會這些技能，最好的方法就是直接捲起袖子開始做——這世上沒有任何東西可以取代第一手的實戰經驗。因為工具和技術的演進實在太快了，靜態的教學（包括你現在看的這篇文章）很快就會過時。老實說，我今天分享的這套做法，幾個月前根本還做不到，我也完全預期它幾個月後就會變成舊時代的產物了。

最後我想說，身為一個開發者，能站在這個時代的風口浪尖上，真的讓人感到無比興奮又充滿力量；但同時，這種快到不可思議的變化速度也常常讓人感到頭暈目眩。我幾個月前學到的東西，現在感覺已經落伍了。每個禮拜我都會看到有人在嘗試更瘋狂的事——比如經營一家全自動化的公司，或是指揮成千上萬個 AI 代理組成的蜂群——光是想跟上這些新資訊，就已經像是一份全職工作了。

如果你也正在這條路上摸索，非常歡迎你跟我聯絡（我的聯絡方式在 About 頁面）。我很期待能跟大家交流、學習、互相切磋！

[^1]: Garry Tan 也有寫一個叫 [gstack](https://github.com/garrytan/gstack) 的外掛，解決的是同樣的問題。概念上是一樣的，只是實作方式不同。我自己還沒用過 gstack，但我知道有不少朋友很喜歡。

[^2]: 另外還有一些可以外掛上去的選擇性技能，像是用來管理 git worktrees 或是執行測試驅動開發 (TDD)。

[^3]: Ghostty 是一個終端機模擬器，它的速度比 Mac 內建的 Terminal 快非常多——只要你用過一次，就絕對無法忍受內建 Terminal 的那種卡頓感了。

[^4]: 我也有試過 Claude Code 內建的語音模式，但轉錄的精準度沒那麼好。再加上我不管在電腦還是手機上都習慣用 WisprFlow，能統一用同一組快捷鍵還是比較方便。

[^5]: 我一開始買 Mac Mini 其實是為了跑 OpenClaw 和其他地端的 AI 助理，沒想到意外促成了這套遠端開發環境。其實拿一台舊的 MacBook 也可以達到一樣的效果。

[^6]: 像是 Conductor、Cursor 和 Warp，現在好像都在朝同一個方向發展。我應該還漏講了很多其他的工具。

[^7]: 寫給 Anthropic 團隊的許願池：如果 Chat、Cowork 和 Code 這三個功能之間的切換體驗能更無縫一點就好了。

[^8]: 在我寫這篇文章的時候，Superset 已經推出了支援遠端連線的 alpha 測試版，Codex 的桌面版也是。Claude 也在過去幾週內修掉了一大堆桌面版的 bug。

[^9]: 說真的，用 AI 寫程式這件事，讓我開始對「軟體」和「軟體工程」的本質產生了存在主義式的疑問。這部分之後有機會再寫一篇文章跟大家分享。