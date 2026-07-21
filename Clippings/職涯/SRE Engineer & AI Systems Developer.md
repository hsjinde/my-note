---
title: "SRE Engineer & AI Systems Developer"
source: "https://core-pulse.19980803.xyz/"
author: "Ethan"
created: 2026-05-28
description: "Ethan - SRE 工程師 / AI 系統開發者。致力於 RNN 查詢優化研究與高可用架構設計，以 SRE 思維構建自動化與智能化的基礎設施。"
tags:
  - clippings
  - SRE
  - AI
  - Portfolio
---

# SRE Engineer & AI Systems Developer

## 👤 About Ethan
SRE 工程師與 AI 系統開發者。致力於 RNN 查詢優化研究與高可用架構設計，以 SRE 思維構建能夠自我修復的智能化基礎設施。

---

## 🛠️ Technical Arsenal (Skills & Infrastructure)

### Core Stack
- **Languages & Frontend:** React, TypeScript, Python, Go
- **Containers & Orchestration:** Docker, Kubernetes
- **Infrastructure as Code (IaC):** Terraform
- **Observability & Monitoring:** Prometheus, Grafana
- **Databases & Caching:** PostgreSQL, Redis
- **Proxy & CDN:** Nginx, Cloudflare

### Core Infrastructure & Security
- **Inference Platform:** 自建 LLM 推理後端 (OpenClaw AI)，安全運行於 Docker 環境，透過 Cloudflare Tunnel 暴露，並以 Zero Trust 機制安全存取與管理。
- **Hosting & CDN:** Cloudflare Pages
- **Compute:** RackNerd VPS

---

## 📊 System Metrics & Status

> [!success] **Production Status Overview**
> - **System Uptime (Last 90 days):** `99.97%`
> - **Web Performance (Lighthouse Metrics):**
>   - **LCP (Largest Contentful Paint):** `0.8s`
>   - **FID (First Input Delay):** `<1ms`
>   - **CLS (Cumulative Layout Shift):** `0.01`

### CI / CD Pipeline
- [x] **Push** (Trigger)
- [x] **Lint / Test** (PyTest / Static Analysis)
- [x] **Build** (Docker images / frontend assets)
- [x] **CF Deploy** (Cloudflare Pages deployment)
- [x] **CDN Purge** (Edge cache invalidation)

---

## 🚀 Selected Projects (Projects that matter)

### 1. RNN SPARQL Optimizer
> [!abstract] **Research Paper · IEEE Published**
> 發表於 IEEE Xplore 的研究論文。開發了一種基於遞迴神經網絡 (RNN) 的 SPARQL 查詢優化模型，顯著提升語義網大數據環境下的查詢效能。
> - **Tags:** #RNN #SPARQL #Deep-Learning #Semantic-Web #Performance-Optimization
> - **Paper Link:** [Read Paper on IEEE Xplore](https://ieeexplore.ieee.org/document/10230082)
> 
> | Attribute | Details |
> | :--- | :--- |
> | **Problem** | 傳統的 SPARQL 查詢優化器在處理複雜、大規模的圖形數據時，難以準確預估路徑權重，導致查詢延遲過高。 |
> | **Solution** | 引入 LSTM 網絡學習查詢語法與執行路徑的特徵，利用 RNN 的序列處理能力動態調整查詢執行計畫。 |
> | **Result** | 實驗證明在多個基準數據集上，查詢反應時間平均降低了 **35%**，並成功發表於 IEEE 學術期刊。 |

### 2. OpenClaw
> [!info] **AI Inference Platform**
> 自建 LLM 推理後端，運行於 RackNerd VPS 的 Docker 環境，透過 Cloudflare Tunnel 安全暴露，並以 Zero Trust 保護管理介面。
> - **Tags:** #Docker #Cloudflare-Tunnel #Zero-Trust #FastAPI #Ollama
> 
> | Attribute | Details |
> | :--- | :--- |
> | **Problem** | 商業 AI API 成本高且無法客製化，隱私敏感資料不能上傳雲端。 |
> | **Solution** | 在 VPS 部署 Ollama + FastAPI，以 Cloudflare Tunnel 替代 Nginx 反向代理，Zero Trust 控制存取。 |
> | **Result** | 月費降低 **78%**，延遲 **< 200ms**，資料完全自主控制。 |

### 3. SRE DevOps Lab
> [!tip] **Automation & Observability**
> 系統化 SRE 學習實驗室，實踐 Python 自動化腳本、PyTest 單元測試、Prometheus 監控與 Grafana 儀表板建置。
> - **Tags:** #Python #Prometheus #Grafana #GitHub-Actions #K8s
> - **GitHub Repository:** [View all on GitHub](https://github.com/hsjinde)
> 
> | Attribute | Details |
> | :--- | :--- |
> | **Problem** | 缺乏實際的 SRE 訓練環境，理論與實務存在落差。 |
> | **Solution** | 建立完整的 DevOps pipeline，從程式碼到部署全自動化，整合 Alertmanager 實現 on-call 流程。 |
> | **Result** | MTTR 降低 **60%**，自動化覆蓋率達 **90%**，成功構建 GitHub Portfolio。 |

---

## 📝 Technical Notes (SRE 與開發筆記)
紀錄系統架構設計、演算法解題思維以及基礎設施自動化的實戰經驗。

### 📚 Cloudflare D1 資料庫上線測試
- **Type:** Tutorial | **Reading Time:** 5 min | **Date:** 2026-04-28
- **Tags:** #Database #Cloudflare
- **Summary:** 測試 Edge 邊緣資料庫是否成功連接與運作。

### 📚 從零打造 SRE 現代化網站：Serverless 架構演進史
- **Type:** Tutorial | **Reading Time:** 5 min | **Date:** 2026-04-28
- **Tags:** #SRE #Cloudflare #Serverless
- **Summary:** 紀錄 CORE PULSE 專案的架構演進史。從打造 Apple-style 靜態佈局、手寫 GitHub Actions CI/CD 管線，到整合 Cloudflare R2 圖床與 D1 邊緣資料庫，完整展示現代化 Serverless 架構的強大威力。

### 📚 KeyLogger-Server
- **Type:** Tutorial | **Reading Time:** 5 min | **Date:** 2026-04-28
- **Tags:** #Cpp #Windows #Systems-Security
- **Summary:** C++ windows 環境下的系統鍵盤記錄服務器設計與開發。
