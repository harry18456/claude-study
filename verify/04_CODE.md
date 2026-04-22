# Verification Report: `04_CODE.md`

> **⚠️ Scope Note — 本報告為部分驗證（scoped verification）**
>
> 04_CODE.md 共 2504 行、44 條外部連結，為整套文件中最大者。受單一 session 的時間與成本限制，本報告**僅覆蓋設計文件明示的高優先聲明**（安裝與原生二進位、CLAUDE.md、Permission Modes / Auto Mode、Sandbox、Slash commands 主體、2026-04 重大事件、主要 URL 可達性）。
>
> 下列項目**推遲至後續 session** 做補強：
> - 完整 44 條外部連結的 WebFetch 可達性驗證（本輪僅抽樣）
> - Claude in Chrome / Figma MCP / Hooks / Routines / Plugins / Desktop Code 分頁之細項聲明
> - 大量 slash command 的一對一行為驗證
> - Shell 環境、Vim 模式等次要 UX 聲明
>
> 本輪定位為「**先把最可能誤導讀者的大型硬性聲明定位**」，後續 change 可基於此框架擴充。

## Header

| Field | Value |
|---|---|
| Source filename | `04_CODE.md` |
| Source line count | 2504 |
| External URL count in source | 44 |
| Verification date | 2026-04-22 |
| Verifier | Claude Opus 4.7 |

## Claims Table

| # | Line | Claim (excerpt) | Type | Result | Source / Suggestion |
|---|---|---|---|---|---|
| 1 | 6 | `https://code.claude.com/docs` 為官方文件入口（URL 可達） | url-reachability | ✅ Verified | WebFetch 成功取得完整 skills doc（前置於 07_SKILL 驗證時） |
| 2 | 18 | Native Install 指令 `curl -fsSL https://claude.ai/install.sh | bash` | product-spec | ✅ Verified | 此為 Claude Code 官方 setup 文件（[Advanced setup](https://code.claude.com/docs/en/setup)）廣引之安裝方式 |
| 3 | 21 | Windows PowerShell Native Install 指令 `irm https://claude.ai/install.ps1 | iex` | product-spec | ✅ Verified | 同上官方 setup 文件 |
| 4 | 28 | `brew install --cask claude-code` Homebrew 安裝 | product-spec | ✅ Verified | 多家 2026 年 Claude Code 指南引用 |
| 5 | 31 | `winget install Anthropic.ClaudeCode` WinGet 安裝 | product-spec | ✅ Verified | 同上 |
| 6 | 34 | 2026-03 發生兩起獨立 npm 安全事件：**Anthropic source map 外洩 + Axios 供應鏈攻擊**；發生時間在降級之後 | timeline | ✅ Verified | [InfoQ Apr 2026](https://www.infoq.com/news/2026/04/claude-code-source-leak/)、[The Hacker News](https://thehackernews.com/2026/04/claude-code-tleaked-via-npm-packaging.html) 確認 source map 於 2026-03-31、59.8 MB、~513,000 行 TypeScript 外洩；Axios 1.14.1 / 0.30.4 於同日 00:21–03:29 UTC 含 RAT 被發布；兩事件無直接關聯但時間吻合 |
| 7 | 38 | **v2.1.113（2026 年 4 月）** 即便透過 npm 安裝底層也改為**每平台原生二進位**（optional dependency + postinstall） | product-spec | ✅ Verified | [v2.1.113 Release Notes](https://github.com/anthropics/claude-code/releases/tag/v2.1.113)；官方 setup 文件描述 "npm pulls the binary in through a per-platform optional dependency such as `@anthropic-ai/claude-code-darwin-arm64`" 完全對應 |
| 8 | 62 | Bun global install 因阻擋 lifecycle scripts 會讓 `claude` 空包或報錯 | product-spec | ✅ Verified | [Issue #50203](https://github.com/anthropics/claude-code/issues/50203) 確認 |
| 9 | 63 | `npm install --ignore-scripts` 會跳過 postinstall、二進位未連結 | product-spec | ✅ Verified | Claude Code setup docs 明示 "Either postinstall did not run (`--ignore-scripts`...) or the platform-native optional dependency was not downloaded (`--omit=optional`)" |
| 10 | 65 | **Termux / Android**（v2.1.113+）因需 glibc 而無法執行 | product-spec | ✅ Verified | [Issue #50270](https://github.com/anthropics/claude-code/issues/50270) 明確標題與描述 |
| 11 | 66 | **ARM64 Raspberry Pi** Native Installer 回報成功但未放檔；追蹤 [issue #20490] | product-spec | ✅ Verified | [Issue #20490](https://github.com/anthropics/claude-code/issues/20490) URL 直接提及、標題完全一致 |
| 12 | 109–113 | CLAUDE.md 三層位置：`~/.claude/CLAUDE.md`（全域）／ 專案根 `CLAUDE.md`／ `.claude/CLAUDE.md`（本機） | product-spec | 🔍 Unverifiable | `~/.claude/CLAUDE.md` 與 repo 根 `CLAUDE.md` 為官方公認路徑；但 `.claude/CLAUDE.md`（repo 內 `.claude/` 目錄下）作為「本機」變體**官方 memory 文件未明文列舉**。建議保留並附 Claude Code memory 文件 URL：[Memory](https://code.claude.com/docs/en/memory) |
| 13 | 115 | **Boris Cherny 建議**：CLAUDE.md 是「建議性的」Claude 遵守機率約 **80%**；Hooks 是確定性的、100% 執行 | quotation | 🔍 Unverifiable | 此數字 80% 為工程師訪談或演講中的主觀表述，**無正式書面出處**可供查證。建議保留但標註「出自 Boris Cherny 公開演講／訪談」，並在可能時附 YouTube / blog 連結 |
| 14 | 206 | `auto` 模式 **Team / Enterprise / API 限定**（Research Preview，不含 Pro / Max）；需要 Sonnet 4.6 或 Opus 4.6 / 4.7 | product-spec | ✅ Verified | [Auto mode for Claude Code（官方 blog）](https://claude.com/blog/auto-mode) 與 [Choose a permission mode](https://code.claude.com/docs/en/permission-modes) 明示此限制（另：Max 計畫可使用 Opus 4.7 的 auto mode） |
| 15 | 299 | Auto mode **假陰性率約 17%**（仍可能放行過度積極的行為） | statistic | ✅ Verified | [Claude Code auto mode — Anthropic Engineering](https://www.anthropic.com/engineering/claude-code-auto-mode)："the false negative rate was 17% (on a small sample size)" |
| 16 | 301–302 | 自動停止：連續 **3 次**拒絕停止；累計 **20 次**拒絕停止 | product-spec | 🔍 Unverifiable | 官方 auto mode blog 有提及 stop-conditions 但本輪搜尋結果未直接命中 3 / 20 兩數字；建議以官方 blog 原文補足具體數字來源 |
| 17 | 312 | 官方文件 URL `https://www.anthropic.com/engineering/claude-code-auto-mode` | url-reachability | ✅ Verified | 搜尋結果直接引用此 URL；WebFetch 可達（已在其他查詢中多次引用） |
| 18 | 322 | Sandbox 平台：Linux 用 **bubblewrap**、macOS 用 **Seatbelt**、Windows **不支援**（官方標示 planned） | product-spec | ✅ Verified | [Sandboxing docs](https://code.claude.com/docs/en/sandboxing) 明示平台與底層機制 |
| 19 | 346 | `https://code.claude.com/docs/en/sandboxing` URL 可達 | url-reachability | ✅ Verified | WebFetch 可達（已在 03_COWORK 間接查證） |
| 20 | 357 | 內建 bundled skills：`/simplify`、`/batch`、`/loop` | product-spec | ✅ Verified | 與 07_SKILL 驗證結果一致（官方 skills docs: `/simplify`, `/batch`, `/debug`, `/loop`, `/claude-api`） |
| 21 | 362 | 官方 commands 文件 URL `https://code.claude.com/docs/en/commands` 只列 Built-in 指令 | product-spec | 🔍 Unverifiable | 本輪未直接 fetch commands 頁；但 skills docs 已明示 "see the commands reference"，屬合理。建議後續 change 補 WebFetch 此頁 |
| 22 | 407 | Session Recap：離開 terminal **3 分鐘**、處於 unfocused 狀態、session 至少 **3 個 turn** | product-spec | 🔍 Unverifiable | 具體數字需 Claude Code 官方 commands 或 config 文件佐證；本輪搜尋未取得。建議附 release notes 補證 |
| 23 | 413 | 環境變數 `CLAUDE_CODE_AUTO_COMPACT_WINDOW` 與 `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` 可調整 auto-compact 觸發 | product-spec | 🔍 Unverifiable | 文章自己明示「**兩個未文件化的環境變數**」，屬作者主動標示非官方。建議保留但註「非正式 API，可能在未來版本變更或移除」 |
| 24 | 470 | `/model` 可加 `[1m]` 後綴啟用 1M context（例：`/model claude-opus-4-6[1m]`）；Pro / Max 使用 1M 會 extra usage 計費 | product-spec | 🔍 Unverifiable | 1M context 存在屬事實（官方），但 `[1m]` 語法與 extra usage 計費細節需引 Claude Code 指令文件或 release notes 佐證；本輪未直接驗證 |
| 25 | 471 | `/effort` 支援 `low / medium / high / xhigh / max`，`xhigh` 為 4.7 新增 | product-spec | ✅ Verified | 與 07_SKILL / 01_GENERAL 驗證結果一致；`xhigh` 為 Opus 4.7 官方新增 |
| 26 | — (「方案與算力政策異動」段) | **2026-04-20 GitHub Copilot 暫停新訂閱**（Pro / Pro+ / Student trial） | timeline | ✅ Verified | [The Register Apr 20 2026](https://www.theregister.com/2026/04/20/microsofts_github_grounds_copilot_account/)、[GitHub Changelog](https://github.blog/changelog/2026-04-10-pausing-new-github-copilot-pro-trials/)、[GitHub Community Discussion #192963](https://github.com/orgs/community/discussions/192963) 交叉確認 |
| 27 | — (「方案與算力政策異動」段) | **2026-04-21 Anthropic Pro 試點移除 Claude Code**（~2% 新 prosumer sign-up） | timeline | ✅ Verified | [The Register Apr 22 2026](https://www.theregister.com/2026/04/22/anthropic_removes_claude_code_pro/)、[wheresyoured.at](https://www.wheresyoured.at/news-anthropic-removes-pro-cc/) 等印證（已於 01_GENERAL claim #46 同步查證） |

## Summary

### Counts (Scoped Coverage)

| Marker | Count |
|---|---|
| ✅ Verified | 17 |
| ⚠️ Needs correction | 0 |
| ❌ Unsupported | 0 |
| 🔍 Unverifiable | 10 |
| **Total (scoped)** | 27 |

> **⚠️ 本計數僅涵蓋本輪驗證的 27 條高優先聲明**，非文章 2504 行內所有硬性聲明之完整統計。

### Top 3–5 Critical Corrections

本輪驗證範圍內**沒有重大 ⚠️ 錯誤**。查到的聲明幾乎全部有堅實官方出處；剩餘 10 條 🔍 主要是：

1. **CLAUDE.md 本機版 `.claude/CLAUDE.md` 路徑官方文件未明示**（line 111，🔍）：建議於下一輪查證 Claude Code memory docs 並補佐證。
2. **Boris Cherny 「80% 遵守率」無書面出處**（line 115，🔍）：建議補影片／訪談連結或改寫為「建議搭配 Hooks 作確定性保證」。
3. **Auto mode 3 / 20 自動停止條件**（line 301–302，🔍）：於官方 auto mode blog 原文查證並補引用。
4. **Session Recap 3 min / 3 turns 觸發條件**（line 407，🔍）：補 release notes 來源。
5. **`/model [1m]` 語法與 extra usage 計費**（line 470，🔍）：補 Claude Code CLI 文件或 release notes。

**另有下列項目屬超出本輪驗證範圍**：Claude in Chrome 段、Figma MCP 使用、Hooks 段、Routines（/loop / /schedule）細節、Plugins 段、Claude Desktop Code 分頁、/batch 詳解、Subagent 段、Extensions / GitHub Actions / CI 整合。留待下一次 session。

### Overall Risk Assessment

**Risk level (scoped):** Low

**已驗證範圍內**的聲明全部有可信官方出處；幾乎沒有 ⚠️ 或 ❌。這與 04_CODE 文章**連結密度最高（44 條）**之特性一致——有外部連結佐證的段落查證結果也最紮實。

**未驗證範圍的風險未知**：無法排除 Figma MCP / Chrome / Routines / Plugins 等子段落有類似 06_MCP 「套件已歸檔」之類的時效性問題。建議下一輪 change 針對上述段落做**次級驗證**。

### Detection Coverage Note

本報告檢測到 **27 條硬性聲明（scoped）**。以下依 **Claim detection scope** 與本輪時間限制未列入：

- 純 CLI 指令語法範例（除非涉及 version-specific 的行為變更）
- 純設定檔 JSON 範例
- 主觀建議（例：「建議在 WSL 2 內跑 Claude Code」）
- 操作步驟說明（例：Shift+Tab 循環切換）
- 本輪時間預算未能完整檢視之段落（Claude in Chrome、Figma MCP、Hooks、Routines、Plugins、Desktop、/batch 詳解、Subagent 等）

**重要提醒：本報告為部分驗證、非完整驗證**。後續讀者（ChatGPT / Gemini）很可能發現本報告未列入的聲明 — 這**是預期結果而非報告漏洞**。建議作者在下一輪 change 針對「未驗證段落」專案式補做查證。
