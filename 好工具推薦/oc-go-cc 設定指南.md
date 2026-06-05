---
title: oc-go-cc 設定指南
created: 2026-06-05
tags:
  - tool
  - proxy
  - claude-code
  - opencode
---

# oc-go-cc 設定指南

> oc-go-cc 是一套 API 格式轉換 Proxy，讓 Claude Code 可以透過 OpenCode Go/Zen 的模型來執行。

## 系統架構

```
Claude Code (Anthropic API 格式)
     ↓
oc-go-cc (localhost:9090) ←── 即時轉換 Anthropic ↔ OpenAI/Responses/Gemini 格式
     ↓
OpenCode Go / Zen (開放模型 API)
```

## 安裝

### 透過 Scoop（Windows）

```powershell
# 安裝 Scoop（如果尚未安裝）
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh | iex

# 加入 oc-go-cc bucket 並安裝
scoop bucket add oc-go-cc https://github.com/samueltuyizere/scoop-bucket
scoop install oc-go-cc
```

### 注意：Scoop shim 問題

如果安裝後 `oc-go-cc` 指令找不到，可能是 Scoop 的 shim 未正確建立。解法：

```powershell
# 手動加入 PATH
$env:Path += ";$env:USERPROFILE\scoop\apps\oc-go-cc\0.2.5"
```

永久加入 PATH：
```powershell
$currentPath = [Environment]::GetEnvironmentVariable("Path", "User")
[Environment]::SetEnvironmentVariable("Path", "$currentPath;$env:USERPROFILE\scoop\apps\oc-go-cc\0.2.5", "User")
```

### 透過 Docker

```powershell
docker run -d --restart unless-stopped --name oc-go-cc -p 9090:3456 `
  -v %USERPROFILE%\.config\oc-go-cc:/root/.config/oc-go-cc `
  ghcr.io/samueltuyizere/oc-go-cc
```

## 初始設定

### 產生設定檔

```powershell
oc-go-cc init
```

設定檔路徑：`~/.config/oc-go-cc/config.json`

### 取得 OpenCode API Key

1. 執行 `opencode auth login` 或前往 https://opencode.ai/auth
2. 選擇 OpenCode Go 或 Zen 方案
3. 複製 API Key

### 設定 API Key

方式一：設定環境變數（建議用於安全性）

```powershell
[Environment]::SetEnvironmentVariable("OC_GO_CC_API_KEY", "sk-opencode-your-key-here", "User")
```

方式二：直接寫入設定檔

```json
{
  "api_key": "sk-opencode-your-key-here"
}
```

### 修改 Port

Windows 的 Hyper-V/WSL 會保留特定埠範圍。查詢保留埠：

```powershell
netsh int ipv4 show excludedportrange protocol=tcp
```

將設定檔中的 port 改到保留範圍外的數字：

```json
{
  "host": "127.0.0.1",
  "port": 9090
}
```

常見可用埠：`8080`、`9090`、`3000`、`8000`

## 啟動 Proxy

```powershell
oc-go-cc serve
```

預設監聽 `http://127.0.0.1:9090`。

### 指定不同 Port

```powershell
oc-go-cc serve --port 9090
```

### 背景執行

```powershell
Start-Process -FilePath "$env:USERPROFILE\scoop\apps\oc-go-cc\0.2.5\oc-go-cc.exe" -ArgumentList "serve" -WindowStyle Hidden
```

## 設定檔說明

```json
{
  "api_key": "${OC_GO_CC_API_KEY}",
  "host": "127.0.0.1",
  "port": 9090,
  "hot_reload": false,
  "models": {
    "default":     { "provider": "opencode-go", "model_id": "kimi-k2.6" },
    "fast":        { "provider": "opencode-go", "model_id": "qwen3.6-plus" },
    "think":       { "provider": "opencode-go", "model_id": "glm-5" },
    "complex":     { "provider": "opencode-go", "model_id": "glm-5.1" },
    "long_context": { "provider": "opencode-go", "model_id": "minimax-m2.5" },
    "background":  { "provider": "opencode-go", "model_id": "qwen3.5-plus" }
  },
  "fallbacks": {
    "default": [
      { "provider": "opencode-go", "model_id": "mimo-v2-pro" },
      { "provider": "opencode-go", "model_id": "qwen3.6-plus" }
    ]
  }
}
```

### 模型情境說明

| 情境 | 預設模型 | 用途 |
|------|---------|------|
| `default` | kimi-k2.6 | 一般問答與編碼 |
| `fast` | qwen3.6-plus | 快速回應（低延遲） |
| `think` | glm-5 | 需要深度思考的任務 |
| `complex` | glm-5.1 | 複雜多步驟任務 |
| `long_context` | minimax-m2.5 | 長上下文處理 |
| `background` | qwen3.5-plus | 背景任務 |

## 與 Claude Code 串接

### 透過環境變數

```powershell
$env:ANTHROPIC_BASE_URL = "http://127.0.0.1:9090"
$env:ANTHROPIC_AUTH_TOKEN = "unused"
claude
```

### 透過 CC Switch 切換

1. 打開 CC Switch 桌面應用
2. 在 Claude Code 分類下找到 **oc-go-cc (OpenCode Go)**
3. 點擊啟用（CC Switch 會自動寫入 `~/.claude/settings.json`）
4. 開啟新終端機執行 `claude`

可在 CC Switch 一鍵切換不同 Provider：
- **Claude Official** → 直連 Anthropic
- **cli-api** → 其他 relay 服務
- **oc-go-cc (OpenCode Go)** → 走 OpenCode Go 模型

## 驗證 Proxy 正常運作

```powershell
$body = @{
  model = "claude-sonnet-4-20250514"
  max_tokens = 100
  messages = @(@{role = "user"; content = "Say hello in one word"})
} | ConvertTo-Json

Invoke-RestMethod -Uri "http://127.0.0.1:9090/v1/messages" `
  -Method Post -Body $body -ContentType "application/json" `
  -Headers @{"x-api-key"="test"} -TimeoutSec 120
```

成功回應範例：
```json
{
  "id": "chatcmpl-...",
  "type": "message",
  "role": "assistant",
  "content": [{ "type": "text", "text": "Hello" }],
  "model": "kimi-k2.6",
  "usage": { "input_tokens": 13, "output_tokens": 139 }
}
```

## 疑難排解

| 問題 | 原因 | 解法 |
|------|------|------|
| `bind: access permissions` | 埠被 Windows 保留 | 換一個埠（如 9090） |
| `oc-go-cc 找不到指令` | Scoop shim 未建立 | 手動加入 PATH |
| 請求逾時 | API Key 未設定 | 設定 `OC_GO_CC_API_KEY` |
| 模型回應慢 | 預設模型較重 | 改用 `fast` 情境的模型 |
