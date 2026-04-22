# Verification Report: `06_MCP.md`

## Header

| Field | Value |
|---|---|
| Source filename | `06_MCP.md` |
| Source line count | 162 |
| External URL count in source | 1 |
| Verification date | 2026-04-22 |
| Verifier | Claude Opus 4.7 |

## Claims Table

| # | Line | Claim (excerpt) | Type | Result | Source / Suggestion |
|---|---|---|---|---|---|
| 1 | 51 | `@modelcontextprotocol/server-filesystem` — 讀寫本機檔案目錄 | product-spec | ✅ Verified | [npm 套件頁](https://www.npmjs.com/package/@modelcontextprotocol/server-filesystem) 現仍活躍，最新版 2026.1.14、週下載 137k；亦為 `modelcontextprotocol/servers` repo `src/filesystem` 下**仍在維護**的 reference server 之一 |
| 2 | 52 | `@modelcontextprotocol/server-postgres` — 查詢 PostgreSQL | product-spec | ⚠️ Needs correction | 官方於 **2025-07-10 將此套件 deprecated 並歸檔**至 [modelcontextprotocol/servers-archived](https://github.com/modelcontextprotocol/servers-archived)，npm 頁（v0.6.2）亦停止更新。雖仍可安裝，但被官方視為已廢棄 / 有已知安全議題（[Datadog Security Labs — SQL injection case study](https://securitylabs.datadoghq.com/articles/mcp-vulnerability-case-study-SQL-injection-in-the-postgresql-mcp-server/)）。建議改寫為「此 reference server 已歸檔；改用社群維護版本或自建」 |
| 3 | 53 | `@modelcontextprotocol/server-sqlite` — 讀寫 SQLite | product-spec | ⚠️ Needs correction | 已被官方歸檔至 servers-archived repo（與 PostgreSQL 同）。建議比照處置 |
| 4 | 54 | `@modelcontextprotocol/server-github` — GitHub repo、PR、Issues | product-spec | ⚠️ Needs correction | 官方已歸檔；開發遷移至 [github/github-mcp-server](https://github.com/github/github-mcp-server)（GitHub 官方接手維護）。建議改寫為：「GitHub 官方 MCP server 位於 `github/github-mcp-server`；原 `@modelcontextprotocol/server-github` 為已歸檔 reference 版本」 |
| 5 | 55 | `@modelcontextprotocol/server-brave-search` — Brave 網路搜尋 | product-spec | ⚠️ Needs correction | 已被官方歸檔，**由 Brave 官方提供替代 MCP server**。建議更新為 Brave 官方版本的安裝指令 |
| 6 | 56 | `@modelcontextprotocol/server-puppeteer` — 操控瀏覽器 | product-spec | ⚠️ Needs correction | 已歸檔（最後更新 2025-05-12）。建議改推 Playwright MCP / Browserbase 或社群活躍版本 |
| 7 | 57 | Slack MCP — 讀寫 Slack 頻道 | product-spec | 🔍 Unverifiable | 文章僅以「Slack MCP」泛稱，未給具體 npm 套件名；官方 `@modelcontextprotocol/server-slack` 亦已歸檔。建議明示要使用哪個具體 server（Slack 官方版或社群版）並附連結 |
| 8 | 58 | Figma MCP — 讀取設計稿資訊 | product-spec | ✅ Verified | Figma 官方提供 MCP server（亦是本次 session context 中 `claude.ai Figma` MCP 的對應實作）。來源：Figma 官方文件與 claude.ai Figma MCP |
| 9 | 59 | Sentry MCP — 查詢錯誤報告 | product-spec | 🔍 Unverifiable | 原 `@modelcontextprotocol/server-sentry` 已歸檔；Sentry 官方是否推出繼任 MCP server 需另行查證。建議補具體套件／URL |
| 10 | 61 | `https://modelcontextprotocol.io/servers` 連結到完整清單 | url-reachability | ❌ Unsupported | WebFetch 回傳 **HTTP 404**。此 URL 目前**不存在**。官方 server 目錄實際路徑為 [modelcontextprotocol/servers GitHub repo](https://github.com/modelcontextprotocol/servers) 與 [MCP Registry](https://registry.modelcontextprotocol.io/)。建議：更新連結為這兩個中的一個，並刪除失效 URL |
| 11 | 69 | MCP server 設定存放於 `~/.claude.json`（全域）或 `.claude/settings.json`（專案層級） | product-spec | ⚠️ Needs correction | 官方 Claude Code 文件實際使用 **`~/.claude.json`（全域）與 `.mcp.json`（專案根目錄，可 `git` 提交）**，`.claude/settings.json` **並非 MCP 設定檔**。建議更新為「全域 `~/.claude.json`／專案 `.mcp.json`」。來源：[Claude Code CLI reference — MCP](https://code.claude.com/docs/en/cli-reference)、多家 2026 CLI guide |
| 12 | 105 | `claude mcp add <name> -- <command>` 新增 MCP server | product-spec | ✅ Verified | [CLI reference](https://code.claude.com/docs/en/cli-reference) 確認；亦可加 `--scope project` 寫入 `.mcp.json` |
| 13 | 111 | `claude mcp list` 列出已設定 MCP server | product-spec | ✅ Verified | 同上 CLI reference |
| 14 | 114 | `claude mcp remove <name>` 刪除 MCP server | product-spec | ✅ Verified | 同上 |
| 15 | 133 | Claude.ai 網頁支援 MCP，**需 Pro 以上方案** | product-spec | ⚠️ Needs correction | 官方規則更細：<br>- **Directory connectors**（預先整合）：**所有方案（含 Free）可用**<br>- **Custom MCP server URL**（自行加 URL）：**需 Pro 以上付費方案**<br>建議改寫為「Claude.ai 網頁透過 Connectors 支援 MCP：預先整合之 connectors 全方案可用，自訂 MCP server URL 需 Pro 以上方案」。來源：[Get started with custom connectors using remote MCP](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp) |
| 16 | 137 | Claude.ai 設定入口：**Settings → Integrations（或 Connections）** | product-spec | ⚠️ Needs correction | 官方實際命名為 **Settings → Connectors**；「Add custom connector」位於此頁面下。"Integrations" 非當前官方命名。建議更新為「Settings → Connectors → Add custom connector」 |

## Summary

### Counts

| Marker | Count |
|---|---|
| ✅ Verified | 5 |
| ⚠️ Needs correction | 8 |
| ❌ Unsupported | 1 |
| 🔍 Unverifiable | 2 |
| **Total** | 16 |

### Top 3–5 Critical Corrections

1. **Lines 52–56 — 五個 `@modelcontextprotocol/server-*` 套件皆已歸檔**（5× ⚠️）：postgres、sqlite、github、brave-search、puppeteer 皆於 2025 年被官方歸檔。**文章仍將其列為「官方與社群常用」會直接誤導讀者使用已過時、已知有安全風險的 reference 實作**。建議整理一份 2026-Q2 的活躍清單替換，並標示歸檔版本為「historical / not recommended」。
2. **Line 61 — 官方 servers URL 404**（❌）：`modelcontextprotocol.io/servers` 已不存在，應改為 GitHub repo 或 MCP Registry。
3. **Line 69 — 設定檔路徑錯誤**（⚠️）：專案層級 MCP 設定應為 `.mcp.json` 而非 `.claude/settings.json`。讀者若照文章寫會導致 MCP 不被載入。
4. **Line 133 — Claude.ai MCP 方案規則過度簡化**（⚠）：Directory connectors 全方案可用；只有自訂 MCP URL 需付費。
5. **Line 137 — Settings 路徑命名錯誤**（⚠）：實際為 "Connectors"，非 "Integrations / Connections"。

### Overall Risk Assessment

**Risk level:** High

此文為 MCP 的**入門教學**，但**所列的 6 個具體套件名中有 5 個已被 Anthropic 官方歸檔**（83% 過時率）。讀者依此文操作會 (a) 安裝已廢棄套件、(b) 遇到未修補的安全漏洞（例：postgres server 已知 SQL injection 案例）、(c) 在網頁介面找不到「Integrations」選單。設定檔路徑錯誤則會導致 MCP 完全不被載入 — 所有後續除錯都會卡住。建議本文作為**下一輪 change 最優先全面改寫**的對象。

### Detection Coverage Note

本報告檢測到 **16 條硬性聲明**。以下未列入：

- 純操作範例（JSON 設定檔樣板、CLI 指令範例）
- ASCII 對比圖（API vs MCP）
- 主觀建議（例：「設定時請確認來源可信」「使用環境變數」）
- 使用情境舉例（例：「查詢 GitHub Issues」流程敘述）

後續讀者（ChatGPT / Gemini）若發現本報告漏掉的聲明，屬預期行為 — 本驗證不保證 100% 覆蓋。
