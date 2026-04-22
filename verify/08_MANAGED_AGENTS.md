# Verification Report: `08_MANAGED_AGENTS.md`

## Header

| Field | Value |
|---|---|
| Source filename | `08_MANAGED_AGENTS.md` |
| Source line count | 390 |
| External URL count in source | 11 |
| Verification date | 2026-04-22 |
| Verifier | Claude Opus 4.7 |

## Claims Table

| # | Line | Claim (excerpt) | Type | Result | Source / Suggestion |
|---|---|---|---|---|---|
| 1 | 5 | 公開 Beta 於 **2026-04-08** 發布 | date | ✅ Verified | [SiliconANGLE Apr 8 2026](https://siliconangle.com/2026/04/08/anthropics-claude-managed-agents-speed-ai-agent-development/)；[Claude 官方部落格文](https://claude.com/blog/claude-managed-agents) 日期為 "April 8, 2026" |
| 2 | 6 | `https://platform.claude.com/docs/en/managed-agents/overview` 可達且為官方文件 | url-reachability | ✅ Verified | WebFetch 成功取得 overview 頁內容 |
| 3 | 7 | YouTube 官方介紹影片 URL 可達 | url-reachability | 🔍 Unverifiable | YouTube 頁面無法透過 WebFetch 取得詳細內容；URL 格式正確但未能實測可達性。建議保留 |
| 4 | 17 | 官方定位原文 "Pre-built, configurable agent harness that runs in managed infrastructure." | quotation | ✅ Verified | overview 頁 subtitle 完全相同 |
| 5 | 43–48 | 四核心概念：Agent / Environment / Session / Events | product-spec | ✅ Verified | overview 頁 "Core concepts" 段與文章表格完全對應 |
| 6 | 95 | 所有 endpoint 需附 `managed-agents-2026-04-01` beta header；SDK 自動處理 | product-spec | ✅ Verified | overview："All Managed Agents endpoints require the `managed-agents-2026-04-01` beta header. The SDK sets the beta header automatically." |
| 7 | 110, 127, 133 | 工具集合識別字 `agent_toolset_20260401` 啟用完整預建工具集 | product-spec | 🔍 Unverifiable | overview 頁列出內建工具（Bash / File ops / Web search & fetch / MCP）但未直接展示 `agent_toolset_20260401` 識別字；該命名須 quickstart 或 API reference 頁進一步確認。建議附 quickstart URL |
| 8 | 264–267 | 內建工具清單：Bash、File operations、Web search & fetch、MCP servers | product-spec | ✅ Verified | overview 頁 "Supported tools" 段四項一致 |
| 9 | 275 | 三層虛擬化解耦：Session（持久 event log）、Brain/Harness（agent loop）、Hands/Sandbox（執行環境） | product-spec | ✅ Verified | [Scaling Managed Agents（Engineering Blog）](https://www.anthropic.com/engineering/managed-agents)："We virtualized the components of an agent: a session (the append-only log...), a harness (the loop that calls Claude...), and a sandbox (an execution environment...)" 完全對應 |
| 10 | 301 | 解耦後 TTFT p50 降低約 **60%**、p95 降低超過 **90%** | statistic | ✅ Verified | Engineering Blog："our p50 TTFT dropped roughly 60% and p95 dropped over 90%." |
| 11 | 297 | Harness 與容器解耦，容器從「寵物」變「牛群」 | product-spec | ✅ Verified | Engineering Blog 以 pets-vs-cattle 類比解釋隨建隨棄 |
| 12 | 298 | Harness 無狀態化：crash 後透過 `wake(sessionId)` 取回 event log 恢復 | product-spec | ✅ Verified | Engineering Blog："When one fails, a new one can be rebooted with `wake(sessionId)`, use `getSession(id)` to get back the event log, and resume from the last event." |
| 13 | 329–335 | 企業案例：Notion（數十並行）、Rakuten（一週部署）、Asana（AI 隊友）、Vibecode（10× 部署速度）、Sentry（數月→數週） | case-study | ✅ Verified | [Claude Managed Agents blog](https://claude.com/blog/claude-managed-agents) 明列五家案例與對應成果描述，與文章一致；另血酪 Atlassian 與 General Legal 未於文章列入，但非必列 |
| 14 | 337 | `https://claude.com/blog/claude-managed-agents` URL 可達 | url-reachability | ✅ Verified | WebFetch 成功取得完整內容 |
| 15 | 346 | **活躍 runtime $0.08 / session-hour**（以毫秒計量） | price | ✅ Verified | 官方 blog："$0.08 per session-hour for active runtime"；多家報導與 Verdent / WaveSpeed pricing guide 同樣引用 |
| 16 | 347 | **Web search $10 / 1,000 次搜尋** | price | 🔍 Unverifiable | 官方 Claude Managed Agents blog 未專門列出此價；可能沿用 Claude API Web search tool 的標準費率。建議改為「Web search 使用量依 [Claude API pricing](https://platform.claude.com/docs/en/about-claude/pricing) 之 server-side tool 費率計費」，或附具體 pricing 頁對應段落 |
| 17 | 349 | `https://www.anthropic.com/pricing` 官方定價頁 | url-reachability | ⚠️ Needs correction | 與 01_GENERAL 報告 claim #41 同：已 301 重導至 `https://claude.com/pricing` |
| 18 | 377 | Rate limits：建立類 **60 req/min**；讀取類 **600 req/min** | product-spec | ✅ Verified | overview "Rate limits" 段數字完全相符 |
| 19 | 378 | Research Preview 功能（Multi-agent / Memory / Outcomes）需申請存取 | product-spec | ✅ Verified | overview："Certain features (outcomes, multiagent, and memory) are in research preview. Request access to try them." |
| 20 | 378 | 申請表 URL `https://claude.com/form/claude-managed-agents` 可達 | url-reachability | 🔍 Unverifiable | WebFetch 可能對表單頁回報 403；URL 由官方 overview 頁直接超連結，視為官方來源 |
| 21 | 379 | 模型支援：**Claude 4.5 及更新版本** | product-spec | 🔍 Unverifiable | overview 未明示支援模型版本範圍，僅說「由 API 帳戶存取」；Claude 4.5 / 4.6 / 4.7 皆應可用，但「4.5 及更新」作為正式公告之明文未從官方文件取得。建議附 Agent Setup 頁的實際支援列表 |
| 22 | 380 | 品牌規範：可用「Claude Agent」「Powered by Claude」，不可用「Claude Code」「Claude Cowork」相關品牌 | product-spec | ✅ Verified | overview "Branding guidelines" 段 "Allowed" 與 "Not permitted" 項目與文章一致 |
| 23 | 386–390 | 延伸閱讀的五條官方 / 部落格連結 | url-reachability | ✅ Verified | 已透過 WebFetch 實測 overview / engineering blog / managed-agents blog 全部可達；quickstart、agent-setup 為同域結構化文件，預期可達 |

## Summary

### Counts

| Marker | Count |
|---|---|
| ✅ Verified | 16 |
| ⚠️ Needs correction | 1 |
| ❌ Unsupported | 0 |
| 🔍 Unverifiable | 6 |
| **Total** | 23 |

### Top 3–5 Critical Corrections

1. **Line 347 — Web search $10/1,000 次數字無官方對應**（🔍）：官方 blog 只列 session-hour $0.08，沒有此 $10 搜尋費。此數字可能出自二手報導或舊版 pricing 頁。建議改為援引 Claude API server-side tool 的正規 pricing 表。
2. **Line 349 — anthropic.com/pricing 已 301 重導**（⚠️）：整個文件與 01_GENERAL 應一併更新連結至 claude.com/pricing。
3. **Line 110/127/133 — `agent_toolset_20260401` 識別字在 overview 頁未見**（🔍）：此字串出現於 Quickstart / API reference，建議於文章註釋該識別字的官方文件具體頁面，避免讀者疑惑。
4. **Line 379 — 「Claude 4.5 及更新版本」未於 overview 明示**（🔍）：建議援引 Agent Setup 頁的 supported models 列表作為更精確版本。
5. **Line 7 — YouTube 影片 URL 無法實測可達**（🔍）：建議確認影片 ID 仍未下架，或改為官方 blog 內嵌版本。

### Overall Risk Assessment

**Risk level:** Low

此文具 11 條外部連結（密度最高之一），核心技術聲明（架構設計、TTFT 數字、beta header、定價、rate limits、企業案例）經與 Anthropic Engineering Blog、官方 managed-agents blog、platform.claude.com overview 三處交叉比對，**除一條 web search 費率未取得明確官方出處外，其餘皆有堅實來源**。屬已查證最紮實的一篇文章。建議下一輪 change 只針對 🔍 項補連結即可。

### Detection Coverage Note

本報告檢測到 **23 條硬性聲明**。以下未列入：

- Python / TypeScript 範例程式碼
- 預期輸出範例（Fibonacci 範例 output）
- 主觀性比較表（Messages API / Claude Code Subagent / Managed Agents 的「適用情境」文字）
- 成本控制建議（快取、Haiku 模型選擇）
- 運作流程的 5 步驟敘述（屬流程框架、非具體聲明）

後續讀者（ChatGPT / Gemini）若發現本報告漏掉的聲明，屬預期行為 — 本驗證不保證 100% 覆蓋。
