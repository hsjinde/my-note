---
title: Matt Pocock Skills for Real Engineers
type: concept
tags:
  - ai/agent
  - ai/workflow
  - developer-tools
updated: '2026-07-24'
---

# Matt Pocock Skills for Real Engineers

**Matt Pocock Skills for Real Engineers**（開源庫名稱 `mattpocock/skills`）是一套專為 AI Agent（如 Claude Code、Cursor、Antigravity 等）設計的**標準工程化作業流程（Procedural Workflows）技能庫**。

其核心精神在於**擺脫「憑感覺寫程式（Vibe Coding）」**，透過規範化、可重複執行的 SOP（Standard Operating Procedures），讓 AI Agent 具備資深工程師的嚴謹思維與軟體工程規範。

---

## 💡 核心設計理念

1. **Procedural Workflows (流程化作業)**：不給 AI 模糊或隨意的 Prompt，而是透過 `SKILL.md` 定義明確的執行步驟，引導 Agent 完成從需求規劃、架構審查到測試與重構的標準流程。
2. **End-to-End Lifecycle (完整開發生命週期)**：將軟體開發拆解成可鏈結的獨立技能（Skill Chaining），形成一個連續且互補的開發循環。
3. **Engineering Rigor (工程嚴謹度)**：強制導入 TDD（測試驅動開發）、PRD 審查與架構防禦機制，降低 AI 產生幻覺（Hallucination）或隨意破壞現有程式碼結構的風險。

---

## 🛠️ 主要技能與分類

### 1. 需求與架構規劃 (Planning & Design)
*   **`/to-prd`**：將口述想法或零散需求提煉並轉化為結構化的 PRD（Product Requirements Document）。
*   **`/to-tickets`**：將 PRD 自動拆解為可執行的 Task / Issue，並標註明確的驗收條件（Acceptance Criteria）。
*   **`/grill-me` / `/grill-with-docs`**：在編寫程式碼前，AI 會扮演嚴苛的架構師，針對設計決策、邊界條件與專案既有文件對開發者進行質詢（Grilling），提早排除潛在設計盲點。

### 2. 開發與品質維護 (Development & Quality)
*   **`/tdd`**：強制執行 Red-Green-Refactor 循環，堅持「先寫測試再編寫實作」的測試驅動開發原則。
*   **`/improve-codebase-architecture`**：針對既有程式碼進行架構重構，提高模組化程度、降低耦合度並增強可測試性。
*   **`/diagnosing-bugs`**：結構化的 Bug 排查 SOP，要求找出根本原因（Root Cause）前禁止盲目修補症狀。
*   **`git-guardrails`**：Git 操作防護網，防止 AI 執行危險命令（如 `git push --force` 或破壞性的 `git reset`）。

### 3. 初始化與設定 (Setup & Tooling)
*   **`/setup-matt-pocock-skills`**：專案初始化的首選命令，自動將 Agent 設定與團隊的 Issue 追蹤工具（如 GitHub Issues、Linear）及代碼規範同步。

---

## 📦 安裝與整合方式

*   **CLI 直接安裝**：
    ```bash
    npx skills@latest add mattpocock/skills
    ```
*   **Claude Code Plugin 市集**：
    ```text
    /plugin marketplace add mattpocock/skills
    ```
*   **初始化專案配置**：
    安裝完成後在 Agent 內執行 `/setup-matt-pocock-skills` 即可進行專案環境適配。

---

## 🔗 相關概念與連結
*   [[Claude-Code-Skills]] — Claude Code 技能機制與標準
*   [[AI-產品開發工作流]] — AI 輔助開發的流程與實踐
*   [[Matt-Pocock]] — 專案作者與 AI 工程工作流推廣者
