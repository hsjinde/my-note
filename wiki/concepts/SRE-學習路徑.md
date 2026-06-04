---
title: SRE 學習路徑
tags: [SRE, 學習路線, 技能樹]
updated: 2026-06-04
source_count: 1
---

# SRE 學習路徑

6 階段 Site Reliability Engineer 完整成長路線，從基礎建設到領導策略。

## 第一階段：基礎建設 Foundation

打好底層基礎，理解電腦系統運作。

| 主題 | 涵蓋 |
|---|---|
| Linux 系統管理 | 檔案系統與權限、Process/Signal/Systemd、網路設定與排查、Shell Scripting、cron/systemd timers |
| 網路基礎 | TCP/IP/DNS/HTTP(S)、負載均衡、防火牆與 Proxy/Nginx、tcpdump/Wireshark、VPN/TLS |
| 程式語言 | Python（自動化腳本）、Go（SRE 首選）、資料結構與演算法、測試與除錯、YAML/JSON/TOML |
| 雲端基礎 | AWS/GCP/Azure 概念、運算/儲存/網路服務、IAM 與計費、CLI 工具、基礎架構設計模式 |
| 版本控制 | Git 工作流程、Pull Request/Code Review、Semantic Versioning、文件撰寫 |

**里程碑：** 能獨立配置 Linux 伺服器、撰寫自動化腳本、理解請求完整路徑、部署第一個雲端服務

---

## 第二階段：核心維運 Core Operations

具備線上服務維運的實戰能力。

| 主題 | 涵蓋 |
|---|---|
| 監控與觀測性 | Metric/Log/Trace 三大支柱、Prometheus+Grafana、ELK/Loki、儀表板設計、Alerting 規則 |
| CI/CD 與部署 | GitHub Actions/GitLab CI、Helm Chart、部署策略（藍綠/金絲雀/滾動）、回滾機制、Feature Flag |
| 容器與編排 | Dockerfile 最佳實踐、Docker Compose、Kubernetes 基礎、Service/Ingress/NetworkPolicy、資源管理 |
| 資料庫維運 | SQL 調優、備份還原、Connection Pooling、Redis/RabbitMQ/Kafka 基礎、資料遷移 |
| On-Call 實戰 | 事件分級、故障排查流程、Postmortem 撰寫 |

**里程碑：** 能獨立處理線上事故、建置監控系統、管理 CI/CD 管線、操作 K8s 集群

---

## 第三階段：SRE 核心實踐 Core SRE

掌握 SRE 核心方法論，用數據驅動可靠性。

| 主題 | 涵蓋 |
|---|---|
| SLI/SLO/Error Budget | 定義可靠性指標、目標設定與協商、Error Budget 決策機制、Burn Rate Alerting、多層次 SLO |
| 事件管理 | Incident Command 架構、分級回應（P0-P3）、溝通協調、無責事後檢討、Action Item 追蹤 |
| 容量與效能 | 容量規劃、負載測試（k6/Locust）、效能瓶頸分析、水平 vs 垂直擴展、Auto Scaling/FinOps |
| 減少瑣事 (Toil) | 瑣事定義與追蹤、自動化機會識別、自助服務工具、Runbook 標準化、50% 工時規則 |

---

## 第四階段：進階可靠性 Advanced Reliability

設計能抵禦故障的彈性系統。

| 主題 | 涵蓋 |
|---|---|
| 混沌工程 | 故障注入、Chaos Mesh/Litmus、Game Day 演練、爆炸半徑控制 |
| 容錯設計模式 | Circuit Breaker、Bulkhead/Rate Limiting、Retry with Backoff、超時與取消傳播、降級策略 |
| 分散式系統 | CAP 定理、Raft/Paxos、Saga/2PC 分散式交易、Istio/Linkerd 服務網格、Kafka 事件驅動 |
| DevSecOps | SAST/DAST/SCA 掃描、Container Image 安全、Vault Secrets 管理、SBOM 供應鏈安全、Zero Trust |

---

## 第五階段：自動化與平台工程 Automation & Platform

從維運者轉變為平台建設者。

| 主題 | 涵蓋 |
|---|---|
| Infrastructure as Code | Terraform/Pulumi/CDK、不可變基礎設施、Ansible 配置管理、GitOps（ArgoCD/Flux）、Policy as Code（OPA/Kyverno）|
| Kubernetes 進階 | Operator 開發、Custom Resource Definition、排程策略、多集群管理、Karpenter/KEDA 成本優化 |
| 平台工程 | 內部開發者平台（IDP）、Backstage/Port、開發者體驗、服務目錄、Golden Path |
| 進階觀測性 | OpenTelemetry 標準化、分散式追蹤、關聯性分析、自適應取樣、觀測成本控制 |

---

## 第六階段：領導與策略 Staff+ Leadership

具備技術策略與組織影響力。

| 主題 | 涵蓋 |
|---|---|
| 技術策略 | ADR 架構決策取捨、技術債管理、現代化遷移規劃、可靠性策略藍圖、成本效益分析 |
| 組織文化 | 可靠性文化導入、跨團隊協作、當責不責備文化、SRE 與 Dev 團隊關係、變革管理 |
| 指導與影響力 | Mentoring、RFC/設計文件、技術演講、面試招聘、跨部門影響力 |
| 前瞻視野 | 新技術評估、FinOps 成本治理、AI/ML Ops 在 SRE 的應用、邊緣運算、GreenOps |

---

## 相關概念

- [[Ethan]] — 個人技術檔案與專案實績

## 來源

- [[SRE 學習路徑圖.canvas]]（個人學習，2026-06）
- [[SRE Engineer & AI Systems Developer]]（Clippings，2026-05）
