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

### 官方與社群常用 MCP Servers

| 類別 | MCP Server | 功能 |
|------|-----------|------|
| **檔案系統** | `@modelcontextprotocol/server-filesystem` | 讀寫本機檔案目錄 |
| **資料庫** | `@modelcontextprotocol/server-postgres` | 查詢 PostgreSQL |
| **資料庫** | `@modelcontextprotocol/server-sqlite` | 讀寫 SQLite |
| **版本控制** | `@modelcontextprotocol/server-github` | GitHub repo、PR、Issues |
| **搜尋** | `@modelcontextprotocol/server-brave-search` | Brave 網路搜尋 |
| **瀏覽器** | `@modelcontextprotocol/server-puppeteer` | 操控瀏覽器 |
| **溝通** | Slack MCP | 讀寫 Slack 頻道 |
| **設計** | Figma MCP | 讀取設計稿資訊 |
| **監控** | Sentry MCP | 查詢錯誤報告 |

> 完整清單：[modelcontextprotocol.io/servers](https://modelcontextprotocol.io/servers)

---

## 在 Claude Code 中設定 MCP

### 設定格式

MCP server 設定存放於 `~/.claude.json`（全域）或 `.claude/settings.json`（專案層級）。

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
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
      }
    },
    "postgres": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-postgres",
        "postgresql://localhost/mydb"
      ]
    }
  }
}
```

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
