---
title: Ethan
tags: [人物, SRE, AI, 個人]
updated: 2026-06-04
source_count: 1
---

# Ethan

SRE 工程師與 AI 系統開發者。致力於 RNN 查詢優化研究與高可用架構設計。

## 技術棧

| 領域 | 技術 |
|---|---|
| 語言 | React, TypeScript, Python, Go |
| 容器與編排 | Docker, Kubernetes |
| IaC | Terraform |
| 觀測性 | Prometheus, Grafana |
| 資料庫 | PostgreSQL, Redis |
| Proxy/CDN | Nginx, Cloudflare |

## 基礎設施

- 自建 LLM 推理後端（OpenClaw AI），Docker + Cloudflare Tunnel + Zero Trust
- Hosting：Cloudflare Pages
- Compute：RackNerd VPS

## 專案

### RNN SPARQL Optimizer
IEEE 發表的研究論文。基於 RNN 的 SPARQL 查詢優化模型，查詢反應時間平均降低 **35%**。
- [[https://ieeexplore.ieee.org/document/10230082|IEEE Xplore]]

### OpenClaw
自建 LLM 推理後端，VPS 上 Docker 部署，Cloudflare Tunnel 安全暴露。
- 月費降低 **78%**，延遲小於 **200ms**

### SRE DevOps Lab
SRE 學習實驗室：Python 自動化、PyTest 測試、Prometheus/Grafana 監控。
- 完整 DevOps pipeline，MTTR 降低 **60%**，自動化覆蓋率 **90%**

## 系統指標

- Uptime（90 天）：**99.97%**
- LCP：**0.8s**，FID：**<1ms**，CLS：**0.01**

## 相關概念

- [[SRE-學習路徑]] — SRE 學習路線

## 來源

- [[SRE Engineer & AI Systems Developer]]（Clippings，2026-05）
