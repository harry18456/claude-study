# Verification Report: `07_SKILL.md`

## Header

| Field | Value |
|---|---|
| Source filename | `07_SKILL.md` |
| Source line count | 359 |
| External URL count in source | 0 |
| Verification date | 2026-04-22 |
| Verifier | Claude Opus 4.7 |

## Claims Table

| # | Line | Claim (excerpt) | Type | Result | Source / Suggestion |
|---|---|---|---|---|---|
| 1 | 29 | 全域 skills 路徑 `~/.claude/skills/` | product-spec | ✅ Verified | [Claude Code — Extend Claude with skills](https://code.claude.com/docs/en/skills) 明列 "Personal: `~/.claude/skills/<skill-name>/SKILL.md`" |
| 2 | 30 | 專案 skills 路徑 `.claude/skills/`（repo 根目錄） | product-spec | ✅ Verified | 同上官方 skills 文件 "Project: `.claude/skills/<skill-name>/SKILL.md`" |
| 3 | 31 | 插件 skills 路徑 `~/.claude/plugins/` | product-spec | ⚠️ Needs correction | 官方文件顯示 plugin skills 位於 `<plugin>/skills/<skill-name>/SKILL.md`，由插件根目錄帶入，路徑因安裝位置而異；`~/.claude/plugins/` 可能為**已安裝插件的快取位置**但不是 skill 官方路徑。建議改寫為「插件自帶 skill：位於插件自身目錄 `skills/`，透過插件系統掛載」。來源：[Claude Code skills docs — Share skills](https://code.claude.com/docs/en/skills) |
| 4 | 33 | 載入優先順序：**專案 skills > 全域 skills > 插件 skills** | product-spec | ⚠️ Needs correction | 官方明文為 **enterprise > personal > project**（enterprise 最高、project 最低），外加 plugin 以 `plugin-name:skill-name` namespace 獨立不會名稱衝突。文章把「專案 > 全域」說反了。建議改寫為「企業管理式 > 全域個人（`~/.claude/skills/`）> 專案（`.claude/skills/`）；插件透過 namespace 隔離不參與同名競爭」。來源：[Claude Code skills docs — Where skills live](https://code.claude.com/docs/en/skills) |
| 5 | 35 | Commands 與 Skills 兩系統現已合併；`commands/` 目錄仍可用但屬舊式寫法 | product-spec | ✅ Verified | 官方文件明示 "Custom commands have been merged into skills. A file at `.claude/commands/deploy.md` and a skill at `.claude/skills/deploy/SKILL.md` both create `/deploy` and work the same way. Your existing `.claude/commands/` files keep working." |
| 6 | 40–41 | Claude Code 預設附帶的內建 **skill** 包含 `/compact` 壓縮、`/help` 列表 | product-spec | ⚠️ Needs correction | 官方文件區分 **built-in commands**（`/help`、`/compact`）與 **bundled skills**（`/simplify`、`/batch`、`/debug`、`/loop`、`/claude-api`）。`/help` 與 `/compact` **不是 skill、而是 built-in command**。文章將兩者混為一談。建議改寫為：「內建指令：`/help`、`/compact`；內建 skill：`/simplify`、`/batch`、`/debug`、`/loop`、`/claude-api`」。來源：同上 skills docs 之 "Bundled skills" 段 |
| 7 | 65–67 | 三層載入機制：**Metadata**（永遠在 context，~100 字）／ **SKILL.md body**（skill 觸發時載入，500 行以內）／ **Bundled resources**（按需載入，無限制） | product-spec | ⚠️ Needs correction | 三層機制與官方描述**大致吻合**：description 永在 context、SKILL.md body 觸發時載入、supporting files 按需。但以下需修正：<br>（a）「Metadata ~100 字」不準確：官方說 description + when_to_use 合併**截短至 1,536 characters**、整體預算動態為 context window 的 1%（fallback 8,000 characters），而非 ~100 字。<br>（b）500 行建議**正確**（官方明示 "Keep SKILL.md under 500 lines"）。<br>建議更新 Metadata 字數上限為「description 與 when_to_use 合計 1,536 字元」。來源：Claude Code skills docs — Skill content lifecycle / Troubleshooting |
| 8 | 105 | 參數接收變數 `$ARGUMENTS` | product-spec | ✅ Verified | 官方 skills docs "Available string substitutions" 段列出 `$ARGUMENTS` 為官方變數；另有 `$ARGUMENTS[N]`、`$N`、`$name`、`${CLAUDE_SESSION_ID}`、`${CLAUDE_SKILL_DIR}` |
| 9 | 182–183 | `name`、`description` 為 frontmatter 必填欄位 | product-spec | ⚠️ Needs correction | 官方 frontmatter reference 明列 **`name: No`（Required 欄位為 No）**、**`description: Recommended`**，並說明「All fields are optional. Only `description` is recommended so Claude knows when to use the skill.」`name` 省略時會使用目錄名、`description` 省略時使用 body 第一段。建議修正「必填」欄位標記：`name` → 可選（推薦）、`description` → 推薦（非強制） |
| 10 | 184 | `compatibility` 欄位：相依需求說明 | product-spec | 🔍 Unverifiable | 官方 Claude Code skills frontmatter reference **未列** `compatibility` 欄位；`compatibility` 屬 Agent Skills 開放標準（agentskills.io）的公共欄位，Claude Code 未必解讀此欄位為相依需求。建議保留但加註「此欄位屬 Agent Skills 標準，Claude Code 目前無特殊解讀行為」 |
| 11 | 185 | `allowed-tools` 欄位：限制可用工具 | product-spec | ⚠️ Needs correction | 官方定義 **與文章描述方向相反**：`allowed-tools` 是「當 skill active 時**免除這些工具的使用權詢問**」，**而非限制**可用工具。官方原文 "grants permission for the listed tools while the skill is active... It does not restrict which tools are available"。文章描述為「限制可用工具」會誤導。建議改寫為「允許 skill active 時免詢問使用的工具清單」 |
| 12 | 186 | `context` 欄位：`fork` = 在獨立子 agent 執行 | product-spec | ✅ Verified | 官方 "Set to `fork` to run in a forked subagent context." |
| 13 | 187 | `agent` 欄位：`true` = 以 agent 模式運行 | product-spec | ⚠️ Needs correction | 官方：**`agent` 欄位值為 subagent type 名稱**（例如 `Explore`、`Plan`、`general-purpose` 或自訂 agent），與 `context: fork` 搭配指定執行環境。**不是 boolean `true`**。建議改寫為「指定在 forked context 下使用哪種 subagent 類型」 |
| 14 | 188 | `disable-model-invocation` 欄位：`true` = 只有使用者能手動呼叫 | product-spec | ✅ Verified | 官方敘述一致 |
| 15 | 189 | `user-invocable` 欄位：`false` = 只有 Claude 能自動觸發，不出現在 `/help` | product-spec | ✅ Verified | 官方：「Set to `false` to hide from the `/` menu. Use for background knowledge users shouldn't invoke directly.」（注意是從 `/` 選單而非 `/help` 本身） |
| 16 | 286–289 | `skill-creator` 來源為 `claude-plugins-official`（Anthropic 官方插件）；呼叫 `/skill-creator` | product-spec | ✅ Verified | [github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator) 與 [Skill Creator Claude Plugin 官方頁](https://claude.com/plugins/skill-creator) 確認 |
| 17 | 295–301 | skill-creator 流程：訪談 → 寫草稿 → 跑測試 → 評估迭代 → 優化 description → **打包輸出 `.skill` 檔案可分享安裝** | product-spec | ⚠️ Needs correction | 訪談、草稿、測試、迭代、描述優化皆可由官方 GitHub repo 與 Medium 實測文佐證。但 **「打包成 `.skill` 檔案」無官方來源**：Claude Code 目前 skill 分享機制為 (a) 直接提交 `.claude/skills/` 到 repo、(b) 以插件包（plugin marketplace）分發、(c) 透過 managed settings 企業部署；**沒有 `.skill` 檔案格式**。建議刪除 `.skill` 副檔名說法，改為「生成 skill 目錄與 SKILL.md 草稿，可透過插件系統分享」 |
| 18 | 348 | 插件安裝指令：`claude plugins install github:your-org/claude-skills-pack` | product-spec | ⚠️ Needs correction | 官方 CLI 語法為：(a) `/plugin install <name>@claude-plugins-official`（從官方 marketplace 安裝）、(b) `/plugin marketplace add <owner/repo>` 新增自訂 marketplace 後再 `/plugin install`。**沒有 `claude plugins install github:...` 這個頂層子命令**。建議改寫為：<br>```bash<br>/plugin marketplace add your-org/claude-skills-pack<br>/plugin install <skill-name>@your-org-skills-pack<br>```<br>來源：[Discover and install prebuilt plugins](https://code.claude.com/docs/en/discover-plugins) |

## Summary

### Counts

| Marker | Count |
|---|---|
| ✅ Verified | 8 |
| ⚠️ Needs correction | 9 |
| ❌ Unsupported | 0 |
| 🔍 Unverifiable | 1 |
| **Total** | 18 |

### Top 3–5 Critical Corrections

1. **Line 33 — 載入優先順序方向反了**（⚠️）：官方為 enterprise > personal > project，文章寫成「專案 > 全域 > 插件」屬反向。此為 skill 系統最基礎的行為模型，錯誤會導致讀者團隊設置時產生實際 bug。
2. **Line 185 — `allowed-tools` 意義反了**（⚠️）：官方是「免詢問使用權」**而非**「限制使用」。兩者語意相反，讀者照錯誤理解會把本應允許的 skill 寫成受限版本。
3. **Line 187 — `agent` 欄位值型別錯誤**（⚠️）：官方需填 subagent type 名稱，文章寫 `true` 會導致寫出的 skill 無法啟動。
4. **Line 40–41 — 內建指令與內建 skill 混用**（⚠️）：`/compact` / `/help` 是 built-in command、不是 skill；bundled skills 另有 `/simplify` 等五個。
5. **Line 348 — 插件安裝 CLI 錯誤**（⚠️）：`claude plugins install github:...` 不是實際語法，讀者照寫會失敗；應使用 `/plugin install` slash command 配 `/plugin marketplace add`。

### Overall Risk Assessment

**Risk level:** High

本文 0 條外部連結、卻描述了大量 frontmatter 欄位、載入機制、CLI 指令等**可直接被讀者照做**的技術細節。查證後發現：**18 條聲明中有 9 條需修正**（50%）。多數錯誤為「語意相反」或「欄位型別錯誤」，照本宣科會讓讀者寫出**無法運作的 skill**。建議本文作為下一輪 change 的最高優先修補對象，並於再版時**補充官方文件連結**至少 5 條以提高可追溯性。

### Detection Coverage Note

本報告檢測到 **18 條硬性聲明**。以下依 **Claim detection scope** 未列入：

- 純範例程式碼（frontmatter 樣板、範例 skill 內容）
- 主觀建議（例：「說明『為什麼』而非只寫『做什麼』」「保持單一職責」）
- 流程描述（例：「訪談 → 寫草稿」各步驟的內部操作）
- 示意圖（ASCII 對比框）

後續讀者（ChatGPT / Gemini）若發現本報告漏掉的聲明，屬預期行為 — 本驗證不保證 100% 覆蓋。
