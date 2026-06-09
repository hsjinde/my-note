---
title: OpenCode MCP 配置指南
created: 2026-06-07
tags:
  - tool
  - mcp
  - opencode
  - obsidian
  - notebooklm
  - playwright
---

# OpenCode MCP 配置指南

> 本筆記記錄 OpenCode 已配置的 MCP servers，設定檔位於 `~/.config/opencode/opencode.json`。

## 已配置的 MCP Servers

| Server | 用途 | 狀態 |
|--------|------|------|
| `obsidian` | 讀寫 Obsidian vault | ✅ 已啟用 |
| `notebooklm` | NotebookLM 筆記本管理 | ✅ 已啟用 |
| `playwright` | 瀏覽器自動化 | ✅ 已啟用 |
| `open-computer-use` | 桌面自動化（點擊、輸入、截圖） | ✅ 已啟用 |

---

## opencode.json 完整 MCP 設定

```json
{
  "mcp": {
    "obsidian": {
      "type": "local",
      "command": ["C:\\Users\\hsjin\\AppData\\Roaming\\npm\\mcpvault.cmd", "D:\\my-note"],
      "enabled": true
    },
    "notebooklm": {
      "type": "local",
      "command": ["C:\\Users\\hsjin\\.local\\bin\\notebooklm-mcp.exe", "--transport", "stdio"],
      "enabled": true
    },
    "playwright": {
      "type": "local",
      "command": ["npx", "-y", "@playwright/mcp"],
      "enabled": true
    },
    "open-computer-use": {
      "type": "local",
      "command": ["open-computer-use", "mcp"],
      "enabled": true
    }
  }
}
```

---

## 各 Server 安裝說明

### Obsidian（mcpvault）

- **套件**：`@bitbonsai/mcpvault`
- **安裝**：`npm install -g @bitbonsai/mcpvault`
- **執行檔**：`C:\Users\hsjin\AppData\Roaming\npm\mcpvault.cmd`
- **Vault 路徑**：`D:\my-note`
- **文件**：[[好工具推薦/oc-go-cc 設定指南]]

### NotebookLM

- **套件**：`notebooklm-mcp-cli` v0.7.1
- **安裝**：`uv tool install notebooklm-mcp-cli`
- **執行檔**：`C:\Users\hsjin\.local\bin\notebooklm-mcp.exe`
- **登入帳號**：`piplup19980803@gmail.com`
- **重新登入**：`nlm login`（需關閉 Chrome 後執行）

> [!tip] 登入小技巧
> 若 `nlm login` 瀏覽器閃退，先執行 `Stop-Process -Name chrome -Force` 關閉所有 Chrome 後再試。

### Playwright

- **套件**：`@playwright/mcp`（透過 npx 自動下載，無需預先安裝）
- **用途**：開啟網頁、填表單、截圖、網頁自動化

### open-computer-use

- **套件**：`open-computer-use` v0.1.52
- **安裝**：`npm install -g open-computer-use`
- **啟動方式**：`open-computer-use mcp`（不可用 `npx`）
- **可用工具**：click、drag、type_text、press_key、scroll、get_app_state、list_apps 等 9 個工具

> [!warning] Windows 注意
> 勿使用 `npx open-computer-use install-opencode-mcp`，此命令在 Windows 需要 POSIX shell，會失敗。請直接手動編輯 `opencode.json`。

---

## 重新啟動說明

OpenCode 配置**只在啟動時載入一次**，修改設定後需重啟才生效。  
重啟後右上角會出現對應數量的 MCP 燈號。

---

## 相關資源

- 懶人包來源：[mathruffian-dot/opencode-lazy-packs](https://github.com/mathruffian-dot/opencode-lazy-packs/tree/main)
- 對應懶人包：`03-obsidian`、`01-notebooklm`、`06-browser`
- OpenCode 設定文件：`~/.config/opencode/opencode.json`
