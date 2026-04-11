[← 上一篇：Cowork](03_COWORK.md) | [下一篇：API →](05_API.md)

# Claude Code CLI

> **知識截止日期：** 2026 年 4 月  
> 官方文件：[code.claude.com/docs](https://code.claude.com/docs)

---

## 安裝與初始設定

### 安裝

**Native Install（官方推薦，自動更新）：**

```bash
# macOS / Linux / WSL
curl -fsSL https://claude.ai/install.sh | bash

# Windows PowerShell
irm https://claude.ai/install.ps1 | iex
```

其他安裝方式（不自動更新，需手動更新）：

```bash
# Homebrew（macOS）
brew install --cask claude-code

# WinGet（Windows）
winget install Anthropic.ClaudeCode
```

> **npm 安裝已 deprecated**（2026 年 1~2 月宣布），官方理由：Native Installer 更快、不需要 Node.js 依賴、支援背景自動更新。2026/03 另發生兩個獨立的 npm 安全事件（Anthropic source map 外洩 + Axios 供應鏈攻擊），事件在棄用之後，未直接導致棄用決定，但進一步說明了遷移的必要性。
>
> 詳細背景見下方「npm 外洩事件始末」段落。
>
> 若仍在用 npm 版本，可執行以下指令遷移：
> ```bash
> curl -fsSL https://claude.ai/install.sh | bash
> npm uninstall -g @anthropic-ai/claude-code
> ```

```bash
# 確認安裝
claude --version
```

### 認證

```bash
claude auth login
# 開啟瀏覽器完成 OAuth 登入

# 確認登入狀態
claude auth status

# 登出
claude auth logout
```

### 健康檢查

```bash
claude doctor
# 檢查安裝、認證、設定是否正常
```

### LLM Gateway（第三方模型介接）

Claude Code 支援透過 LLM Gateway 介接其他模型提供商，包括地端（本地）模型。閘道需實作 Anthropic Messages API 格式，因此通常需要搭配 LiteLLM 等代理層進行 API 轉換。

主要用途為企業的集中認證管理、用量追蹤、成本控管與稽核日誌。

> 官方文件：[LLM gateway configuration](https://code.claude.com/docs/en/llm-gateway)

---

## CLAUDE.md 設定

`CLAUDE.md` 是給 Claude Code 看的「專案說明書」，讓 Claude 了解你的專案慣例。每次啟動 session 時自動載入。

### 檔案位置

| 位置 | 作用範圍 |
|------|---------|
| `~/.claude/CLAUDE.md` | 全域（所有專案） |
| `<repo根目錄>/CLAUDE.md` | 專案層級（已提交，團隊共用） |
| `.claude/CLAUDE.md` | 專案層級（本機，加入 .gitignore） |

**載入優先順序：** 全域 → 專案 → 本機（後者可覆蓋前者）

> **CLAUDE.md vs Hooks**（Boris Cherny 的建議）：CLAUDE.md 是「建議性的」，Claude 遵守機率約 80%。Hooks 是確定性的，100% 執行。**若某件事必須每次都發生、不能有例外，用 hooks 而非 CLAUDE.md。**

### 撰寫範例

```markdown
# 專案指引

## 技術堆疊
- Python 3.12 + FastAPI
- PostgreSQL 16（ORM: SQLAlchemy）
- 測試：pytest + pytest-asyncio

## 程式碼風格
- 遵循 PEP 8
- 函式長度不超過 50 行
- 使用 type hints

## 命名慣例
- 變數/函式：snake_case
- 類別：PascalCase
- 常數：UPPER_SNAKE_CASE
- API endpoint：kebab-case

## 工作流程
- 所有修改需先寫測試（TDD）
- commit message 格式：feat/fix/refactor: description
- PR 說明需包含 Why、What Changes、Testing

## 禁止事項
- 不要在程式碼中硬寫 API key 或密碼
- 不要提交 .env 檔案
```

---

## CLI 基本操作

### 啟動方式

```bash
claude                          # 進入互動式 REPL
claude "幫我 review 這段程式碼"  # 帶初始 prompt 啟動
claude -p "解釋這個函式"          # 執行後直接退出（non-interactive）
claude -c                       # 恢復最近一次 session
claude -r                       # 互動式選擇 session 恢復
claude -r "名稱或ID" "繼續"      # 直接恢復指定 session
```

### 完整 CLI 指令參考

#### 基本操作

| 指令 | 說明 |
|------|------|
| `claude` | 啟動互動式 REPL |
| `claude "prompt"` | 帶初始 prompt 啟動 |
| `claude -p "prompt"` | 執行並退出（pipe 友善） |
| `claude -c` | 恢復最近 session |
| `claude -r` | 互動式選擇器，挑選要恢復的 session |
| `claude -r <id 或名稱> "prompt"` | 直接恢復指定 session |
| `claude update` | 更新到最新版本 |

#### 認證管理

| 指令 | 說明 |
|------|------|
| `claude auth login` | 登入（支援 `--email`、`--sso`） |
| `claude auth logout` | 登出 |
| `claude auth status` | 查看登入狀態 |

#### 工具與整合

| 指令 | 說明 |
|------|------|
| `claude mcp` | 管理 MCP servers |
| `claude mcp add <name> -- <cmd>` | 新增 MCP server |
| `claude mcp list` | 列出 MCP servers |
| `claude mcp remove <name>` | 刪除 MCP server |
| `claude agents` | 列出已設定的 subagents |
| `claude doctor` | 檢查安裝健康狀態 |
| `claude remote-control` | 透過 claude.ai 控制 session |

### Permission Modes（權限模式）

控制 Claude 執行操作時是否需要你逐一確認：

| 模式 | 說明 |
|------|------|
| `default` | 每次工具呼叫都詢問 |
| `acceptEdits` | 自動接受檔案編輯，其餘仍詢問 |
| `plan` | 只規劃不執行，所有操作需你確認 |
| `bypassPermissions` | 跳過所有確認，完全自動執行 |

**切換方式：**

```bash
# 啟動時指定模式
claude --permission-mode plan

# session 中按 Shift+Tab 循環切換模式
# default → acceptEdits → plan → bypassPermissions
```

**`--dangerously-skip-permissions`**

等同於啟動時直接進入 `bypassPermissions` 模式，Claude 不會詢問任何操作確認，全自動執行：

```bash
claude --dangerously-skip-permissions
```

> **注意：** 適合在已知安全的自動化腳本中使用。日常互動不建議開啟，Claude 可能在未確認的情況下修改、刪除檔案。

### Sandbox 模式

Sandbox 讓 Claude Code 的 Bash 指令在**作業系統層級的隔離環境**中執行，限制檔案系統和網路存取，防止意外破壞。

**平台支援：**

| 平台 | 隔離技術 | 狀態 |
|------|---------|------|
| **Linux** | bubblewrap | 支援 |
| **macOS** | Seatbelt | 支援 |
| **Windows** | — | 不支援（官方標示 planned） |

**啟用方式：**

```bash
/sandbox        # 在 session 中切換
```

或在 `settings.json` 中設定：
```json
{
  "sandbox": {
    "enabled": true
  }
}
```

**Windows 替代方案：**
- **WSL 2**：在 WSL 2 內跑 Claude Code，安裝 `bubblewrap` 即可使用 sandbox（WSL 1 不支援）
- **Docker**：用 Docker 容器提供隔離
- **deny rules**：在 `settings.json` 設定檔案路徑 deny 規則，手動保護敏感目錄

> 官方文件：[Sandboxing - Claude Code Docs](https://code.claude.com/docs/en/sandboxing)

---

## Slash Commands

在 Claude Code session 中輸入 `/` 開頭的指令。分為三種類型：

| 類型 | 說明 | 範例 |
|------|------|------|
| **Built-in 指令** | 邏輯寫死在 Claude Code 裡，固定行為 | `/clear`、`/model`、`/status` |
| **Bundled Skills** | 隨 Claude Code 附帶的 prompt-based skill | `/simplify`、`/batch`、`/loop` |
| **Custom Skills** | 你自己或團隊建立的 SKILL.md 檔案 | `/prepare-pr`、`/deploy` |

**判斷一個指令是哪種：**
- 輸入 `/skills` → 只列出 Skills（Bundled + Custom）
- 查 [官方 commands 文件](https://code.claude.com/docs/en/commands) → 只有 Built-in 指令
- 差異本質：Built-in 執行固定邏輯；Skills 讓 Claude 依照 prompt 自行規劃並執行，可以動態讀檔、spawn 子 agent

### 實際範例：同一個功能的兩種定義方式

這個專案同時有 `/spectra:ask` 和 `/spectra-ask`，兩者內容幾乎相同，差別在於：

| | `/spectra:ask` | `/spectra-ask` |
|---|---|---|
| **來源** | `.claude/commands/spectra/ask.md` | `.claude/skills/spectra-ask/SKILL.md` |
| **命名規則** | 子目錄 `spectra/` + 檔名 `ask` → 用 `:` 連接 | 目錄名稱直接成為指令名 |
| **格式** | Commands（舊格式） | Skills（新格式） |
| **工具限制** | 無 | `disallowedTools: [Edit, Write]`（禁止修改檔案） |

Skills 比 Commands 多出的 frontmatter 能力：
- `disallowedTools` / `allowed-tools`：限制或授權可用工具
- `disable-model-invocation`：防止 Claude 自動觸發
- `context: fork`：在獨立子 agent 執行
- `paths`：只在特定檔案路徑下自動觸發

> 執行 `/help` 可查看當前 session 所有可用指令（含自訂 skill 和 MCP 指令）

### 初始化與設定

| 指令 | 說明 |
|------|------|
| `/init` | 為目前專案產生 `CLAUDE.md`，設定 `CLAUDE_CODE_NEW_INIT=1` 可啟用互動式引導流程（含 skills、hooks、memory 設定） |
| `/config` | 開啟設定介面（主題、模型、輸出格式等）。別名：`/settings` |
| `/permissions` | 管理工具存取權限的 allow / ask / deny 規則 |
| `/memory` | 編輯 CLAUDE.md、管理 auto-memory |
| `/keybindings` | 開啟或建立快捷鍵設定檔 |
| `/terminal-setup` | 為目前終端機安裝 Shift+Enter 等快捷鍵綁定（VS Code、Alacritty、Warp 等需要） |

### Session 管理

| 指令 | 說明 |
|------|------|
| `/clear` | 清除對話歷史，釋放 context。別名：`/reset`、`/new` |
| `/compact [指示]` | 壓縮對話歷史，可附加保留重點的指示 |
| `/resume [session]` | 恢復指定 session（ID 或名稱），不填則開啟選擇器。別名：`/continue` |
| `/rewind` | 把對話和程式碼還原到先前某個時間點。別名：`/checkpoint` |
| `/fork` | 從目前狀態建立對話分支。別名：`/branch` |
| `/rename [名稱]` | 重新命名目前 session，不填則自動產生名稱 |
| `/export [檔名]` | 將對話匯出為純文字 |

### 資訊查詢

| 指令 | 說明 |
|------|------|
| `/help` | 列出所有可用指令 |
| `/status` | 顯示版本、模型、帳號、連線狀態（Claude 執行中也可用） |
| `/cost` | 查看目前 session 的 token 用量 |
| `/context` | 視覺化顯示 context 用量與警告 |
| `/skills` | 列出可用的 skills |
| `/stats` | 視覺化顯示每日用量、session 歷史、連續使用紀錄 |
| `/insights` | 分析你所有歷史 session，生成個人化 HTML 報告（使用模式、成就、摩擦點、建議功能），存放在 `~/.claude/usage-data/` |
| `/powerup` | 互動式教學系統——18 個課程（每課 3-10 分鐘），有動畫 demo，涵蓋 context 管理、Hooks、MCP、sub-agents、`/loop` 等。所有方案可用 |
| `/doctor` | 診斷安裝與設定是否正常 |

### 開發工具

| 指令 | 說明 |
|------|------|
| `/diff` | 互動式查看未提交的變更與每個 turn 的 diff |
| `/plan [描述]` | 進入計畫模式，先規劃再執行 |
| `/security-review` | 掃描目前分支的安全漏洞 |
| `/pr-comments [PR]` | 取得 GitHub PR 的留言（需要 `gh` CLI） |
| `/mcp` | 管理 MCP servers 連線（含啟用 computer-use） |
| `/agents` | 管理 agent 設定 |

### 側問與工具

| 指令 | 說明 |
|------|------|
| `/btw <問題>` | 快速問一個不影響主對話歷史的側問（Claude 執行中也可用），回答後自動消失 |
| `/chrome` | 管理 Claude in Chrome 瀏覽器整合（連線、權限、設為預設）；詳見下方「Claude in Chrome」段落 |
| `/voice` | 切換語音輸入（push-to-talk） |
| `/copy [N]` | 複製最後一個回應到剪貼簿，`/copy 2` 複製倒數第二個 |
| `/rc` | 讓此 session 可從 claude.ai 遠端控制。別名：`/remote-control` |

### 外觀

| 指令 | 說明 |
|------|------|
| `/model` | 切換 AI 模型 |
| `/effort [low\|medium\|high\|max]` | 調整推理深度（捷徑：在 prompt 中輸入 `think` 自動設為 high，讓 Claude 先思考再行動） |
| `/theme` | 切換色彩主題 |
| `/vim` | 切換 Vim 編輯模式 |
| `/fast [on\|off]` | 切換快速模式 |

### 進階（Bundled Skills）

| 指令 | 說明 |
|------|------|
| `/batch` | 並行執行大規模變更 |
| `/loop <間隔> <任務>` | 定時重複執行任務（e.g., `/loop 5m check deployment status`） |
| `/schedule` | 建立雲端排程任務（關機也能執行） |
| `/simplify` | 對已變更程式碼做品質改善分析 |
| `/feedback` | 回報問題給 Anthropic |

> `/loop` 與 `/schedule` 的詳細說明見下方「排程系統」段落。  
> `/batch` 的詳細說明見下方「/batch 大規模並行變更」段落。

---

## 互動模式的 Shell 環境

### Shell 環境說明

Claude Code 在各平台上使用的 shell：

| 平台 | 內部 Shell | 說明 |
|------|-----------|------|
| **Windows** | Git Bash | 不是 PowerShell 也不是 CMD，需安裝 Git for Windows |
| **macOS** | Bash | 即使系統預設是 zsh（Catalina 後），Claude Code 仍用 bash |
| **Linux** | Bash | 同上，不繼承使用者的預設 shell 設定 |

> macOS / Linux 使用者注意：如果你的工具、PATH、環境變數設定在 `.zshrc` 裡，Claude Code 的 Bash 工具可能看不到這些設定。可在 `settings.json` 加入 `CLAUDE_CODE_SHELL` 覆蓋：
>
> ```json
> {
>   "env": {
>     "CLAUDE_CODE_SHELL": "/bin/zsh"
>   }
> }
> ```

**Windows 特別說明：**

```
你的終端機（PowerShell / CMD）
  └── 啟動 claude
        └── 內部使用 Git Bash 執行所有 shell 指令
```

- 路徑使用正斜線 `/`（不是 `\`）
- Unix 指令（`ls`、`cat`、`grep`）都能用
- Windows 專屬指令（`dir`、`ipconfig`）行為可能不同

### `!` 前置字元（Bash Mode）

在互動模式下，輸入 `!` 開頭可直接執行 shell 指令，**不需要 Claude 審核或批准**：

```bash
! git status
! ls -la
! npm test
```

**重要特性：**

| 特性 | 說明 |
|------|------|
| **Claude 看得到輸出** | 指令結果會加入對話 context，Claude 可讀取並參考 |
| **無法互動輸入** | 不支援需要輸入密碼、確認等互動操作（會卡住） |
| **即時顯示** | 輸出即時顯示，支援長時間執行的指令 |
| **可背景執行** | 長指令可按 `Ctrl+B` 移到背景繼續跑 |

**無法使用 `!` 的情境：**
```bash
! sudo apt install xxx   # 需要輸入密碼 → 不行
! git commit             # 會開啟 editor → 不行，改用 git commit -m "..."
! ssh user@host          # 互動式 SSH → 不行
```

### `@` 前置字元（檔案路徑補全）

輸入 `@` 觸發檔案路徑自動補全，在 prompt 中直接引用檔案：

```
@src/auth/login.ts 這個函式有什麼問題？
```

---

## Skills

Skill 是以 Markdown 格式定義的**可重用工作流程**，讓你把常用流程封裝成 slash command，一次定義、反覆使用。

### 儲存位置

| 層級 | 路徑 | 作用範圍 |
|------|------|---------|
| **全域** | `~/.claude/skills/` | 所有 session |
| **專案** | `.claude/skills/` | 只在此 repo |

### 使用方式

```bash
/skill-name              # 直接呼叫
/skill-name argument     # 帶參數呼叫
```

也可以透過自然語言觸發：Claude 判斷情境後自動套用對應的 skill。

### 建立 Skill

在 `.claude/skills/` 建立 Markdown 檔案：

```markdown
---
name: prepare-pr
description: 分析 git diff 並準備 PR 說明
---

## 流程
1. 執行 `git diff main...HEAD`
2. 分析變更範圍
3. 生成 PR 標題與說明

## Guardrails
- 不直接 push，只生成內容
```

> 詳細說明、最佳實踐、團隊共用方式請參考 `07_SKILL.md`

---

## Hooks 系統

Hooks 讓你在 Claude Code 的工具執行前後自動觸發 shell 指令。

### 設定位置

`~/.claude/settings.json`（全域）或 `.claude/settings.json`（專案）

### Hooks 類型

| 類型 | 觸發時機 |
|------|---------|
| `PreToolUse` | 工具執行**前** |
| `PostToolUse` | 工具執行**後** |
| `Stop` | Session 結束時 |

### 設定範例

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write \"$CLAUDE_TOOL_INPUT_FILE_PATH\""
          }
        ]
      }
    ],
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "grep -r 'console.log' --include='*.ts' ."
          }
        ]
      }
    ]
  }
}
```

**這個設定的實際效果：**

| Hook | 觸發時機 | 效果 |
|------|---------|------|
| `PostToolUse` + `matcher: "Edit"` | **每次 Claude 編輯任何檔案後** | 自動執行 `prettier`，確保格式永遠一致，Claude 不需要手動整理縮排或風格 |
| `Stop` | **Session 結束時** | 掃描所有 `.ts` 檔案是否有 `console.log`，輸出結果提醒你在 commit 前清理 |

> **關鍵差異（對照 CLAUDE.md）：** CLAUDE.md 寫「不要加 console.log」，Claude 遵守約 80%。改成 Stop hook 後，即使 Claude 不小心留了，你也一定會在 session 結束時看到警告——100% 確定不漏。

**Hooks 是否消耗額度？**

| Hook 類型 | 輸出去哪 | 消耗額度？ |
|-----------|---------|-----------|
| `PreToolUse` | 輸出進 Claude context（工具執行前） | **會** |
| `PostToolUse` | 輸出進 Claude context（工具執行後） | **會** |
| `Stop` | Session 結束後才執行，只顯示在終端 | **不會** |

實務上影響有限——hook 輸出通常只有幾行。但若 hook 輸出非常冗長（如 lint 噴出數百行），就會明顯消耗 context。建議讓 hook 保持輸出簡潔，或在指令後加 `> /dev/null 2>&1` 靜默不重要的輸出。

---

## /batch 大規模並行變更

> **推出版本：** Claude Code v2.1.63

`/batch` 是針對「跨大量檔案執行相同類型變更」設計的 Bundled Skill。它先訪談你了解需求，再把工作**分解成多個獨立單元，並行執行**——規模可達數十、數百，甚至**數千個 agent**，速度比逐一送 prompt 快數倍。

### 運作原理

```
/batch <任務描述>
  → Orchestrator 進入 plan mode
  → 派出 Explore agents 深入分析受影響範圍
  → 把工作拆解成 5～30 個自包含單元
  → 你審核計畫後確認
  → 每個單元各自在獨立 git worktree 中執行（防止互相干擾）
  → 結果合併，衝突浮出供你 review
```

每個 agent 運作在自己的 branch 與 working copy，互不影響。完成後你必須 review diff 才能 commit。

### 適合情境

| 情境 | 範例 |
|------|------|
| 函式庫遷移 | 把所有 `axios` 換成 `fetch` |
| API 變更 | 更新所有呼叫舊 API 的地方 |
| 慣例強制 | 在所有資料庫呼叫加上 `try/catch` |
| 測試生成 | 為 `/src/api` 下所有沒有測試的函式補測試 |
| 命名規範 | 把所有 snake_case 函式名改成 camelCase |

### 使用要點

- **明確範圍**：指定具體目錄或檔案類型（e.g., `所有 /src/api 下的 TypeScript 檔案`）
- **單一關注點**：一次只做一種變更，不要混合不同性質的修改
- **適合可並行的工作**：每個檔案的變更必須彼此獨立，不能有跨檔案的相依順序

### 與直接下指令的差異

`/batch` 對每個檔案套用 Claude 的推理能力，而不是固定的文字替換 pattern——能處理各檔案間的細微上下文差異，例如同一個函式在不同檔案中有不同參數，也能各自正確處理。

---

## Subagent 系統

Subagent 是在**獨立 context window** 中執行的專業 AI 助理。Claude 根據 subagent 的 `description` 自動判斷何時委派任務，subagent 執行完畢後只把最終結果回傳給主對話——中間的工具呼叫、context 消耗全部隔離在 subagent 內部。

```
主對話 context                    Subagent context
┌─────────────────────┐           ┌────────────────────────┐
│ 你的對話歷史         │           │ 全新 context           │
│ ...                 │           │ system prompt = 你的定義│
│                     │           │ 工具限制 = 你設定的     │
│ → 委派任務 ─────────┼──────────▶│ 執行任務...            │
│                     │           │ 工具呼叫（不佔主 ctx）  │
│ ← 收到最終結果 ◀────┼───────────│ 回傳最終訊息           │
└─────────────────────┘           └────────────────────────┘
```

**與 Skill 的差異：**

| | Subagent | Skill |
|---|---|---|
| **定義方式** | Markdown + YAML frontmatter | Markdown prompt |
| **儲存位置** | `~/.claude/agents/` 或 `.claude/agents/` | `~/.claude/skills/` 或 `.claude/skills/` |
| **執行方式** | 獨立 context window，Claude 自動委派 | 在主對話 context 中執行 |
| **工具控制** | frontmatter 設定 `tools` / `disallowedTools` | frontmatter 設定 `disallowedTools` |
| **適合用途** | 需要隔離 context、可指定不同模型 | 可重用的工作流程、slash command |

### 內建 Subagent

Claude Code 預設附帶五個內建 subagent（`/agents` 可查看）：

| Subagent | 模型 | 用途 |
|----------|------|------|
| **Explore** | Haiku | 快速搜尋與分析 codebase，唯讀，不修改檔案 |
| **Plan** | 繼承 | Plan Mode 下蒐集 context，避免無限 spawn |
| **general-purpose** | 繼承 | 複雜多步驟任務（需要探索＋修改） |
| **statusline-setup** | Sonnet | 設定 status line 顯示 |
| **claude-code-guide** | Haiku | 回答 Claude Code 相關問題 |

> **注意：** Subagent 不能再 spawn subagent（防止無限遞迴）。需要多 agent 並行互動請用 Agent Teams。

### 建立自訂 Subagent

**方法一：互動式（推薦）**

```bash
/agents
→ Create new agent → Personal（~/.claude/agents/）或 Project（.claude/agents/）
→ 輸入描述 → Claude 自動生成 system prompt
→ 選擇工具、模型、記憶設定
```

**方法二：手動建立檔案**

建立 `.claude/agents/code-reviewer.md`（或 `~/.claude/agents/`）：

```markdown
---
name: code-reviewer
description: Reviews code for quality, security, and best practices. Use proactively after writing or modifying code.
tools: Read, Glob, Grep, Bash
model: sonnet
---

You are a code reviewer. Analyze the code and provide
specific, actionable feedback on quality, security, and best practices.
```

> 手動新增後需重啟 session 或執行 `/agents` 才能載入。

### Frontmatter 欄位

| 欄位 | 必填 | 說明 |
|------|------|------|
| `name` | ✓ | 識別名稱（小寫+連字號） |
| `description` | ✓ | Claude 據此判斷何時委派，寫清楚很重要 |
| `tools` | — | 允許的工具白名單（省略則繼承全部） |
| `disallowedTools` | — | 工具黑名單（從繼承清單中移除） |
| `model` | — | `sonnet` / `opus` / `haiku` / 完整 model ID / `inherit`（預設） |
| `permissionMode` | — | `default` / `acceptEdits` / `auto` / `bypassPermissions` / `plan` |
| `maxTurns` | — | 最大 agentic turn 數 |
| `memory` | — | 跨 session 記憶：`user` / `project` / `local` |
| `isolation` | — | `worktree`：在獨立 git worktree 執行（執行完自動清除） |
| `background` | — | `true`：永遠在背景執行 |
| `color` | — | UI 顯示顏色（`red` / `blue` / `green` 等） |
| `hooks` | — | 專屬於此 subagent 的 lifecycle hooks |
| `skills` | — | 載入到此 subagent context 的 skills |
| `mcpServers` | — | 此 subagent 可用的 MCP servers |
| `effort` | — | 推理深度：`low` / `medium` / `high` / `max`（Opus 4.6 限定） |

### 儲存位置與優先順序

| 位置 | 範圍 | 優先順序 |
|------|------|---------|
| Managed settings（組織管理員部署） | 組織全域 | 1（最高） |
| `--agents` CLI flag | 當次 session | 2 |
| `.claude/agents/` | 當前專案 | 3 |
| `~/.claude/agents/` | 所有專案 | 4 |
| Plugin `agents/` 目錄 | Plugin 啟用範圍 | 5（最低） |

同名 subagent 以較高優先順序的定義為準。

### 查看與管理

```bash
/agents              # 互動式管理介面（建立、編輯、刪除）
claude agents        # CLI 列出所有 subagent（不啟動互動 session）
```

---

## 排程系統

Claude Code 提供三種排程方式，適合不同使用情境：

| | `/loop` | Desktop 本機排程 | `/schedule`（雲端） |
|---|---|---|---|
| **執行位置** | 本機，session 內 | 本機，Desktop app | Anthropic 雲端 |
| **持久化** | ✗（關 session 消失） | △（Desktop 需開著） | ✓（關機也能執行） |
| **最小間隔** | 1 分鐘 | 1 分鐘 | 1 小時 |
| **到期限制** | 3 天自動刪除 | 補跑（往回查 7 天） | 無 |
| **本機檔案存取** | ✓ | ✓ | ✗（僅限 GitHub repo） |
| **方案需求** | 所有方案 | 所有方案 | Pro 以上 |

### /loop

在當前 session 內定時執行任務，適合短期監控與自動化。

**語法：**

```bash
/loop <間隔> <任務描述>
```

間隔單位：`m`（分鐘）、`h`（小時）、`d`（天），最短 1 分鐘，**最長一週**。Boris Cherny（Claude Code 作者）本人有常駐的 loop 自動處理 code review、rebase、PR shepherd 等工作。

**範例：**

```bash
# 每 2 分鐘檢查 log 並摘要新錯誤
/loop 2m tail the last 20 lines of logs/server.log and summarize any new errors

# 每 5 分鐘確認 job queue 還有多少任務
/loop 5m check how many items are left in the job queue

# 每小時執行 /status 並回報
/loop 1h /status
```

**管理執行中的任務：**

底層工具（Claude 自動呼叫，也可直接說自然語言觸發）：

| 動作 | 說明 |
|------|------|
| `CronList` | 列出所有執行中的排程任務與 ID |
| `CronDelete` | 刪除指定 ID 的任務 |

或直接告訴 Claude：
```
列出所有排程任務
取消 job 79febfa1
/loop --cancel    # 取消目前的 loop
```

**注意事項：**
- 任務 3 天後自動到期（防止被遺忘的 loop 無限消耗 API 額度）
- 關閉 terminal 或結束 session 即刪除所有 loop
- 執行時間有最多 10%（上限 15 分鐘）的抖動（jitter）

### Desktop 本機排程

在 Claude Desktop 的 Cowork 或 Code 分頁建立，Desktop app 開著就會自動執行。支援本機檔案、自訂 MCP。  
任務定義儲存於 `~/.claude/scheduled-tasks/<task-name>/SKILL.md`。

系統休眠恢復後會補跑錯過的任務（往回查最多 7 天）。  
適合需要本機資源（SQLite、自訂 MCP）的長期自動化任務。

### /schedule（雲端）

由 Anthropic 雲端執行，不依賴你的電腦是否開機。

```bash
/schedule
```

或在 claude.ai 介面建立。每次執行都是全新的獨立 session，prompt 必須自給自足（不帶入前次 context）。  
僅支援 GitHub repo（不支援本機路徑）、MCP 只限預先設定的 Connectors。

**適合：** 定期產生報告、監控 GitHub PR/Issues、自動化程式碼審查等不需要本機環境的任務。

---

## Claude Code on the Web（雲端執行）

> **狀態：** Research Preview  
> **入口：** [claude.ai/code](https://claude.ai/code)（瀏覽器）、Claude iOS / Android App（手機）、**Claude Desktop Code 頁面**  
> **方案：** Pro / Max / Team / Enterprise

Claude Code on the Web 讓 Claude 在 **Anthropic 管理的雲端 VM** 上執行任務——不需要你的電腦開著。可以從瀏覽器、**手機 App**、**Claude Desktop 的 Code 頁面**、或 terminal 發起任務，任務跑完後送出 PR。手機特別適合在外出時派任、監看進度，或在 session 有問題時介入回覆。

> **Claude Desktop Code 頁面補充：** Desktop 的 Code 分頁同樣可以選擇 GitHub repo 連接雲端執行，不限於本機 terminal 工作流。

### 運作方式

```
你發起任務
  → Anthropic 雲端 VM clone 你的 GitHub repo
  → 執行 setup script（安裝依賴）
  → Claude Code 分析、修改程式碼、跑測試
  → 推送到分支
  → 你在 Web 介面查看 diff、建立 PR
```

### 設定

**方法一：從瀏覽器**

1. 前往 [claude.ai/code](https://claude.ai/code)
2. 連接 GitHub 帳號
3. 在你的 repo 安裝 **Claude GitHub App**
4. 選擇預設雲端環境
5. 輸入任務，送出

**方法二：從 terminal（需要 `gh` CLI）**

```bash
/web-setup
```

會自動把本地 `gh auth token` 同步到雲端、建立預設環境、在瀏覽器開啟 claude.ai/code。

### Diff View（上 PR 前預覽變更）

Claude 執行後，介面會顯示 `+12 -1` 等 diff 統計，點擊可開啟 diff 檢視器：

- 逐檔案確認變更內容
- 直接在 diff 上留言要求修改
- 反覆迭代後再建立 PR

不需要先開草稿 PR 才能 review，減少 GitHub 上的雜訊。

### Auto-fix（自動修 PR）

安裝 Claude GitHub App 後，可讓 Claude 監看 PR，自動回應 CI 失敗與 review 留言：

- CI check 失敗 → Claude 調查並自動推修復
- reviewer 留言 → 若意圖明確，Claude 直接改；若有歧義，先問你
- Claude 用**你的 GitHub 帳號**回覆 PR 留言，每條回覆標注「來自 Claude Code」

> ⚠️ 若你的 repo 有依賴留言觸發的自動化（Atlantis、Terraform Cloud 等），Claude 的留言也可能觸發這些 workflow，使用前先確認。

**開啟方式：**
- Web 介面建立的 PR → CI 狀態列點擊 **Auto-fix**
- 任何 PR → 把 PR URL 貼入 session，說「watch this PR and fix any CI failures」

### 在 Web 與 Terminal 之間移動任務

**Terminal → Web（雲端執行）：**

```bash
# 把任務推到雲端執行，本地繼續工作
claude --remote "Fix the authentication bug in src/auth/login.ts"

# 並行執行多個任務（各自獨立 session）
claude --remote "Fix the flaky test in auth.spec.ts"
claude --remote "Update the API documentation"
```

```bash
/tasks    # 查看所有背景 session 進度
```

**Web → Terminal（把雲端 session 拉回本地）：**

```bash
/teleport          # 互動式選擇 web session 拉回本地（別名 /tp）
claude --teleport  # 同上，從 terminal 啟動
```

拉回時 Claude 會自動 fetch 並 checkout 對應分支，載入完整對話歷史。

> **注意：** 方向是單向的——只能從 web 拉回 terminal，不能把既有 terminal session 推到 web。`--remote` 是建立全新的 web session。

**Teleport 前置條件：**

| 條件 | 說明 |
|------|------|
| 乾淨的 git 狀態 | 不能有未 commit 的變更（會提示 stash） |
| 正確的 repo | 必須在同一個 repo 的 checkout 下執行 |
| 分支已推送 | 雲端 branch 必須已推到 remote |
| 同一帳號 | 必須登入同一個 Claude.ai 帳號 |

### 雲端環境

預設映像（Ubuntu 24.04）已安裝常見工具鏈：Python、Node.js（LTS）、Ruby、PHP、Java、Go、Rust、C++、PostgreSQL 16、Redis 7.0 等。

**自訂環境（Setup Script）：**

在環境設定中填入 Bash script，每次建立新 session 時在 Claude Code 啟動前執行：

```bash
#!/bin/bash
apt update && apt install -y gh
npm install
```

> `setup script` 只跑一次（建立 session 時）；`SessionStart hook` 則每次 session 啟動都跑，兩者互補。

**設定方式（在 [claude.ai/code](https://claude.ai/code)）：**

1. 點選目前的環境名稱 → 開啟環境選擇器
2. **「Add environment」**（新增）或點選現有環境的**設定按鈕**（修改）
3. 填入：
   - 環境名稱
   - 網路存取層級（見下方）
   - 環境變數（`.env` 格式）
   - Setup Script

**網路存取：** 預設「Limited」模式，允許 npm、PyPI、GitHub 等常見套件來源；可設定為「Full」或「No internet」。

### 限制

- 只支援 **GitHub**（不支援 GitLab、Bitbucket；GitLab 和 Bitbucket 無法用雲端 session）
- 不支援 Auto Mode（Auto Mode 只限 CLI 和 Desktop app）
- 本機路徑不可用，`/schedule` 雲端任務也只能存取 GitHub repo

---

## Claude in Chrome（瀏覽器整合）

> **狀態：** Beta  
> **支援瀏覽器：** Google Chrome、Microsoft Edge（不支援 Brave、Arc、其他 Chromium；不支援 WSL）  
> **需求：** Claude Code v2.0.73+、[Claude in Chrome 擴充套件](https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn) v1.0.36+、Pro / Max / Team / Enterprise 方案

Claude Code 透過 **Claude in Chrome 瀏覽器擴充套件**獲得瀏覽器自動化能力。Claude 在可見的 Chrome 視窗中即時執行操作，共享你的登入狀態，能存取所有已登入的網站。

### 啟用方式

```bash
# 單次啟用（當次 session）
claude --chrome

# 在現有 session 中啟用 / 管理連線
/chrome

# 設為預設（每次啟動自動連線）
/chrome → 選擇 "Enabled by default"
```

> 設為預設會在每次 session 載入瀏覽器工具，略微增加 context 消耗。若只偶爾需要，建議按需用 `--chrome`。

### 可執行的操作

| 功能 | 說明 |
|------|------|
| **Live 偵錯** | 直接讀取 console 錯誤與 DOM 狀態，對應修復程式碼 |
| **設計驗證** | 從 Figma mock 建完 UI 後，開瀏覽器確認是否一致 |
| **Web App 測試** | 測試表單驗證、視覺 regression、使用者流程 |
| **已登入網站存取** | 操作 Google Docs、Gmail、Notion 等，不需要 API connector |
| **資料擷取** | 從網頁抓取結構化資料，儲存成 CSV |
| **表單填寫自動化** | 讀取本地 CSV，逐行填入 CRM 或後台表單 |
| **跨網站工作流** | 協調多個分頁完成跨平台任務 |
| **Session 錄製** | 將瀏覽器操作錄製成 GIF，用於文件或分享 |

### 典型使用範例

```
# 測試本地 web app
I just updated the login form validation. Open localhost:3000,
try submitting the form with invalid data, check the error messages.

# Debug console 錯誤
Open the dashboard page and check the console for any errors on load.

# 批次填表
I have contacts.csv. For each row, go to crm.example.com → Add Contact
and fill in name, email, phone.

# 在 Google Docs 寫內容（不需要 API）
Draft a project update based on recent commits and add it to my
Google Doc at docs.google.com/document/d/abc123.

# 錄製 demo GIF
Record a GIF of the checkout flow from add-to-cart to confirmation.
```

### 與 Computer Use 的關係

| | Claude in Chrome | Computer Use（CLI） |
|---|---|---|
| **操作對象** | 瀏覽器（Chrome / Edge） | 所有原生 App |
| **平台** | 跨平台 | macOS 限定 |
| **方式** | Chrome 擴充套件 | MCP server |
| **優先順序** | 優先（Claude 先嘗試用 Chrome） | 最後手段 |

> Claude 的工具呼叫優先順序：MCP server → Bash → **Claude in Chrome** → Computer Use

### 連線問題排查

| 問題 | 解法 |
|------|------|
| 擴充套件未偵測到 | 確認擴充套件已啟用 → 重啟 Chrome → `/chrome` 選 Reconnect |
| 首次啟用後無反應 | Native messaging host 需要 Chrome 重啟才能生效 |
| 長時間後斷線 | Extension service worker 閒置，執行 `/chrome` → Reconnect |
| JS 對話框卡住 | 手動關掉 alert/confirm 再告訴 Claude 繼續 |

---

## VS Code 整合

> **需求：** VS Code 1.98.0+  
> **同樣適用：** Cursor（使用相同擴充套件）

Claude Code 提供原生 VS Code 擴充套件，在 IDE 內提供圖形化介面，並與 CLI 共用對話歷史與設定。

### 安裝

```
VS Code Extensions（Ctrl+Shift+X）→ 搜尋 "Claude Code" → Install
```

或直接在瀏覽器點擊 `vscode:extension/anthropic.claude-code`。

### 開啟方式

| 位置 | 方式 |
|------|------|
| **Editor Toolbar**（最快）| 右上角 Spark 圖示（需有開啟的檔案） |
| **Activity Bar** | 左側欄 Spark 圖示 → 顯示 session 清單 |
| **Status Bar** | 右下角 `✱ Claude Code`（無需開啟檔案） |
| **Command Palette** | `Ctrl+Shift+P` → 輸入 "Claude Code" |

### VS Code 專屬功能

以下功能只有 VS Code 擴充套件有，CLI 沒有：

**Inline Diff 審查**

Claude 要修改檔案時，直接在編輯器顯示左右對比，詢問允許、拒絕或指示修改——不需要切出去 review。

**Plan 以 Markdown 開啟**

Plan Mode 下，Claude 產生的計畫會直接開成完整 Markdown 文件，可以直接在文件中加入 inline comment 回饋，然後再讓 Claude 執行。

**@-mention 帶行號**

```
@auth.ts#5-10    ← 指定檔案的第 5 到 10 行
@src/api/        ← 整個資料夾（加尾斜線）
```

選取程式碼後按 `Option+K`（Mac）/ `Alt+K`（Windows）自動插入帶行號的 @-mention。

**Checkpoint（版本回溯）**

滑鼠移到任一訊息可顯示 Rewind 按鈕，提供三個選項：

| 選項 | 說明 |
|------|------|
| Fork conversation from here | 從此訊息分支出新對話，保留所有程式碼變更 |
| Rewind code to here | 還原程式碼到此時間點，保留完整對話歷史 |
| Fork conversation and rewind code | 分支新對話 ＋ 還原程式碼 |

**多 Tab / 多視窗並行**

`Ctrl+Shift+Esc` 開新 Tab；Command Palette → "Open in New Window" 開新視窗。各自維持獨立的對話歷史，可同時進行不同任務。

**Resume 雲端 session**

Past Conversations → **Remote** 分頁，可以直接把 claude.ai/code 的雲端 session 拉回 VS Code 繼續（等同 `/teleport`）。

**@browser 語法**

```
@browser go to localhost:3000 and check the console for errors
```

直接在 prompt 中呼叫 Chrome 整合，不需要先執行 `/chrome`。

### VS Code 設定

| 設定 | 預設 | 說明 |
|------|------|------|
| `selectedModel` | default | 預設模型 |
| `initialPermissionMode` | default | 預設 permission mode |
| `useTerminal` | false | 切換成 CLI 樣式介面 |
| `preferredLocation` | panel | `sidebar`（右側欄）或 `panel`（新 Tab） |
| `autosave` | true | Claude 讀寫前自動存檔 |
| `useCtrlEnterToSend` | false | 用 Ctrl+Enter 送出（而非 Enter） |

> 在 `settings.json` 加入 `"$schema": "https://json.schemastore.org/claude-code-settings.json"` 可獲得設定的自動補全與驗證。

### CLI 限定功能

有些功能只有在 terminal 執行 `claude` 才有，VS Code 擴充套件不支援：

| 功能 | CLI | VS Code 擴充套件 |
|------|-----|----------------|
| `!` bash 快速執行 | ✓ | ✗ |
| Tab 補全 | ✓ | ✗ |
| 所有 slash commands | ✓ | 部分（輸入 `/` 查看可用清單） |

> 在 VS Code 整合終端機（`` Ctrl+` ``）執行 `claude` 即可使用 CLI，並自動連接 VS Code 的 diff 檢視器。

### CLI 在 VS Code 內運作的行為

這是很常見的使用方式：VS Code 開著，用整合終端機（或外部 terminal + `/ide`）操作 CLI。此時 CLI 偵測到 VS Code 存在，會透過內建 IDE MCP server 整合，產生以下行為：

```
Claude 準備修改檔案
  ↓
Claude Code 自動判斷變更大小
  ├─ 小幅修改 → 只在 Claude 面板顯示 inline snippet
  └─ 大幅修改 → 開 VS Code 原生 Diff 編輯器（左：修改前，右：修改後）
  ↓
CLI 在終端機問：「Allow this edit? (y/n)」
  ↓
你在終端機按 y/n 決定
```

**Diff 視窗出現的條件：Claude Code 自動判斷，無法手動控制**

不管是 Extension 還是 CLI，都遵循同一套邏輯：

| 情況 | Extension | CLI（連接 VS Code） |
|------|----------|-------------------|
| **小幅修改**（幾行、局部改動） | Claude 面板內嵌 snippet（有語法高亮） | 終端機文字輸出 |
| **大幅修改**（多行、整段重寫） | VS Code Diff 編輯器 | VS Code Diff 編輯器 |

Extension 和 CLI 的差別只是小改動的預覽形式——Extension 的 inline snippet 比 CLI 的終端機文字好看，但兩者都不是「總是開 Diff 編輯器」。

> 目前沒有設定可以強制所有改動都開 Diff 編輯器。有人開 issue 要求加 `claudeCode.diffDisplay` 設定，Anthropic 於 2026/01 標記為 Not Planned 關閉。  
> Diff 視窗是唯讀預覽，實際的允許／拒絕還是在終端機的 y/n（CLI）或 Claude 面板的按鈕（Extension）。

**外部 terminal 也能享有這個整合：**

在 VS Code 之外的終端機（iTerm2、Windows Terminal 等）執行 `claude` 時，在 session 中輸入 `/ide` 即可手動連接到目前開著的 VS Code，之後修改也會觸發 Diff 視窗。

### 共用歷史：extension ↔ CLI 互通

兩者共享同一份對話歷史。要把 extension 的對話繼續到 CLI：

```bash
claude --resume    # 互動式選擇對話
```

外部 terminal 連回 VS Code：

```bash
/ide    # 在 CLI session 中執行，連接到 VS Code
```

### 內建 IDE MCP Server

擴充套件啟動時會在本機起一個 MCP server（名稱 `ide`），CLI 自動連接，用於開啟 diff 檢視器、讀取選取範圍等內部 RPC。這個 server 不會出現在 `/mcp` 清單，因為不需要設定。

Claude 可見的工具只有兩個：

| 工具 | 說明 |
|------|------|
| `mcp__ide__getDiagnostics` | 取得 VS Code Problems 面板的 language server 錯誤與警告 |
| `mcp__ide__executeCode` | 在 Jupyter notebook 執行 Python（**每次都會跳確認視窗**，無法靜默執行） |

---

## Computer Use（CLI）

> **平台：** macOS 限定（Windows 請用 Desktop app 的 Cowork 設定）  
> **需求：** Pro / Max 方案，Claude Code v2.1.85+，互動式 session

CLI 的 Computer Use 走 MCP server，與 Cowork Desktop 的開關**完全獨立**。

**適合的開發用途：**
- Build native app 後自動啟動、點擊測試
- 對 Electron / iOS Simulator 做 E2E UI 測試，不需要 Playwright 設定
- 重現視覺 bug（調整視窗大小、截圖、修 CSS）
- 操控沒有 CLI / API 的 GUI 工具

**啟用方式：**

```
/mcp
→ 找到 computer-use → 選 Enable
（設定持久化，同一個專案只需設定一次）
```

首次使用時 macOS 會要求兩個權限：
- **Accessibility**：讓 Claude 點擊、輸入、捲動
- **Screen Recording**：讓 Claude 看到你的螢幕

**使用優先順序：**

Claude 會優先用精準工具，Computer Use 是最後手段：

```
MCP server → Bash → Claude in Chrome → Computer Use
```

**安全機制：**
- 每個 App 需單獨批准（每 session 有效）
- Terminal / IDE 等高權限 App 會額外警告
- 按 `Esc` 可隨時中止
- 同一時間只有一個 session 可以控制螢幕

---

## MCP（Model Context Protocol）

MCP 是讓 Claude Code 連接外部工具與服務的標準協定。安裝 MCP server 後，Claude 在對話中可以直接操作這些服務——就像多了一組工具。

### 安裝方式

**方法一：Plugin（推薦，自動設定）**

```bash
claude plugin install figma@claude-plugins-official
```

Anthropic 維護的 official plugin marketplace 會自動完成 MCP 安裝、認證設定與 Skills 注入。

**方法二：手動新增 Remote MCP（HTTP）**

```bash
# 只對當前 session 有效
claude mcp add --transport http figma https://mcp.figma.com/mcp

# 全域（所有專案都能用）
claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp
```

**方法三：手動新增 Local MCP（stdio）**

```bash
# 例如本地執行的 MCP server
claude mcp add my-server -- npx -y my-mcp-server
```

**管理 MCP：**

```bash
claude mcp list              # 列出已安裝的 MCP servers
claude mcp remove <name>     # 移除
/mcp                         # 在 session 中管理（啟用、停用、查看狀態）
```

### 認證

Remote MCP 通常走 OAuth 流程：

```
/mcp → 選擇 server → Authenticate → 瀏覽器跳出授權頁面 → Allow Access
→ Authentication successful
```

---

### 實例：Figma MCP

> **官方文件：** [developers.figma.com/docs/figma-mcp-server](https://developers.figma.com/docs/figma-mcp-server/remote-server-installation/)

Figma 官方提供的 MCP server，讓 Claude Code 能直接讀取設計資料、生成對應程式碼，並把程式碼生成的 UI 推回 Figma 畫布。

#### 安裝

```bash
# Plugin 安裝（最簡單）
claude plugin install figma@claude-plugins-official

# 或手動加入 remote server（全域）
claude mcp add --scope user --transport http figma https://mcp.figma.com/mcp
```

安裝後在 session 中認證：

```
/mcp → figma → Authenticate → Allow Access
```

#### 兩種 MCP Server 模式

| | Remote MCP（推薦） | Desktop MCP（本地） |
|---|---|---|
| **連線位置** | `https://mcp.figma.com/mcp` | `http://127.0.0.1:3845/sse` |
| **需要 Figma Desktop** | 不需要 | 需要（啟用 Dev Mode MCP） |
| **方案需求** | 所有方案（Free 有 rate limit） | Dev 或 Full seat（付費方案） |
| **功能完整度** | 較完整 | 略少 |

Desktop 模式啟用：Figma 選單 → Preferences → **Enable Dev Mode MCP Server**

#### 可執行的操作

| 功能 | 說明 |
|------|------|
| **讀取設計資料** | 取得 frame、component 的版型、顏色、間距等設計 token |
| **生成程式碼** | 從選取的 frame 直接輸出對應的 React / HTML / CSS |
| **Code Connect** | 把 Figma 元件對應到 codebase 的實際元件，生成時使用真正的元件而非新寫 |
| **Write to Canvas** ★ | 在 Figma 中建立 frame、元件、變數、auto layout（Beta，Free 期間免費） |
| **UI → Figma** | 把瀏覽器中渲染的 UI 擷取為可編輯的 Figma 圖層 |
| **FigJam 存取** | 讀取 FigJam 圖表內容 |

#### Write to Canvas（Beta）

> **狀態：** Beta 開放中，測試期間免費；正式上線後轉為付費功能  
> **需求：** Full seat（Dev seat 僅限唯讀，Draft 除外）

AI agent 可直接在 Figma 檔案中建立原生設計結構，而非輸出靜態圖片：

- 建立新頁面與 frame
- 設計有 auto layout 的畫面
- 把數值轉為 variable 並更新元件
- 根據現有 design system 建立空白狀態
- 在同一個檔案中多次迭代編輯

**目前限制：**
- 每次 tool call 回應上限 **20KB**
- 不支援自訂字體與外部資產
- 輸出品質屬 Beta 等級，需人工 review 後再用

#### 使用方式

**方法一：貼 Figma 連結**

```
把 Figma frame 連結複製後貼入 prompt：
「Convert this sign-up card design to React: https://figma.com/design/abc123/...?node-id=1-23」
```

**方法二：選取後問（搭配 Figma Desktop）**

```
在 Figma 選取 frame 後，在 Claude Code 說：
「Implement the selected Figma frame as a React component.
 Use my current selection in Figma to get the designs.」
```

**方法三：自然語言描述**

```
「Look at the Button component in our design system (Figma file: xxx)
 and generate the matching TypeScript component.」
```

#### Code Connect 設定

Code Connect 讓 Figma 知道設計元件對應到 codebase 的哪個元件，MCP 生成程式碼時就會使用現有元件而不是重新實作：

```bash
# 在 Figma MCP plugin 的 Skill 中執行
/figma:code-connect
```

或在 prompt 中說：「Set up Code Connect for the Button component in this Figma file.」

設定完成後，Claude Code 生成的程式碼會直接 import 你 codebase 的 `<Button>` 而不是從頭寫一個。

#### Rate Limit

| Figma 方案 | 限制 |
|-----------|------|
| Starter 或 View / Collab seat | 每月最多 **6 次** tool call |
| Professional / Organization / Enterprise（Dev 或 Full seat） | 依 Figma REST API Tier 1 rate limit |

#### 雙向工作流（Design ↔ Code）

```
Figma 設計
  → MCP 讀取設計資料（含 Code Connect 對應）
  → Claude Code 生成符合 codebase 規範的元件
  → 在瀏覽器預覽實作結果
  → Claude in Chrome 擷取渲染後的 UI
  → 推回 Figma 畫布為可編輯圖層（Write to Canvas）
```

---

## Statusline（狀態列）

終端底部的即時狀態列，每次 Claude 回應後更新，顯示 session 關鍵資訊。

### 運作原理

Claude Code 每次更新時，把 JSON 格式的 session 資料透過 stdin 傳給你設定的腳本，腳本輸出的內容就會顯示在底部。

JSON 包含的資料：
- 模型名稱、context 使用率、費用
- 工作目錄、worktree 資訊
- 速率限制（使用率 + 重置時間）

### 設定方式

**方法一：用內建 subagent 自動設定**

```
/statusline
```

`statusline-setup` subagent 會幫你生成腳本並寫入 `settings.json`。

**方法二：手動設定**

在 `~/.claude/settings.json` 加入：

```json
{
  "statusLine": {
    "type": "command",
    "command": "/path/to/your/statusline-script"
  }
}
```

### 社群工具

自己寫腳本解析 JSON 可以完全客製化，但也有現成的社群工具：

| 工具 | 特色 |
|------|------|
| [claude-code-statusline](https://github.com/harry18456/claude-code-statusline) | Go 寫的獨立執行檔，顯示模型、context 進度條、費用、時間、Git 分支、速率限制、子 agent 狀態，零值自動隱藏 |
| [ccstatusline](https://github.com/sirmalloc/ccstatusline) | Powerline 風格，支援主題切換 |
| [ccusage](https://ccusage.com/guide/statusline) | 整合用量分析 |

---

## No Flicker Mode（全螢幕渲染）

Claude Code v2.1.89 推出的新渲染模式，讓介面更像 vim / htop。

### 核心改變

- 輸入框**永遠固定在畫面底部**（不會隨輸出捲動）
- 使用 terminal 的 alternate screen buffer（類似 vim 開啟的方式）
- 長對話不再佔用越來越多記憶體
- **支援滑鼠操作**

### 啟用方式

**macOS / Linux / Git Bash：**
```bash
# 單次使用
CLAUDE_CODE_NO_FLICKER=1 claude

# 永久啟用（加入 ~/.zshrc 或 ~/.bashrc）
export CLAUDE_CODE_NO_FLICKER=1

# 關閉
unset CLAUDE_CODE_NO_FLICKER
```

**Windows PowerShell：**
```powershell
# 單次使用
$env:CLAUDE_CODE_NO_FLICKER=1; claude

# 永久啟用（重開終端機後生效）
[System.Environment]::SetEnvironmentVariable("CLAUDE_CODE_NO_FLICKER", "1", "User")

# 關閉
$env:CLAUDE_CODE_NO_FLICKER=0
```

**需要 Claude Code v2.1.89 以上**

### 滑鼠支援功能

| 操作 | 效果 |
|------|------|
| 點擊輸入框 | 移動游標到點擊位置 |
| 點擊折疊的工具結果 | 展開/收合 |
| 點擊 URL / 檔案路徑 | 在瀏覽器/編輯器開啟 |
| 點擊並拖曳 | 選取文字（自動複製到剪貼簿） |
| 滾輪 | 捲動對話 |

### 停用滑鼠（保留渲染改進）

如果你在 SSH 或 tmux 環境，滑鼠捕捉可能干擾原生選取：

```bash
CLAUDE_CODE_NO_FLICKER=1 CLAUDE_CODE_DISABLE_MOUSE=1 claude
```

### Transcript Mode（對話搜尋）

按 `Ctrl+o` 進入 Transcript Mode，支援 vim 風格的搜尋：

| 按鍵 | 功能 |
|------|------|
| `/` | 開始搜尋 |
| `n` / `N` | 下一個 / 上一個結果 |
| `j` / `k` | 上下移動 |
| `g` / `G` | 跳到最上 / 最下 |
| `[` | 輸出到 terminal scrollback（讓 Cmd+F 可用） |
| `Esc` / `q` | 退出 Transcript Mode |

### 捲動速度調整

```bash
# 預設捲動偏慢，設定倍率（1-20，建議 3）
export CLAUDE_CODE_SCROLL_SPEED=3
```

### tmux 使用者注意

在 tmux 內使用 No Flicker Mode 需要啟用 mouse mode：

```bash
# ~/.tmux.conf
set -g mouse on
```

**不相容：** iTerm2 的 `tmux -CC` 整合模式（請改用一般 tmux）

---

## /buddy 指令

v2.1.89 新增的特殊功能：你的 Claude Code 虛擬寵物。

```bash
/buddy
```

### 功能特點

- 生成一個專屬於你的 ASCII art 虛擬寵物
- **確定性生成**：根據你的 user ID hash 計算，每次結果相同
- 18 種寵物種族（蘑菇、鴨子、龍、水豚等）
- 5 個屬性：`DEBUGGING`、`PATIENCE`、`CHAOS`、`WISDOM`、`SNARK`
- 稀有度等級：Common → Uncommon → Rare → Epic → Legendary
- 可客製化外觀（帽子、眼睛樣式）
- 極低機率出現「shiny」版本

### 原理說明

寵物不是隨機的，而是由以下流程確定性生成：

```
你的 user ID
    → SHA hash（加入 salt）
    → 作為 PRNG seed
    → 依固定順序消耗亂數
    → 決定種族、屬性、外觀
```

這代表每個帳號都有唯一固定的 buddy，換電腦不會變。

---

## npm 外洩事件始末（2026/03）

### 外洩事件（2026/03/30~31）

**起因：** Anthropic 發布 v2.1.88 時，`.npmignore` 少了一條規則，導致 `cli.js.map`（59.8 MB source map）一起打包進 npm 套件。source map 會把壓縮程式碼對應回原始原始碼，等於把完整的 TypeScript 原始碼（1,884 個檔案、512,000 行）送到所有安裝者手中。

**同期事件：** 2026/03/31 00:21–03:29 UTC，npm 上的 Axios 套件遭北韓駭客植入惡意程式碼（[TechCrunch](https://techcrunch.com/2026/03/31/hacker-hijacks-axios-open-source-project-used-by-millions-to-push-malware/)），與 Claude Code 外洩為獨立事件，但時間完全重疊。

---

### 原始碼中發現的未公開功能

| 功能 | 說明 |
|------|------|
| **KAIROS** | 自主背景 daemon 模式，Claude 在閒置時整理記憶、移除矛盾，不需等待人類指令 |
| **ULTRAPLAN** | 把規劃任務卸載到遠端雲端 session（最多 30 分鐘推理），結果再回傳本地 |
| **Coordinator Mode** | 一個 Claude 實例可以 spawn 並管理多個 worker agent 並行執行 |
| **三層記憶架構** | 解決長 session 的「context entropy」問題，非傳統的「全部存起來」策略 |
| **Anti-Distillation** | `ANTI_DISTILLATION_CC` flag 在 API 請求中注入假工具定義，毒化競爭對手錄製 API 流量所得的訓練資料 |
| **BUDDY** | 虛擬寵物（/buddy 指令），18 種種族、稀有度系統、依 user ID hash 確定性生成 |

競爭對手（Cursor、Copilot、Windsurf）因此取得了 Anthropic 花數十億打造的功能藍圖。程式碼在數小時內散布到去中心化鏡像，事實上無法控制。

---

### Anthropic 的應對與後果

**DMCA 大規模下架（又撤回）：**
Anthropic 向 GitHub 提出 8,000+ 個 DMCA 下架通知，但意外把 Anthropic 自己公開 repo 的合法 fork 也一起下架。工程師 Boris Cherny 承認這是誤操作，Anthropic 隨後撤回大部分通知，最終只保留少數幾個 repo 的申訴。

**社群的繞版權回應：**
[claw-code](https://github.com/ultraworkers/claw-code) 在外洩當天早上由開發者從頭用 Rust + Python 重寫，清室實作（cleanroom implementation）Claude Code 的架構模式，不複製任何原始碼，因此不受版權約束。

---

### 事件時間軸

```
2026/01~02  npm 安裝正式宣布 deprecated
2026/03/30  v2.1.88 意外帶 source map 發布到 npm
2026/03/31  社群發現洩漏，程式碼瞬間散布全球
2026/03/31  Axios 供應鏈攻擊（獨立事件，同日）
2026/03/31  Anthropic 發出 8,000+ DMCA 通知
2026/04/01  Anthropic 撤回大部分 DMCA（誤傷合法 repo）
2026/04/01  claw-code 清室重寫版發布
```

---

## 鍵盤快捷鍵

### 多行輸入

| 方法 | 快捷鍵 | 適用情境 |
|------|--------|---------|
| 通用 | `\` + `Enter` | 所有終端機皆可用 |
| macOS 預設 | `Option+Enter` | macOS 內建終端機 |
| 免設定 | `Shift+Enter` | iTerm2、WezTerm、Ghostty、Kitty |
| 其他終端機 | 執行 `/terminal-setup` | VS Code、Alacritty、Warp 等需要額外安裝 |

### 一般控制

| 快捷鍵 | 功能 | 說明 |
|--------|------|------|
| `Ctrl+C` | 取消輸入 / 中止生成 | 有輸入時清除輸入框；生成中時中止回應 |
| `Ctrl+D` | 離開 Claude Code | EOF 訊號 |
| `Ctrl+G` 或 `Ctrl+X Ctrl+E` | 在外部編輯器開啟 | 用於編輯較長的 prompt |
| `Ctrl+R` | 反向搜尋指令歷史 | 互動式搜尋先前輸入過的指令 |
| `Ctrl+L` | 重繪畫面 | 不清除對話，只重繪 UI |
| `Ctrl+O` | 切換詳細輸出 | 展開工具呼叫細節 |
| `Ctrl+B` | 背景執行目前 bash 指令 | tmux 使用者需按兩次（因 tmux 占用 Ctrl+B） |
| `Ctrl+T` | 切換 task list 顯示 | 顯示 / 隱藏多步驟任務清單 |
| `Shift+Tab` | 循環切換 Permission Mode | default → acceptEdits → plan → bypassPermissions |
| `Esc` + `Esc` | Rewind | 開啟可捲動的 checkpoint 選單，可分別選擇還原**程式碼**、**對話**、或**兩者** |

### 文字編輯

| 快捷鍵 | 功能 | 說明 |
|--------|------|------|
| `Ctrl+K` | 刪除到行尾 | 刪除游標右側到行尾，文字暫存於緩衝區 |
| `Ctrl+U` | 刪除到行首 | 刪除游標左側到行首，文字暫存於緩衝區 |
| `Ctrl+Y` | 貼上剛刪除的文字 | 貼回 `Ctrl+K` 或 `Ctrl+U` 刪掉的內容 |
| `Alt+B` | 游標往前移一個單字 | 需設定 Option 為 Meta（macOS） |
| `Alt+F` | 游標往後移一個單字 | 需設定 Option 為 Meta（macOS） |

### 模型與功能

| 快捷鍵 | 功能 |
|--------|------|
| `Alt+T` / `Option+T` | 切換 Extended Thinking |
| `Alt+P` / `Option+P` | 開啟模型選擇器 |
| `Alt+O` / `Option+O` | 切換 Fast Mode |

### 貼上圖片

| 快捷鍵 | 功能 |
|--------|------|
| `Ctrl+V` / `Cmd+V`（iTerm2）/ `Alt+V`（Windows） | 從剪貼簿貼上圖片 |

---

## tmux 說明

tmux（terminal multiplexer）是終端機多工工具，讓你在一個視窗內開多個終端機 session，並讓 session 在背景持續執行，即使關掉終端機也不會中斷。

**對 Claude Code 的影響：**

| 情境 | 說明 |
|------|------|
| 背景執行（`Ctrl+B`） | tmux 的 prefix key 也是 `Ctrl+B`，所以在 tmux 內使用時要**按兩次** |
| No Flicker Mode | 需在 `~/.tmux.conf` 加入 `set -g mouse on` 才能讓滑鼠操作正常 |
| iTerm2 整合 | iTerm2 的 `tmux -CC` 原生整合模式與 No Flicker Mode 不相容，請改用一般 tmux |

**基本 tmux 指令（不熟的話可以先記這幾個）：**

```bash
tmux new -s mywork      # 建立名為 mywork 的 session
tmux attach -t mywork   # 重新連接到 mywork session
tmux ls                 # 列出所有 session
Ctrl+B d                # 中斷連接（session 繼續在背景跑）
Ctrl+B %                # 左右分割視窗
Ctrl+B "                # 上下分割視窗
```

---

## 第三方工具政策異動（2026）

### 訂閱額度限制第三方 Harness（2026 年 4 月 4 日起生效）

Anthropic 於 **2026 年 4 月 4 日 12pm PT / 8pm BST** 正式執行：**訂閱額度（Free / Pro / Max）不再涵蓋第三方工具**，第一波從 OpenClaw 開始，後續將陸續擴展至所有第三方 harness。

**官方 Email 原文重點：**
> "Starting April 4 at 12pm PT / 8pm BST, you'll no longer be able to use your Claude subscription limits for third-party harnesses including OpenClaw."
>
> "They will require extra usage, a pay-as-you-go option billed separately from your subscription."
>
> "Your subscription still covers all Claude products, including Claude Code and Claude Cowork."

**實際影響：**
| 使用情境 | 變化 |
|---------|------|
| Claude.ai 網頁、Claude Desktop、Claude Code | 不受影響，訂閱額度照常 |
| OpenClaw、OpenCode 等第三方工具 | 需開啟 Extra Usage（按 API 費率另計） |

**Anthropic 的補償：**
- 提供一次性免費額度，金額等同當月訂閱費（詳見帳號 Email）

**背景脈絡（分階段推進）：**
- **2026 年 1 月**：封鎖偽造 Claude Code 身份的工具（主要打 OpenCode）
- **2026 年 2 月**：正式寫入 ToS，The Register 等媒體報導
- **2026 年 4 月 4 日**：OpenClaw 正式納入執行，全面擴展至所有 harness

根本原因：第三方工具去除了 Claude Code 內建的速率限制，Agent 任務可無限量耗用 token，訂閱制在此使用情境下對 Anthropic 大幅虧損。

> 相關討論：[Hacker News 討論串](https://news.ycombinator.com/item?id=47069299)、[The Register 報導](https://www.theregister.com/2026/02/20/anthropic_clarifies_ban_third_party_claude_access/)

---

### Claude Code Channels：官方內建的 OpenClaw 替代方案（2026 年 3 月）

Anthropic 於 2026 年 3 月 20 日推出 **Claude Code Channels**，直接透過訊息 App 遠端操控本機的 Claude Code session，不需要任何第三方工具。

**運作方式：**
```
手機 Telegram/Discord/iMessage
        ↓  傳送指令
   MCP Server（本機，channel plugin）
        ↓  轉發給 Claude Code
   Claude Code（本機執行，存取本機檔案/git/工具）
        ↓  完成後回傳結果
手機 Telegram/Discord/iMessage
```

#### 前置需求

- Claude Code **v2.1.80** 以上
- 需用 **claude.ai 帳號登入**（不支援 API key / Console 登入）
- 需安裝 **Bun**（非 Node.js）

```bash
bun --version  # 確認已安裝
```

#### Telegram 設定

1. 開啟 Telegram，找 **@BotFather**，輸入 `/newbot`
2. 給 bot 取名，取得 token
3. 執行：
   ```bash
   claude --channels plugin:telegram@claude-plugins-official
   ```
4. 私訊你的 bot → 它會回傳一組 **6 碼配對碼**
5. 在 Claude Code 輸入配對碼完成綁定
6. 之後直接私訊 bot 即可下指令

#### Discord 設定

1. 到 [Discord Developer Portal](https://discord.com/developers) 建立新 application
2. 新增 bot user，複製 token
3. Claude Code 會產生邀請連結，把 bot 加入你的 server
4. 執行對應指令啟動 channel
5. 在 Discord 頻道或 DM 傳送指令

#### iMessage 設定（僅限 macOS）

- 傳訊息給**自己**的號碼，自動通過驗證
- 新增其他聯絡人：
  ```
  /imessage:access allow +886912345678
  ```

#### 測試用（不需外部服務）

官方提供 **Fakechat**，在 localhost 跑假聊天介面，適合先試玩：

```bash
claude --channels plugin:fakechat@claude-plugins-official
```

→ 開啟瀏覽器，直接輸入訊息測試，不需要 Telegram / Discord 帳號

#### 安全機制

每個 channel 都有 **sender allowlist**，只有明確授權的 user ID 才能發送指令，其他人的訊息會被靜默丟棄。

> 官方文件：[Channels Reference](https://code.claude.com/docs/en/channels)
