---
title: KeyLogger-Server
tags: [專案, C++, 網路]
updated: 2026-06-05
source_count: 1
---

# KeyLogger-Server

C++ Winsock 網路程式，Server-Client 架構的鍵側錄系統。教育用途。

## 架構

- **Server**：一對多通訊，監聽 port 4790，多執行緒處理各 Client
- **Client**：開機自啟，側錄按鍵、截圖、定時回傳

## Server 功能

1. 查看已連線 Client 清單
2. 查看 Log 訊息
3. 傳送 bat 檔給所有 Client
4. 瀏覽 Client 資料夾
5. 查看 Client 當前目錄
6. 遠端執行 Client 上的檔案
7. 關閉 Server

## Client 功能

- 記錄按鍵（含視窗切換時間戳）
- 記錄滑鼠點擊
- 定時截圖
- 自動回傳 Log 與 BMP 至 Server

## Packet Types

| Type | 用途 |
|---|---|
| PT_ChatMessage | 文字訊息 |
| PT_FileFolderInformation | 資料夾內容 |
| PT_CurrFolder | 當前目錄 |
| PT_OpenData | 遠端執行檔案 |
| PT_BMPSize / PT_BMPFile / PT_BMPSave | 截圖傳輸 |

## 技術細節

- **語言**：C++17（ISO C++ 17）
- **平台**：Windows 7/10/11
- **網路**：Winsock API，自封裝 PNet 庫
- **IDE**：Visual Studio
- **Server 日誌結構**：`Serverlog/IP/IP_時間.txt` + `時間.bmp`

## 相關概念

- [[資料結構總覽]] — Socket 通訊使用多執行緒與 Queue 概念

## 來源

- [[KeyLogger-Server]]（Jin-De Lin, 2023）
