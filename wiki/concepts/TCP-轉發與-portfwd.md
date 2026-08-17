---
title: TCP 轉發與 portfwd
tags: [概念, Networking, Python, Security, Socket, Refactoring]
updated: 2026-08-17
source_count: 1
---

# TCP 轉發與 portfwd 工具重構

[[Ethan]] 把散落在實驗室、綁平台、缺測試的 4 組 Python 轉發腳本，整併為單一模組化、跨平台、附單元測試的工具庫 `portfwd`。同時是一份 TCP Port Forwarding 的實戰整理。

## 重構前的四個問題

1. **代碼冗餘**：核心 `forward_data` 雙向轉送邏輯被重複實作 6 次，細節（讀取大小、例外拋出、關閉時機）互不一致。
2. **硬編碼**：寫死實驗室 VM IP 與固定 Port。
3. **平台強依賴與破壞性行為**：直接呼叫 Windows 專屬 API（`ctypes.windll`、`e.winerror`）在 Linux/macOS 崩潰；收到魔術字串 `b'close'` 時 `os._exit(1)` 強殺整個進程。
4. **連線語意問題**：反向中繼端對單一控制連線重複 `accept()`，無法正確多工。

## 四種轉發情境（由簡入繁）

```
[標準轉發] -> [反向連線] -> [停服借埠] -> [IP 動態分流]
 (直連轉送)   (穿透 NAT)    (借用佔用埠)   (來源 IP 分流)
```

1. **標準轉發**：跳板機可直連目標與請求端；本地 `bind` 監聽後主動連目標做雙向轉送。改進點是 `--kill-switch` 只優雅中斷該條 socket 而非殺進程。
2. **反向連線穿透**：目標在 Strict NAT 內、只能對外連線。內網 client 主動連公網 server 建控制連線，外部連入時經控制連線通知 client 反向打洞。
3. **停服借埠型**：目標 Port 被合法服務（如 Tomcat 8080）佔用。暫停服務 → 轉發器搶佔該 Port → 計時器到點恢復服務。
4. **來源 IP 動態分流**：借埠同時對一般使用者無感。先把 Tomcat 搬到空閒高位埠，轉發器接管原埠，依 `Client IP` 分流——指定 IP（`lhost`）轉到目標 RDP、其他一般 IP 轉回搬家後的 Tomcat。

## 重構亮點

- **統一轉發原語**（`relay.py`）：抽出唯一正規的 `forward_data` / `bidirectional_relay`，buffer 由 1024 bytes 放大到 64KB；跨平台斷線過濾 `_is_expected_disconnect`（`ConnectionResetError` 等 + Windows `10053/10054`）。
- **跨平台 Tomcat 控制器**（`tomcat.py`）：`TomcatController` 支援 Windows/Linux/macOS 啟停腳本，用 `xml.etree.ElementTree` 安全改 `server.xml`；`is_admin()` 跨平台判斷 root/Administrator。
- **規範化 CLI**（`cli.py`）：`argparse` 子指令統整四種模式的參數。
- **測試**：pytest Loopback 端到端 28 項全過，`find_free_port()` 自動找空閒埠、`DummyTomcatController` 隔離系統服務副作用，可在任何 CI 無害執行。

## 效益

消除 80%+ 重複程式碼；危險的強殺進程改連線級關閉；完整跨平台；建立現代 Python 套件架構（`pyproject.toml`）與自動化測試。

## 相關

- [[Ethan]] — 作者
- [[wiki/entities/KeyLogger-Server|KeyLogger-Server]] — 同屬網路/socket 底層的 C++ 專案
- [[Postfix-Manager]] — 另一個牽涉埠與網路層的自架服務（Nginx SSL Stream 繞過封鎖埠）

## 來源

- [[工作專案/portfwd-TCP轉發工具重構筆記]]（2026-07-24，四種轉發情境、重構亮點與測試）
