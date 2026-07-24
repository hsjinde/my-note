---
title: PA440-FW-data-configurator
tags: [專案, Python, PaloAlto, 資安自動化, CTI]
updated: 2026-07-24
source_count: 1
---

# PA440-FW-data-configurator

Palo Alto PA-440 防火牆 CTI（Cyber Threat Intelligence 威脅情資）自動化設定工具，將 STIX/JSON 與 Excel 多 Sheet 情資批次清洗還原後，經由 Paramiko SSH 注入防火牆組態中。

## 核心功能

1. **情資自動解析與清洗 (Defang Neutralization)**：支援 JSON/STIX 與 Excel 多工作表，自動識別 IP、FQDN Domain、Malicious URL 及 File Hash，並修復避險字元（如 `hxxp` $\rightarrow$ `http`、`[.]` $\rightarrow$ `.`）。
2. **Palo Alto CLI 自動化**：啟用 CLI `scripting-mode`，避開分頁與控制碼卡死。
3. **大批次 Chunking 寫入**：Address 物件每 200 筆批次下發；Address Group 與 Custom URL Category 每 100 筆批次下發。
4. **實時終端進度條 (Progress Bar)**：透過非阻塞位元串流監聽 (`recv_ready()`) 與 Regex 配對發送狀況。
5. **組態自動化覆核檢驗 (Error Checking)**：自動執行 `show address-group` 比對已套用組態，並回報成功/失敗筆數與限制異常（如 URL 超過 255 字元上限）。

## 技術細節

- **語言與版本**：Python 3.8.18
- **核心依賴**：`paramiko`, `pandas`, `openpyxl`, `chardet`
- **打包交付**：使用 PyInstaller 打包為獨立 `.exe` 免安裝執行檔 (`PaloAltoFirewallConfigurator-v6.6.exe`)
- **平台需求**：Windows 7 以上

## 相關概念

- [[SRE-學習路徑]] — 包含自動化維運 (Automation & Tooling) 與資安防禦
- [[工作專案/PA440-FW-data-configurator|完整技術分享筆記]]

## 來源

- [[工作專案/PA440-FW-data-configurator]]（工作紀錄，完整技術與部署細節）
