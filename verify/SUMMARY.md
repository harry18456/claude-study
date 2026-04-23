# 全局驗證摘要（Global Verification Summary）

- **驗證日期：** 2026-04-22
- **驗證者：** Claude Opus 4.7
- **變更名稱：** verify-articles-2026-04
- **涵蓋文件：** 9 份（01_GENERAL、02_CHAT、03_COWORK、04_CODE、05_API、06_MCP、07_SKILL、08_MANAGED_AGENTS、appendix_claude_design_prompt）

## 依 Processing Order 的各篇總計

| # | 報告 | ✅ | ⚠️ | ❌ | 🔍 | Total | Risk |
|---|---|---|---|---|---|---|---|
| 1 | 01_GENERAL.md | 29 | 9 | 2 | 11 | 51 | Medium |
| 2 | 07_SKILL.md | 8 | 9 | 0 | 1 | 18 | **High** |
| 3 | 02_CHAT.md | 12 | 4 | 0 | 3 | 19 | Low |
| 4 | 06_MCP.md | 5 | 8 | 1 | 2 | 16 | **High** |
| 5 | 03_COWORK.md | 14 | 3 | 0 | 3 | 20 | Low |
| 6 | 05_API.md | 13 | 4 | 0 | 1 | 18 | Low–Medium |
| 7 | 08_MANAGED_AGENTS.md | 16 | 1 | 0 | 6 | 23 | Low |
| 8 | appendix | 8 | 0 | 0 | 3 | 11 | Low |
| 9 | 04_CODE.md（scoped） | 17 | 0 | 0 | 10 | 27 | Low（scoped） |
| **合計（全量）** |  | **122** | **38** | **3** | **40** | **203** | — |

> **注意**：04_CODE.md 為 scoped 驗證，未覆蓋 Chrome / Figma MCP / Hooks / Routines / Plugins / Desktop / /batch 詳解 / Subagent 等段落；合計數字不含這些未驗證段落。實際硬性聲明總數預估 250+。
>
> **另注**：03_COWORK.md 於 2026-04-23 新增 Live Artifacts 段落後做了增補驗證（+9 條聲明，7 ✅ / 1 ⚠️ / 1 🔍），詳見本檔結尾的 Addendum 區塊與 `verify/03_COWORK.md` 的 Addendum 段落。上表統計仍為 2026-04-22 主驗證結果，未併入 addendum。

## 跨文件最關鍵 Top 10 修正清單

按「**誤導讀者會產生實際損害**」的嚴重度排序：

### Tier A — 會直接造成讀者操作失敗 / 安全風險（優先修正）

1. **06_MCP.md line 52–56：五個 `@modelcontextprotocol/server-*` 套件皆已官方歸檔**（postgres、sqlite、github、brave-search、puppeteer）。讀者照抄會安裝過時／已知有漏洞的套件；postgres server 已有 SQL injection 公開 CVE 案例。建議整份套件清單用「2026-Q2 活躍版本」置換。

2. **06_MCP.md line 69：設定檔路徑錯誤**。專案層級 MCP 設定應為 `.mcp.json`，**不是** `.claude/settings.json`。讀者照文章寫，Claude Code 不會載入 MCP — 後續除錯完全卡住。

3. **07_SKILL.md line 33：skill 載入優先順序方向反了**。官方為 enterprise > personal > project；文章寫「專案 > 全域 > 插件」屬反向。團隊依此設置 skill 會出現行為不如預期。

4. **07_SKILL.md line 185：`allowed-tools` 語意相反**。官方是「**免詢問**使用權」，文章描述為「**限制**可用工具」。兩者語意相反 — 讀者可能把應允許的 skill 寫成受限版本。

5. **07_SKILL.md line 187：`agent` 欄位型別錯誤**。官方需填 subagent type 名稱，文章寫 `true`，會導致 skill 無法啟動。

### Tier B — 會讓讀者取得不正確的產品資訊（重要修正）

6. **06_MCP.md line 61：`modelcontextprotocol.io/servers` 回傳 HTTP 404**。該 URL 已不存在；建議改為 [GitHub repo](https://github.com/modelcontextprotocol/servers) 或 [MCP Registry](https://registry.modelcontextprotocol.io/)。

7. **01_GENERAL.md line 175 & 07_SKILL.md line 348**：定價與安裝 CLI 命令錯誤。Team 方案非 $30/人/月（實際 $20/$25 標準 + $100 Premium）；`claude plugins install github:...` 非實際語法（正確是 `/plugin install` / `/plugin marketplace add`）。

8. **05_API.md line 152：Claude Haiku 3 退役日期差一天**。官方 Model deprecations 頁為 April 20, 2026，文章寫 04-19。依此規劃 migration cut-off 可能造成服務中斷。

9. **02_CHAT.md line 165–166：檔案上傳上限過時**。官方為 30 MB / 檔、20 檔 / 對話；文章寫 20 MB / 5 檔，會讓讀者低估實際能力。

10. **01_GENERAL.md line 84：Anthropic Labs「2024 年中 2 人起步」無官方出處**。搜尋僅得 2026-01 對外公開。若再版保留這段描述請附來源，否則改寫。

## 跨文件通用 Corrections（會影響多篇）

這些修正不只影響單篇，應在 change 時同步全檔：

- **anthropic.com/pricing → claude.com/pricing** 的 301 重導（出現於 01_GENERAL、05_API、08_MANAGED_AGENTS）
- **Opus 4.7 breaking changes 相關段** 需在 05_API、07_SKILL、01_GENERAL 之 temperature/top_p/top_k 描述同步補註「非預設值會 400」
- **Tokenizer 換算（0.75 英文字 / 0.5 中文字）** 需全檔加註「4.7 以前的近似值；4.7 新 tokenizer 需多預估 1.0–1.35×」

## 覆蓋率與局限性

- **全檔覆蓋但 04_CODE scoped**：9 份文章**全部已產出驗證報告**，但 04_CODE.md 2504 行僅覆蓋安裝 / CLAUDE.md / Auto Mode / Sandbox / slash commands 主體 / 2026-04 事件，Chrome / Figma MCP / Hooks / Routines / Plugins / Desktop / Subagent 等段落留待下一輪。
- **URL 可達性未 100% 掃描**：本次對 5 條外部連結（01_GENERAL）與主要官方 URL 做 WebFetch 驗證；04_CODE 的 44 條連結僅抽樣。
- **中譯語意差異排除**：依 design spec，純中譯措辭差異不屬硬性聲明，附錄的英中對照未做逐段校對。
- **自我 bias 風險**：驗證者與撰寫者同屬 Claude 家族模型，對同一事實可能「同樣合理推論」。本輪已用「無外部 URL 即 ❌/🔍」規則防守，但不能完全排除遺漏。

## 後續建議

1. **立即行動（下一輪 change）**：修正 Tier A 的 5 條（06_MCP 套件清單 + 設定檔路徑、07_SKILL 三條技術錯誤）。
2. **近期**：處理 Tier B 的 5 條 + 跨文件通用 corrections。
3. **待排程**：04_CODE.md 未驗證段落（Chrome、Figma MCP、Hooks、Routines、Plugins、Desktop、Subagent、完整 URL 可達性掃描）。
4. **長期**：每 3–6 個月重新驗證一次，因為 Anthropic 產品與定價變化頻繁（2026-Q1~Q2 已有多次政策、套件、命名異動）。

## 附：驗證流程可延續性

本次驗證產出的 `openspec/specs/article-verification/spec.md`（經本 change archive 後成為正式 spec）訂定了**可重複的驗證流程**：

- 報告模板（`verify/_template.md`）
- 四分類標準（✅/⚠️/❌/🔍）
- 來源可信度優先序（官方 → 附屬 → 一線媒體 → 社群）
- Verifier self-bias mitigation 規則（無外部 URL 即 ❌）

未來可由同一人、不同模型、或 ChatGPT / Gemini 等他家模型，依此流程做第二輪交叉驗證，得到**可比較的結果**。

---

## Addendum: 2026-04-23 — 03_COWORK.md Live Artifacts 段落

- **觸發原因**：Anthropic 於 2026-04-20 官方發布 Live Artifacts 功能；03_COWORK.md 新增對應章節。
- **範圍**：僅新增段落，不重新驗證 2026-04-22 主報告的 20 條既有 claims。
- **增補聲明數**：9 條（7 ✅ / 1 ⚠️ / 0 ❌ / 1 🔍）
- **關鍵事實**：
  - Live Artifacts 為 Cowork 內連接器驅動的**持久化**產出物，開啟即刷新 — 多家媒體與 Claude Help Center 確認
  - 支援 **Pro / Max / Team / Enterprise** 方案於 **Claude web 與 Desktop**
  - 連接器透過 **MCP** 架構；原生覆蓋 Gmail / Google Calendar / Slack / Notion / HubSpot / Jira / Salesforce / Snowflake / Asana 等 38+ 工具
- **從使用者原稿修正的兩處**：
  1. **「支援 Power BI」** → 改為「Power BI / BigQuery / 內部系統可透過**第三方 MCP connector** 擴充」；Anthropic 原生清單未列 Power BI
  2. **「手機上還沒辦法用」** → 改為「手機端功能有限」；官方明示 web 與 desktop 支援完整，但未完全排除手機可用性
- **連動更新**：
  - `openspec/specs/cowork-collaboration/spec.md` 新增 `Requirement: Document Live Artifacts feature`（附兩個 Scenario）
  - `verify/03_COWORK.md` 結尾新增 Addendum 區塊，列 9 條聲明與查證結果

> 下一輪主驗證時可將本 addendum 併入 03_COWORK.md 的主 claims table 重新彙總。
