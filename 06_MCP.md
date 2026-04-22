[← 上一篇：API](05_API.md) | [下一篇：Skill →](07_SKILL.md)

# MCP（Model Context Protocol）

> **知識截止日期：** 2026 年 4 月

---

## MCP 是什麼？

MCP（Model Context Protocol）是 Anthropic 制定的開放標準，讓 Claude 能夠**主動連接並操作外部工具與資料來源**。

### MCP vs Claude API 的差異

```
Claude API：你呼叫 Claude
┌────────────┐    請求     ┌─────────┐
│  你的程式  │ ──────────▶ │  Claude │
│            │ ◀────────── │         │
└────────────┘    回應     └─────────┘

MCP：Claude 連接外部工具
┌─────────┐   工具呼叫   ┌────────────────┐
│  Claude │ ────────────▶ │  MCP Server    │
│         │ ◀──────────── │  (GitHub/DB/   │
└─────────┘   工具結果   │   Slack/...)   │
                          └────────────────┘
```

**簡單說：**
- **API**：你讓 Claude 回答問題
- **MCP**：你讓 Claude 去操作其他工具，代替你完成任務

### MCP 解決什麼問題？

沒有 MCP 時，工程師每次需要「讓 Claude 查看 GitHub Issue」都要手動複製貼上。有了 MCP，Claude 可以直接：
- 查詢你的 GitHub repo、PR、Issues
- 讀寫本機檔案系統
- 查詢資料庫
- 發送 Slack 訊息
- 操作 Figma 設計稿

---

## MCP Server 類型

### 官方與第一方 MCP Servers（2026-Q2 活躍版本）

| 類別 | MCP Server | 維護者 |
|------|-----------|--------|
| **檔案系統** | [`@modelcontextprotocol/server-filesystem`](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem) | Anthropic（官方 reference） |
| **網頁抓取** | [`@modelcontextprotocol/server-fetch`](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch) | Anthropic（官方 reference） |
| **Git（本機）** | [`@modelcontextprotocol/server-git`](https://github.com/modelcontextprotocol/servers/tree/main/src/git) | Anthropic（官方 reference） |
| **記憶** | [`@modelcontextprotocol/server-memory`](https://github.com/modelcontextprotocol/servers/tree/main/src/memory) | Anthropic（官方 reference） |
| **資料庫** | [`@supabase/mcp-server-supabase`](https://github.com/supabase-community/supabase-mcp) | Supabase（第一方） |
| **資料庫** | [`@neondatabase/mcp-server-neon`](https://github.com/neondatabase/mcp-server-neon) | Neon（第一方，限 Neon Postgres） |
| **版本控制** | [`github/github-mcp-server`](https://github.com/github/github-mcp-server) | **GitHub 官方** |
| **搜尋** | [`@brave/brave-search-mcp-server`](https://github.com/brave/brave-search-mcp-server) | **Brave 官方** |
| **瀏覽器** | [`@playwright/mcp`](https://github.com/microsoft/playwright-mcp) | **Microsoft 官方** |
| **溝通** | [Slack 官方 remote MCP](https://docs.slack.dev/changelog/2026/02/17/slack-mcp/) | **Slack 官方**（2026-02 推出） |
| **設計** | [Figma MCP](https://www.figma.com/developers/mcp) | **Figma 官方** |
| **監控** | [`@sentry/mcp-server`](https://github.com/getsentry/sentry-mcp) | **Sentry 官方** |

> 完整目錄：[modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) 與 [MCP Registry](https://registry.modelcontextprotocol.io/)

### ⚠️ 歷史 Reference Servers（已歸檔，不建議新專案使用）

下列五個 server 為 Anthropic 早期官方 reference 實作，**已於 2025-05-29 整批歸檔**至 [`modelcontextprotocol/servers-archived`](https://github.com/modelcontextprotocol/servers-archived)。歸檔後**不再收安全修補與 bug fix**（例：postgres server 有已知 SQL injection 案例）。若仍在 npm 上找到這些套件，請改用下表的繼任版本。

| 已歸檔 | 建議改用 |
|---|---|
| `@modelcontextprotocol/server-postgres` | Supabase MCP 或 Neon MCP |
| `@modelcontextprotocol/server-sqlite` | ⚠️ 無第一方繼任；社群版需自行評估安全性 |
| `@modelcontextprotocol/server-github` | `github/github-mcp-server`（GitHub 官方） |
| `@modelcontextprotocol/server-brave-search` | `@brave/brave-search-mcp-server`（Brave 官方） |
| `@modelcontextprotocol/server-puppeteer` | `@playwright/mcp`（Microsoft 官方） |

---

## 在 Claude Code 中設定 MCP

### 設定格式

Claude Code 的 MCP 設定有三種 scope（作用範圍）：

| Scope | 載入於 | 可隨 git 分享 | 存放位置 |
|---|---|---|---|
| **Local**（預設） | 當前專案（僅自己） | ❌ | `~/.claude.json`（該專案路徑下） |
| **Project**（團隊共用） | 當前專案（全隊） | ✅ 提交 `.mcp.json` | `.mcp.json`（專案根目錄） |
| **User**（跨專案） | 你的所有專案 | ❌ | `~/.claude.json`（根層級） |

> ⚠️ `.mcp.json` 與 `.claude/settings.json` 是**不同檔案**。後者是 Claude Code 的一般設定（hooks、permissions 等），**不含 MCP server 設定**。

團隊共用的 `.mcp.json` 範例（可 commit 進 repo）：

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/you/projects"
      ]
    },
    "github": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "GITHUB_PERSONAL_ACCESS_TOKEN",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
      }
    },
    "supabase": {
      "command": "npx",
      "args": [
        "-y",
        "@supabase/mcp-server-supabase",
        "--access-token",
        "sbp_..."
      ]
    }
  }
}
```

個人專屬設定建議透過 CLI 寫入（見下節「透過 CLI 新增」），避免手動編輯 `~/.claude.json`。

### 透過 CLI 新增

```bash
# 新增 filesystem MCP server
claude mcp add filesystem -- npx -y @modelcontextprotocol/server-filesystem /path/to/dir

# 新增 GitHub MCP server
claude mcp add github -- npx -y @modelcontextprotocol/server-github

# 列出已設定的 MCP servers
claude mcp list

# 刪除 MCP server
claude mcp remove github
```

### 驗證連線

啟動 Claude Code 後，可在對話中確認：

```
你：列出目前可用的工具

Claude：目前可用的 MCP 工具包含：
- filesystem: read_file, write_file, list_directory...
- github: get_pull_request, list_issues, create_issue...
```

---

## 在 Claude.ai 中設定 MCP

Claude.ai 網頁介面也支援 MCP（需要 Pro 以上方案）。

### 設定步驟

1. 前往 **Settings** → **Integrations**（或 **Connections**）
2. 點擊 **Add MCP Server**
3. 輸入 MCP Server 的連線資訊：
   - **Name**：識別名稱
   - **Command / URL**：Server 啟動指令或遠端 URL
4. 確認連線後，在對話中即可使用

### 使用方式

連線後，在對話中直接描述任務即可：

```
「幫我查看 my-repo 最近 5 個 PR 的狀態」
「列出 production 資料庫中，最近一小時的錯誤記錄」
「在 #engineering 頻道發送 deploy 完成的通知」
```

Claude 會自動決定何時呼叫哪個 MCP tool。

---

## 安全注意事項

- MCP Server 擁有直接操作外部系統的權限，設定時請確認來源可信
- 包含敏感資訊（Token、密碼）的設定請使用環境變數，不要硬寫在設定檔
- 定期檢查已安裝的 MCP Server 清單，移除不再使用的項目
