# Fix Recommendations — 驗證發現之修正建議彙整

> **用途**：本檔彙整 2026-04-22 驗證發現中、經使用者與驗證者討論後達成共識的**具體修改建議**。
> 原文（`0*_*.md`、`appendix_*.md`）目前**尚未修改**；本檔為「待改寫清單」，最終採用與否由作者決定。
>
> **來源**：`verify/SUMMARY.md` Top 10 優先清單
> **討論日期起始**：2026-04-22
> **討論對象**：Claude Opus 4.7 與使用者逐條討論，每條皆獨立查證

---

## Tier A —（Top 5）會直接造成讀者操作失敗 / 安全風險

### ✅ Tier A #1 — 06_MCP.md line 47–61：官方 / 社群常用 MCP Servers 表格

**問題摘要**
原表格列出 6 個具體套件名，其中 5 個（postgres、sqlite、github、brave-search、puppeteer）已於 **2025-05-29** 被 Anthropic 整批歸檔至 [`modelcontextprotocol/servers-archived`](https://github.com/modelcontextprotocol/servers-archived)。postgres server 有已知 SQL injection 案例。完整清單連結 `modelcontextprotocol.io/servers` 亦 404（見 Tier B #6）。

**獨立查證**（2026-04-22）
- `modelcontextprotocol/servers` 主 repo README 已明示歸檔事實
- `modelcontextprotocol/servers-archived` 內確認 7 個舊套件
- `github/github-mcp-server` 為 GitHub 官方接手（v1.0.1 / 2026-04-21）
- `@playwright/mcp`（Microsoft 官方）、`@brave/brave-search-mcp-server`（Brave 官方）、`getsentry/sentry-mcp`（Sentry 官方）皆活躍
- Slack 官方於 2026-02-17 推出 remote MCP server
- SQLite 確認**無第一方繼任版本**，僅社群版

**決議建議**
採用「**拆兩個表格**」方案：主表格列 2026-Q2 活躍版本 + 歸檔區做對照。

**擬改寫內容**

```md
### 官方與第一方 MCP Servers（2026-Q2 活躍版本）

| 類別 | MCP Server | 維護者 |
|------|-----------|--------|
| 檔案系統 | [@modelcontextprotocol/server-filesystem](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem) | Anthropic（官方 reference） |
| 網頁抓取 | [@modelcontextprotocol/server-fetch](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch) | Anthropic（官方 reference） |
| Git（本機） | [@modelcontextprotocol/server-git](https://github.com/modelcontextprotocol/servers/tree/main/src/git) | Anthropic（官方 reference） |
| 記憶 | [@modelcontextprotocol/server-memory](https://github.com/modelcontextprotocol/servers/tree/main/src/memory) | Anthropic（官方 reference） |
| 資料庫 | [@supabase/mcp-server-supabase](https://github.com/supabase-community/supabase-mcp) | Supabase（第一方） |
| 資料庫 | [@neondatabase/mcp-server-neon](https://github.com/neondatabase/mcp-server-neon) | Neon（第一方，限 Neon Postgres） |
| 版本控制 | [github/github-mcp-server](https://github.com/github/github-mcp-server) | GitHub 官方 |
| 搜尋 | [@brave/brave-search-mcp-server](https://github.com/brave/brave-search-mcp-server) | Brave 官方 |
| 瀏覽器 | [@playwright/mcp](https://github.com/microsoft/playwright-mcp) | Microsoft 官方 |
| 溝通 | [Slack 官方 remote MCP](https://docs.slack.dev/changelog/2026/02/17/slack-mcp/) | Slack 官方（2026-02 推出） |
| 設計 | [Figma MCP](https://www.figma.com/developers/mcp) | Figma 官方 |
| 監控 | [@sentry/mcp-server](https://github.com/getsentry/sentry-mcp) | Sentry 官方 |

> 完整目錄：[modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) 與 [MCP Registry](https://registry.modelcontextprotocol.io/)

### ⚠️ 歷史 Reference Servers（已歸檔，不建議新專案使用）

下列五個 server 為 Anthropic 早期官方 reference 實作，已於 2025-05-29 整批歸檔至
[modelcontextprotocol/servers-archived](https://github.com/modelcontextprotocol/servers-archived)。
歸檔後不再收安全修補與 bug fix（例：postgres server 有已知 SQL injection 案例）。
若在 npm 上找到這些套件，請改用下表的繼任版本。

| 已歸檔 | 建議改用 |
|---|---|
| @modelcontextprotocol/server-postgres | Supabase MCP 或 Neon MCP |
| @modelcontextprotocol/server-sqlite | ⚠️ 無第一方繼任；社群版需自行評估安全性 |
| @modelcontextprotocol/server-github | github/github-mcp-server（GitHub 官方） |
| @modelcontextprotocol/server-brave-search | @brave/brave-search-mcp-server（Brave 官方） |
| @modelcontextprotocol/server-puppeteer | @playwright/mcp（Microsoft 官方） |
```

**狀態**：✅ 討論完成，待作者採用

---

### ✅ Tier A #2 — 06_MCP.md line 67–99：設定格式段落

**問題摘要**
原文 line 69：「MCP server 設定存放於 `~/.claude.json`（全域）或 `.claude/settings.json`（專案層級）。」
此句錯誤：
1. **`.claude/settings.json` 不是 MCP 設定檔**（它是 Claude Code 一般設定:hooks、permissions），**不含 `mcpServers`**。讀者照抄會導致 MCP 完全不被載入。
2. 實際專案層級 MCP 設定應為 **`.mcp.json`**（專案根目錄）。
3. 原文只寫「全域 / 專案」二分，漏了實際有三種 scope（Local / Project / User）。

**獨立查證**（2026-04-22）
官方 `code.claude.com/docs/en/mcp` 明文列出三 scope 表：
- Local（預設）：當前專案、個人私有、存 `~/.claude.json`（該專案路徑下）
- Project：當前專案、團隊共用、存 `.mcp.json`（專案根目錄，可 git 提交）
- User：所有專案、個人、存 `~/.claude.json`（根層級）

並明確註記：「MCP local-scoped servers are stored in `~/.claude.json`, while general local settings use `.claude/settings.local.json`.」— 即兩者為完全不同檔案。

**決議建議**
採用「完整重寫 line 67–99」方案：以三 scope 表取代原本單句，並將 JSON 範例同步換掉 Tier A #1 已歸檔的套件（github、postgres），一次解決兩個關聯問題。

**擬改寫內容**

```md
### 設定格式

Claude Code 的 MCP 設定有三種 scope（作用範圍）：

| Scope | 載入於 | 可隨 git 分享 | 存放位置 |
|---|---|---|---|
| Local（預設） | 當前專案（僅自己） | ❌ | ~/.claude.json（該專案路徑下） |
| Project（團隊共用） | 當前專案（全隊） | ✅ 提交 .mcp.json | .mcp.json（專案根目錄） |
| User（跨專案） | 你的所有專案 | ❌ | ~/.claude.json（根層級） |

> ⚠️ `.mcp.json` 與 `.claude/settings.json` 是不同檔案。後者是 Claude Code
> 的一般設定（hooks、permissions 等），不含 MCP server 設定。

團隊共用的 `.mcp.json` 範例（可 commit 進 repo）：

[JSON 範例 — 使用 Tier A #1 建議的活躍套件：filesystem、github/github-mcp-server、supabase 等]

個人專屬設定建議透過 CLI 寫入（見下節「透過 CLI 新增」），
避免手動編輯 ~/.claude.json。
```

**狀態**：✅ 討論完成，待作者採用

---

### ✅ Tier A #3 — 07_SKILL.md line 25–33：Skill 儲存位置與載入優先順序

**問題摘要**
原文三處錯：
1. Line 33 載入優先順序「專案 > 全域 > 插件」**方向反了**
2. Line 31 插件路徑 `~/.claude/plugins/` 是安裝快取位置，**不是 skill 官方路徑**
3. 完全漏掉 **Enterprise 層級**

**獨立查證**（2026-04-22，官方 skills docs「Where skills live」段）
> When skills share the same name across levels, higher-priority locations win:
> **enterprise > personal > project**. Plugin skills use a `plugin-name:skill-name`
> namespace, so they cannot conflict with other levels.

四層路徑官方表格：
- Enterprise：managed settings 指定
- Personal：`~/.claude/skills/<skill-name>/SKILL.md`
- Project：`.claude/skills/<skill-name>/SKILL.md`
- Plugin：`<plugin>/skills/<skill-name>/SKILL.md`

**實際影響**
團隊若想用 project skill 覆蓋 personal skill（常見：團隊共用 commit 流程覆蓋個人版），照文章寫以為 project 會贏，實際 personal 贏 → 團隊版永遠用不到。

**決議建議**
一次修正四層路徑表 + 優先順序 + 插件 namespace 說明。

**擬改寫內容**

```md
### Skill 儲存位置

| 層級 | 路徑 | 作用範圍 |
|------|------|---------|
| Enterprise（管理式） | 企業 managed settings 指定 | 組織全體使用者 |
| Personal（全域個人） | ~/.claude/skills/<skill>/SKILL.md | 你的所有專案 |
| Project（專案） | .claude/skills/<skill>/SKILL.md | 僅本專案 |
| Plugin（插件） | <plugin>/skills/<skill>/SKILL.md | 啟用該插件處 |

**同名衝突時的優先順序：** Enterprise > Personal > Project

插件 skill 透過 `plugin-name:skill-name` namespace 獨立，
不會與上述三層同名競爭。
```

**狀態**：✅ 討論完成，待作者採用

---

### ✅ Tier A #4 + #5 — 07_SKILL.md line 178–189：Frontmatter 欄位表

**問題摘要**
整張 frontmatter 表有多處錯誤，其中兩條屬於 Tier A：
- **Line 185（Tier A #4）** — `allowed-tools` 語意**完全相反**：文章寫「限制可用工具」，官方實為「免詢問使用權」。讀者誤把本應允許的 skill 寫成受限版本；或誤以為這欄能做工具沙盒（實際做不到）。
- **Line 187（Tier A #5）** — `agent` 欄位型別錯誤：文章寫 `true`（boolean），官方需填 **subagent type 名稱**（字串，如 `Explore`、`Plan`、`general-purpose`）。照文章寫會導致 skill 無法啟動。

**連帶一併處理的同表格錯誤**（驗證報告 claim #9、#10、#15）：
- Line 182–183 `name`、`description` 標「必填 ✓」 — 官方：`name: No`、`description: Recommended`
- Line 184 列 `compatibility` — Claude Code **不解讀此欄位**（屬 Agent Skills 標準、非 Claude Code）
- Line 189「不出現在 `/help`」 — 官方實為「從 **`/` 選單**隱藏」

**獨立查證**（2026-04-22，官方 skills docs frontmatter reference）
- `allowed-tools`：「**grants permission** for the listed tools while the skill is active... **It does not restrict** which tools are available」
- `agent`：「Which **subagent type** to use when `context: fork` is set.」範例：`agent: Explore`
- 官方明示：「**All fields are optional.** Only `description` is recommended.」

**決議建議**
整張表重寫，同時補進原本漏掉的 8 個官方欄位（`when_to_use`、`argument-hint`、`arguments`、`model`、`effort`、`paths`、`hooks`、`shell`），讓表格從 7 欄擴到 15 欄，一次對齊官方。

**擬改寫內容**

```md
### Frontmatter 欄位說明

**所有欄位皆為選填**；只有 `description` 是「建議填」（影響 Claude 判斷觸發）。

| 欄位 | 必填 | 說明 |
|------|------|------|
| name | 否 | skill 名稱（即 slash command）。省略時使用目錄名。 |
| description | 建議 | 觸發機制的核心，Claude 靠此判斷何時使用。省略時使用 body 第一段。與 when_to_use 合計截斷於 1,536 字元。 |
| when_to_use | 否 | 額外觸發情境（trigger phrase、範例請求）。 |
| argument-hint | 否 | 補全時顯示預期參數（例：[issue-number]）。 |
| arguments | 否 | 位置參數命名，對應 skill 內 $name 變數替換。 |
| allowed-tools | 否 | skill active 時**免詢問**使用的工具清單。**不是限制**；其他工具仍可呼叫，但仍受 permission 設定管。 |
| context | 否 | fork = 在獨立 subagent 執行，隔離 context。 |
| agent | 否 | 搭配 context: fork，指定 **subagent type 名稱**（如 Explore、Plan、general-purpose 或自訂）。省略則用 general-purpose。 |
| disable-model-invocation | 否 | true = 只有使用者能手動呼叫（適合 /deploy、/commit 等副作用指令）。 |
| user-invocable | 否 | false = 從 `/` 選單隱藏，只有 Claude 能自動觸發（適合背景知識型 skill）。 |
| model | 否 | 此 skill 啟用時使用的 model（同 /model 可用值）。 |
| effort | 否 | effort level（low/medium/high/xhigh/max）。 |
| paths | 否 | glob patterns 限制 skill 只在編輯匹配檔案時自動觸發。 |
| hooks | 否 | skill 生命週期 hooks。 |
| shell | 否 | 執行 `!\`command\`` 用的 shell（bash 預設 / powershell）。 |
```

（原有的 `compatibility` 欄位**移除**：非 Claude Code 解讀欄位，保留會誤導。）

**狀態**：✅ 討論完成，待作者採用

---

## Tier B —（Top 6–10）重要修正

### ✅ Tier B #6 — 06_MCP.md line 61：servers 完整清單 URL 404

**問題摘要**
原文「完整清單：modelcontextprotocol.io/servers」此 URL **不存在**（WebFetch 回 HTTP 404）。

**獨立查證**（2026-04-22）
- `https://modelcontextprotocol.io/servers` → 404
- 官方 server 目錄實際位於：
  - [`modelcontextprotocol/servers` GitHub repo](https://github.com/modelcontextprotocol/servers)
  - [MCP Registry](https://registry.modelcontextprotocol.io/)

**決議建議**
Tier A #1 的擬改寫已把此連結換成上述兩個。本條與 A #1 合併處理即可。

**擬改寫內容**（同 Tier A #1）
```md
> 完整目錄：[modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers)
> 與 [MCP Registry](https://registry.modelcontextprotocol.io/)
```

**狀態**：✅ 討論完成（已併入 Tier A #1 改寫）

---

### ✅ Tier B #7 — 定價與插件 CLI 兩處錯誤

#### 7a. 01_GENERAL.md line 175：Team 方案定價錯誤

**問題摘要**
原文寫「**Team** | $30/人/月」— 官方無 $30 這個價位。

**獨立查證**（2026-04-22，`claude.com/pricing`）
Team 方案實際有兩種 seat：
- **Standard Seat**：$20/人/月（年繳）或 $25/月繳
- **Premium Seat**：$100/人/月（年繳）或 $125/月繳（含 Claude Code、更高 Agent 用量）

**擬改寫內容**

```md
| Team Standard | $20/人/月（年繳）/ $25（月繳） | 工程團隊（5–150 人） | Pro 功能 + 團隊協作、管理員控制台 |
| Team Premium  | $100/人/月（年繳）/ $125（月繳） | 含 Claude Code 的團隊 | Standard + Claude Code + 更高 Agent 用量 |
| Enterprise | 洽談 | 企業 | 自訂合約、SSO、更高速率、資料隱私合規、專屬支援 |
```

並同步更新 line 181–182 的「工程師評估建議」：
- 小團隊採用（5–20 人）→ **Team Standard**
- 小團隊需要 Claude Code → **Team Premium**

#### 7b. 07_SKILL.md line 348：插件安裝指令格式錯誤

**問題摘要**
原文：`claude plugins install github:your-org/claude-skills-pack`
- 頂層命令 `claude plugin install`（有 `plugins` alias）**存在**
- 但 **`github:` 前綴格式無效**；官方格式為 `<name>@<marketplace>`

**獨立查證**（2026-04-22）
- [Claude Code CLI reference](https://code.claude.com/docs/en/cli-reference)：`claude plugin install code-review@claude-plugins-official`
- Plugin 系統需先 `marketplace add` 再 `install`：
  - `/plugin marketplace add <owner/repo>` 或 CLI 等價命令
  - `/plugin install <skill-name>@<marketplace-name>`

**擬改寫內容**

```md
1. 建立一個獨立的 skill 插件 repo
2. 透過 Claude Code 的插件系統安裝：
   ```bash
   # 先註冊 marketplace
   /plugin marketplace add your-org/claude-skills-pack

   # 再安裝
   /plugin install <skill-name>@your-org-claude-skills-pack
   ```

   官方 marketplace 上的插件可直接安裝：

   ```bash
   /plugin install code-review@claude-plugins-official
   ```
```

**狀態**：✅ 討論完成，待作者採用

---

### ✅ Tier B #8 — 05_API.md line 152：Haiku 3 退役日期差一天

**問題摘要**
原文：「Claude Haiku 3 **2026-04-19 退役**」
官方退役日期為 **April 20, 2026**，差一天。

**獨立查證**（2026-04-22，`platform.claude.com/docs/en/about-claude/model-deprecations`）

官方表列：
| API Model Name | Retirement Date |
|---|---|
| `claude-3-haiku-20240307` | **April 20, 2026** |

且 Deprecation history 明示：「On February 19, 2026, Anthropic notified developers... Retirement Date: April 20, 2026」。

**實際影響**
今天（2026-04-22）Haiku 3 已退役兩天；依文章規劃 migration cut-off 為 04-19 的團隊，理論上提前一天切，影響小。但發文時機若落在 2026-04-19 當日，讀者會誤以為「今天就退役」而不是「明天」，造成 API 404 追查時間損失。

**擬改寫內容**

```md
> 另外：Claude Sonnet 4 / Opus 4 已 deprecated，**2026-06-15 退役**；
> Claude Haiku 3 **2026-04-20 退役**（已於 2026-02-19 通知）。
```

**狀態**：✅ 討論完成，待作者採用

---

### ✅ Tier B #9 — 02_CHAT.md line 165–166：檔案上傳上限過時

**問題摘要**
原文：
- 單次上傳上限：~20 MB
- 單次對話最多上傳 5 個檔案

**實際官方數值**：
- 單次上傳上限：**30 MB / 檔**
- 單次對話最多：**20 檔**

**獨立查證**（2026-04-22）
- [Claude Help Center — Uploading files](https://support.claude.com/en/articles/8241126-uploading-files-to-claude)：明列 30 MB per file、20 files per conversation
- 適用所有方案（Free / Pro / Max / Team / Enterprise）

**實際影響**
原文把實際能力**低估**（20MB vs 30MB、5 檔 vs 20 檔）。讀者會誤以為 Claude.ai 做不到本可做到的工作（例：上傳大 PDF、多張設計稿比對）。

**擬改寫內容**

```md
**限制：**
- 單次上傳上限：30 MB / 檔
- 單次對話最多上傳 20 個檔案
- 無法執行上傳的程式碼（執行功能限於 Artifacts）

> 檔案更大或數量更多，改用 Claude Files API（單檔上限 500 MB）。
```

**狀態**：✅ 討論完成，待作者採用

---

### ✅ Tier B #10 — 01_GENERAL.md line 84：Anthropic Labs 描述需補來源

**問題摘要**
原文：「Anthropic Labs 是 Anthropic 內部的實驗產品孵化器，**2024 年中由 2 人起步**...」
驗證報告標 ❌ Unsupported，認為此描述**無官方出處**。

**獨立查證**（2026-04-22，補查後更正判斷）
- Anthropic **官方公告** Labs 是在 **2026-01**（對外重組與擴編宣布）
- **「mid-2024 two people」此敘述有二手媒體來源**：[TechBuzz.ai：「launched quietly in mid-2024 with just two people」](https://www.techbuzz.ai/articles/anthropic-reshuffles-leadership-to-expand-ai-labs-unit)
- 相關報導亦散見 Lenny's Newsletter（Mike Krieger 訪談）、NewYorkToday.net 等

**決議建議**
原文敘述**有媒體出處**，只是文章未標註。不需刪除，但再版建議**補加參考連結**以提高可追溯性。驗證報告「❌ Unsupported」可改判為「⚠️ Needs citation」。

**擬改寫內容**

```md
**Anthropic Labs** 是 Anthropic 內部的實驗產品孵化器，
2024 年中由 2 人起步[^labs-origin]，現已擴編為正式團隊：

[^labs-origin]: 出自 TechBuzz 2026-01 報導《Anthropic Reshuffles Leadership
to Expand AI Labs Unit》；Anthropic 官方於 2026-01 重組公告後才對外揭露 Labs 存在。
```

**狀態**：✅ 討論完成，待作者採用

---

## 追加發現（討論中新發現、不在原 Top 10）

### ✅ 追加 #1 — 01_GENERAL.md line 245–259：Max 方案「兩層限額」缺時間脈絡

**問題摘要**
原文敘述技術上正確（採納 2025-11-24 Anthropic 公告的新機制），但**缺少時間脈絡**與「新舊機制差異」說明，導致熟悉舊機制（2025-11-24 前）的讀者（如本次討論用戶）產生衝突記憶。

驗證報告**未掃到此條**（因為文章陳述本身不算「硬性錯誤」，只是資訊不完整）。

**獨立查證**（2026-04-22）

2025-11-24 Anthropic 官方公告逐字：
> "We've increased your limits and **removed the Opus cap**, so you can use
> Opus 4.5 up to your overall limit. **Sonnet now has its own limit**—it's
> set to match your previous overall limit, so you can use just as much as
> before."

時間軸對照：
- **2025-11-24 前**：overall limit + Opus 專屬 cap；Sonnet only 狀態仍扣 overall、僅降速
- **2025-11-24 後**：移除 Opus cap，改為 Sonnet 獨立 limit（匹配原 overall）

且官方公告與 Support docs 存在不一致（[claude-code issue #12487](https://github.com/anthropics/claude-code/issues/12487)，截至 2026-04 未獲官方回應）：
- 公告說「Sonnet now has its own limit」（獨立）
- Support docs 說「usage limits shared across Claude and Claude Code」（共用）

**實際影響**
- 老用戶看新機制敘述會以為記憶出錯
- 新用戶若查到 Support docs 會誤以為 Sonnet only 仍扣 overall
- 兩派讀者都會對「到底哪個版本是真的」產生不信任

**決議建議**
補時間脈絡註記 + Support docs 不一致警告，讓讀者知道：
1. 新機制從 2025-11-24 起
2. 舊記憶不是錯，是過時
3. 官方文件本身有不一致，以系統實際顯示為準

**擬改寫內容**

```md
### Max 方案的兩層限額

Max 方案有**兩個獨立的每週限額**（2025-11-24 起生效的新機制）：

[原 ASCII 圖表，第二層標示「獨立配額」「匹配先前的 overall 額度」]

overall 耗盡後，Claude Code 會自動進入「Sonnet only」狀態，
使用**獨立的 Sonnet 配額**（不再扣 overall）。

> **歷史脈絡**：2025-11-24 前，Max 方案是「overall + Opus cap」結構；
> Sonnet only 狀態仍扣 overall、僅降速。2025-11-24 Anthropic 公告移除
> Opus cap、改為 Sonnet 獨立限額。
>
> ⚠️ **2026-04 注意**：官方公告與 Support docs 曾存在不一致
> （見 claude-code#12487，截至 2026-04 未獲官方回應）。
> 實務上請以系統顯示的配額數字為準。
```

**狀態**：✅ 討論完成、已套用至原文

---

### ✅ 追加 #2 — 04_CODE.md line 2352–2379：第三方 Harness 政策需補 OpenClaw 反向宣稱註

**問題摘要**
原文描述的 2026-04-04 Anthropic 政策（訂閱額度不涵蓋第三方 harness）**技術上正確**，但網路流傳 [OpenClaw docs](https://docs.openclaw.ai/providers/anthropic) 的一段宣稱「Anthropic staff told us OpenClaw-style Claude CLI usage is allowed again」，讀者接觸到此說法會誤以為政策已反轉。

驗證報告**未掃到此條**（04_CODE.md 的「方案與算力政策異動」段落屬 scoped 驗證外）。

**獨立查證**（2026-04-22）

OpenClaw docs 的精確範圍：
> "Anthropic staff told us OpenClaw-style Claude CLI usage is allowed again,
> so OpenClaw treats **Claude CLI reuse and `claude -p` usage** as sanctioned
> for this integration unless Anthropic publishes a new policy."

拆解：
- 範圍限於**官方 Claude CLI subprocess reuse**、`claude -p` pipe 模式
- **不是**訂閱 token 直接授權給第三方 harness
- 末句自留免責條款「除非 Anthropic 公布新政策」
- OpenClaw 自家都推 API key：「For long-lived gateway hosts, Anthropic API keys are still the clearest and most predictable production path.」

Anthropic 立場（2026-04-22 為止）：
- **無任何反轉公告**
- 2026-04-21 Pro 方案反而移除 Claude Code 功能（政策**繼續收緊**）
- The Register、VentureBeat、DEV、Medium 等主流報導方向一致

**實際影響**
- 讀者看 OpenClaw docs 可能誤以為「第三方 harness 訂閱接入又開放」
- 真實情況是「OpenClaw 單邊解讀某個狹窄技術路徑被默許」
- 若依此推斷生產環境用法，可能踩雷（Anthropic 可隨時變更默許範圍）

**決議建議**
在原章節末補一段「灰色地帶補充」，說明：
1. OpenClaw docs 的精確範圍
2. 這是單邊宣稱、非 Anthropic 正式反轉
3. 2026-04-21 政策反而更緊
4. 生產環境仍建議用 API key

**擬改寫內容**

```md
> ⚠️ **灰色地帶補充（2026-04-22）**：OpenClaw 官方 docs 宣稱
> 「Anthropic staff told us OpenClaw-style Claude CLI usage is allowed
> again」，範圍限於**官方 Claude CLI subprocess reuse** 與 `claude -p`
> pipe 模式（**不是**訂閱 token 直接授權給第三方 harness）。
>
> 此為 OpenClaw **單邊宣稱**，Anthropic **未發布任何反轉公告**；
> 且 **2026-04-21 Pro 方案反而進一步移除 Claude Code 功能**，政策方向
> 仍為**繼續收緊**。
>
> **結論**：以 Anthropic 官方最新公告為準；生產環境建議用 **API key**，
> 不要依賴 OpenClaw docs 這條非正式說法。
```

**狀態**：✅ 討論完成、已套用至原文

---

### ✅ 追加 #3 — 04_CODE.md：補 Session Recap（v2.1.108 新 feature）

**問題摘要**
Claude Code v2.1.108（2026-Q2）引入 **Session Recap**：切回 terminal 時自動顯示一句話 recap（做了什麼、卡在哪、下一步）。原文**完全沒提**此 feature。

此為 **新增 feature 補遺**，非原文錯誤；驗證報告也不會掃到「缺漏」。

**獨立查證**（2026-04-22）

官方 [interactive-mode 文件 — Session recap](https://code.claude.com/docs/en/interactive-mode#session-recap) 逐字：
> "When you return to the terminal after stepping away, Claude Code shows
> a one-line recap of what happened in the session so far. The recap
> generates in the background **once at least three minutes have passed
> since the last completed turn and the terminal is unfocused**... Recaps
> only appear once the session has **at least three turns**, and **never
> twice in a row**."
>
> "Session recap is on by default for every plan and provider. The recap
> is always skipped in non-interactive mode."

官方 [env-vars 文件](https://code.claude.com/docs/en/env-vars) 確認：
- `CLAUDE_CODE_ENABLE_AWAY_SUMMARY=0` → 強制關
- `CLAUDE_CODE_ENABLE_AWAY_SUMMARY=1` → 強制開

版本脈絡（[v2.1.108 release notes](https://github.com/anthropics/claude-code/releases/tag/v2.1.108)）：
- v2.1.108 引入、一般用戶預設開
- v2.1.110 擴展至 Bedrock / Vertex / Foundry / `DISABLE_TELEMETRY` 環境
- 現在全平台、全 provider default on

**決議建議**
採用「兩處同動」：
1. `## Slash Commands` → `### 資訊查詢` 表格補 `/recap` 一行（讀者查 slash command 找得到）
2. `## 互動模式的 Shell 環境` 底下新增 `### Session Recap（切回 terminal 自動摘要）` 詳細介紹

詳細段落涵蓋：demo 範例、5 條自動觸發條件、4 種手動/設定方式、版本脈絡、**重要釐清**（不是 cross-session 記憶、不取代 CLAUDE.md、核心價值在降低 entry cost）。

**狀態**：✅ 討論完成、已套用至原文

---

## 跨文件通用 Corrections（來自 SUMMARY.md 附錄）

### 📌 通用 #1 — `anthropic.com/pricing` → `claude.com/pricing`

**影響檔案**：01_GENERAL.md、05_API.md、08_MANAGED_AGENTS.md

**問題**：`anthropic.com/pricing` 會 301 重導至 `claude.com/pricing`。直接寫新 URL 可省一次 redirect、且符合 2026 年品牌調整。

**建議**：全檔 search & replace `anthropic.com/pricing` → `claude.com/pricing`。

---

### 📌 通用 #2 — Opus 4.7 API 參數 breaking change 需註記

**影響檔案**：05_API.md、07_SKILL.md、01_GENERAL.md

**問題**：Opus 4.7 對 `temperature`、`top_p`、`top_k` 參數的非預設值會回 400 錯誤。遷移 4.6 → 4.7 若未同步移除這些覆寫，服務會壞。

**建議**：在 05_API.md 的 Opus 4.7 遷移注意段落補一條：
> 非 1.0 的 `temperature`、非預設 `top_p`/`top_k` 會回 400；升級前確認移除這些覆寫。

---

### 📌 通用 #3 — Tokenizer 換算需加註新估算倍數

**影響檔案**：跨全檔（凡提到「0.75 英文字 / 0.5 中文字 per token」的地方）

**問題**：4.6 前近似值；4.7 新 tokenizer 壓縮比較低，相同文本多消耗 1.0–1.35× tokens。

**建議**：凡出現舊換算值處加註：
> 以上為 4.6 以前近似值；4.7 新 tokenizer 需多預估 1.0–1.35× token 量。

---

## 未處理段落（後續 session 補）

以下項目不在本次 Top 10 處理範圍，留給下一輪：

1. **07_SKILL.md Line 295–301**：「skill-creator 打包成 `.skill` 檔案」 — **無此檔案格式**，需改寫（驗證報告 claim #17）
2. **07_SKILL.md Line 65–67**：三層載入機制描述 — Metadata 字數上限應寫 1,536 字元（驗證報告 claim #7）
3. **07_SKILL.md Line 40–41**：`/compact`、`/help` 誤列為 bundled skill — 應區分「內建指令」與「bundled skills」（驗證報告 claim #6）
4. **06_MCP.md Line 133 & 137**：Claude.ai 方案規則 + Settings 命名（Integrations → Connectors）— 驗證報告 claim #15、#16
5. **04_CODE.md scoped 未驗證段落**：Chrome、Figma MCP、Hooks、Routines、Plugins、Desktop、Subagent
