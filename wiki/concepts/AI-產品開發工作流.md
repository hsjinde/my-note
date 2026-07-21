---
title: AI 產品開發工作流
tags: [AI, 開發工作流, Claude-Code, skills, 產品]
updated: 2026-07-21
source_count: 1
---

# AI 產品開發工作流

一人團隊用 AI 打造產品、估計生產力提升 10 倍的做法（George Xing）。三項互相加乘的關鍵：**代理技能框架、測試自動化、遠端優先且符合人體工學的流程**。

## 1. 用 `/superpowers` 結構化開發

痛點：純靠 Plan Mode「丟需求 → 討論 → 執行」，簡單功能有效，稍複雜就碰運氣——難判斷是計畫不夠細、執行偏離、還是兩者。

[/superpowers](https://github.com/obra/superpowers)（Jesse Vincent 開發，Anthropic 官方市場可裝）綁定四項技能，對應傳統產品開發階段，可單用或串聯：

| 階段 | 技能 | 做什麼 |
|---|---|---|
| **腦力激盪** | brainstorming | 把半成品點子變結構化 PRD；反問澄清、找盲點與矛盾；前端任務還生 wireframe。像同時有 PM + 設計師 |
| **撰寫計畫** | writing-plans | 把規格變詳細實作計畫（Markdown，任務拆成 checkbox + 指示 + 檔案結構 + LOC 估計） |
| **執行計畫** | executing-plans | 讀計畫寫扣；可序列或**平行**（subagent-driven development）；自動判相依、把簡單任務交給輕量模型（Sonnet/Haiku 代 Opus） |
| **程式碼審查** | code-review | 抓並修 bug、標記沒照計畫實作處、找「資深工程師會看出來」的問題；可與執行串聯自動修到全綠 |

> 核心洞見：**AI 便宜的時代，「寫程式前」的準備反而更重要**。認真做腦力激盪、審視規格、把關計畫，最終更快且品質更好。產品思維（釐清問題、了解使用者、jobs-to-be-done）仍不可少，只是以 AI 輔助、高度濃縮地完成。一個**強制你遵循最佳實踐**的技能框架帶來天壤之別。

## 2. 測試自動化 + 跨模型審查

- 結構化框架仍會漏 bug（重疊的側邊欄讓按鈕點不到、後端 race condition）——這類難靠規格裡的測試計畫抓出。
- **引入 Codex 交叉審查**：Codex 邏輯與嚴謹度更好、更精準聽指令（雖較不知變通）。讓 Claude 主寫、Codex 透過 `codex:rescue` 審查實作計畫與程式碼；Claude 修 → 重審，循環 3–4 次到問題清空。額外審查花時間（有時 >30 分）、偶爾吹毛求疵，但通常能抓出 Claude 漏掉的真 bug，後端與資料密集流程提升最明顯。

## 3. 遠端優先、符合人體工學

執行階段短則 10 分、長則數小時，最適合離開電腦。設計成可隨時放手（散步、泡咖啡），讓寫軟體更有效率也更有樂趣。

## 相關概念

- [[自製-Claude-Code-Skills]] — 同樣的「skill 強制最佳實踐」思路（如 ui-fix-verify 的不可跳過驗證）
- [[Claude-Code-Skills]] — skills 生態總覽
- [[Claude-Code]] — 主要開發 agent

## 來源

- [[Clippings/AI 開發工具/How I build with AI as a 1-person product team]]（web clipping，George Xing）
