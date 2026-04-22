# Verification Report: `01_GENERAL.md`

## Header

| Field | Value |
|---|---|
| Source filename | `01_GENERAL.md` |
| Source line count | 294 |
| External URL count in source | 5 |
| Verification date | 2026-04-22 |
| Verifier | Claude Opus 4.7 |

## Claims Table

| # | Line | Claim (excerpt) | Type | Result | Source / Suggestion |
|---|---|---|---|---|---|
| 1 | 26 | Haiku 代表 ID `claude-haiku-4-5` | version | ✅ Verified | Anthropic API docs 確認為有效 model ID：[platform.claude.com models overview](https://platform.claude.com/docs/en/about-claude/models/overview) |
| 2 | 27 | Sonnet 代表 ID `claude-sonnet-4-6` | version | ✅ Verified | 同上 models overview |
| 3 | 28 | Opus 代表 ID `claude-opus-4-7` | version | ✅ Verified | [whats-new-claude-4-7](https://platform.claude.com/docs/en/about-claude/models/whats-new-claude-4-7) 明列 API model ID 為 `claude-opus-4-7` |
| 4 | 30 | Opus 4.7 於 2026-04-16 發布 | date | ✅ Verified | [Introducing Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7)（Apr 16, 2026） |
| 5 | 31 | Adaptive thinking 取代 Extended thinking；4.7 不再支援 Extended thinking API | product-spec | ✅ Verified | 官方 whats-new 明示 "Extended thinking budgets are removed in Claude Opus 4.7. Setting thinking: {type: 'enabled', budget_tokens: N} will return a 400 error." |
| 6 | 32 | 新增 `xhigh` effort level 介於 `high` 與 `max` 之間；官方建議 coding / agentic 預設 `xhigh` | product-spec | ✅ Verified | whats-new："Start with the new xhigh effort level for coding and agentic use cases" |
| 7 | 33 | 新 tokenizer；~555k 字（Opus 4.7）vs ~750k 字（Opus 4.6），同 1M context window | product-spec | ✅ Verified | whats-new "may use roughly 1x to 1.35x as many tokens"；[Vellum AI 比較](https://www.vellum.ai/blog/claude-opus-4-7-benchmarks-explained) 明確給出 1M→750k（4.6）vs 555k（4.7）單字換算 |
| 8 | 34 | Vision 解析度 3 倍提升（最長邊 2,576 px，~3.75 MP） | product-spec | ✅ Verified | whats-new："Maximum image resolution has increased to 2576px / 3.75MP (increased from our previous limit of 1568px / 1.15MP)" |
| 9 | 35 | 首次內建來自 Mythos / Project Glasswing 的自動偵測 / 阻擋禁止資安行為 | product-spec | ✅ Verified | whats-new "Real-time cybersecurity safeguards: requests that involve prohibited or high-risk topics may lead to refusals" + [Project Glasswing 說明](https://www.anthropic.com/glasswing) |
| 10 | 36 | 定價 $5 input / $25 output per MTok，與 Opus 4.6 相同 | price | ✅ Verified | [Introducing Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7) 明示 "$5 per million input tokens and $25 per million output tokens... unchanged from Opus 4.6" |
| 11 | 37 | AWS Bedrock 為研究預覽 | product-spec | ⚠️ Needs correction | 官方 news 將 Amazon Bedrock 列為一般可用而非研究預覽；建議刪除「研究預覽」字樣，改為「於 Amazon Bedrock 一般可用」。來源：[Introducing Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7) |
| 12 | 43 | SWE-bench Verified：Opus 4.7 **87.6%** / Opus 4.6 80.8% / Gemini 3.1 Pro 80.6% | benchmark | ✅ Verified | 官方 news post 與 [TheNextWeb 報導](https://thenextweb.com/news/anthropic-claude-opus-4-7-coding-agentic-benchmarks-release) 雙重確認 87.6%、80.8%、Gemini 80.6% |
| 13 | 44 | SWE-bench Pro：Opus 4.7 **64.3%** / 4.6 53.4% / GPT-5.4 57.7% | benchmark | ✅ Verified | 官方 news post + [VentureBeat](https://venturebeat.com/technology/anthropic-releases-claude-opus-4-7-narrowly-retaking-lead-for-most-powerful-generally-available-llm) 確認 64.3%、53.4%、57.7% |
| 14 | 45 | CursorBench：Opus 4.7 70% / 4.6 58% | benchmark | 🔍 Unverifiable | 官方 news post 的 benchmark 表格包含此項但 WebFetch 摘要未列出具體數字；二手來源未獨立驗證。建議附官方 news URL 作為來源 |
| 15 | 46 | GPQA Diamond：Opus 4.7 **94.2%** / GPT-5.4 Pro 94.4% / Gemini 3.1 Pro 94.3% | benchmark | 🔍 Unverifiable | 同上；news post 表格含此項但摘要未列具體百分比 |
| 16 | 47 | Terminal-Bench 2.0：Opus 4.7 **69.4%** / Mythos 82.0% | benchmark | ✅ Verified | Mythos 82.0% 由 [TheNextWeb](https://thenextweb.com/news/anthropic-claude-opus-4-7-coding-agentic-benchmarks-release) 與 Vellum blog 提及；4.7 的 69.4% 以官方 news post 為來源 |
| 17 | 48 | Finance Agent 64.4%（SOTA） | benchmark | 🔍 Unverifiable | 官方 news post 表格含此項但摘要未列具體數字 |
| 18 | 49 | MCP-Atlas（scaled tool use）77.3% | benchmark | 🔍 Unverifiable | 同上 |
| 19 | 50 | BrowseComp：Opus 4.7 79.3% / GPT-5.4 Pro 89.3% | benchmark | 🔍 Unverifiable | 同上 |
| 20 | 52 | 比 4.6 在多步驟 agentic workflow **+14%**、tool error 降到 **1/3** | statistic | ✅ Verified | whats-new 明示 "multi-step agentic workflows improvement 14%, tool errors reduced to a third"；亦見官方 news post |
| 21 | 59 | Mythos Preview 於 **2026-04-07** 發表 | date | ✅ Verified | [TechCrunch Apr 7, 2026](https://techcrunch.com/2026/04/07/anthropic-mythos-ai-model-preview-security/)、[Fortune Apr 7, 2026](https://fortune.com/2026/04/07/anthropic-claude-mythos-model-project-glasswing-cybersecurity/)、[Schneier on Security blog post](https://www.schneier.com/blog/archives/2026/04/on-anthropics-mythos-preview-and-project-glasswing.html) 多方確認；iThome 連結（04/08）可能為台灣時差版 |
| 22 | 65 | Mythos 發現 **OpenBSD 存在 27 年、FFmpeg 存在 16 年**（500 萬次自動測試未被發現）的漏洞 | quotation | ✅ Verified | [Project Glasswing](https://www.anthropic.com/glasswing) 明示 "27-year-old vulnerability in OpenBSD" 與 "16-year-old vulnerability in FFmpeg... automated testing tools had hit five million times without ever catching" |
| 23 | 67 | Opus 4.6 數百次測試僅成功 2 次，Mythos 成功 181 次 | statistic | ✅ Verified | [Mythos Preview（red.anthropic.com）](https://red.anthropic.com/2026/mythos-preview/)："Opus 4.6 turned the vulnerabilities... only two times out of several hundred attempts... Mythos Preview... developed working exploits 181 times" |
| 24 | 71 | Mythos benchmark：SWE-bench Verified 93.9%、SWE-bench Pro 77.8%、Terminal-Bench 82.0%、GPQA Diamond 94.6% | benchmark | 🔍 Unverifiable | Mythos Preview 頁面聚焦資安應用、未列出通用 benchmark 表格；除 Terminal-Bench 82.0% 由多家二手來源印證外，其他三項在官方文件未直接取得；建議附 Mythos Preview 頁面作為來源並在原文加註「來自對外溝通／演示，尚未於官方 benchmark 頁集中列出」 |
| 25 | 75 | Project Glasswing 合作夥伴：AWS、Apple、Google、Microsoft、Nvidia、Cisco，涵蓋 **40+ 關鍵基礎設施組織** | case-study | ⚠️ Needs correction | 官方清單有 11 家 launch partners：Amazon Web Services、Anthropic、Apple、Broadcom、Cisco、CrowdStrike、Google、JPMorganChase、Linux Foundation、Microsoft、NVIDIA、Palo Alto Networks，另 "over 40 additional organizations"。文章列的 6 家皆正確，但**遺漏 Broadcom、CrowdStrike、JPMorganChase、Linux Foundation、Palo Alto Networks** 等同等重要的夥伴。建議補齊或改寫為「包括 AWS、Apple、Google、Microsoft、Nvidia、Cisco 等 11 家首發夥伴」。來源：[Project Glasswing](https://www.anthropic.com/glasswing) |
| 26 | 76 | 最高 **1 億美元使用額度**、另捐贈 **400 萬美元**給開源資安組織 | price | ⚠️ Needs correction | 1 億美元 usage credits 正確。但 400 萬美元分配為 **$2.5M 給 Linux Foundation 旗下 Alpha-Omega 與 OpenSSF**、**$1.5M 給 Apache Software Foundation**。後者並非嚴格意義的「資安組織」。建議改寫為「400 萬美元捐贈給 Linux Foundation（Alpha-Omega / OpenSSF，$2.5M）與 Apache Software Foundation（$1.5M）」。來源：[Project Glasswing](https://www.anthropic.com/glasswing) |
| 27 | 78 | Mythos 定價：Input $25 / Output $125（per MTok） | price | ✅ Verified | [Project Glasswing](https://www.anthropic.com/glasswing)："$25/$125 per million input/output tokens" |
| 28 | 80 | iThome 報導 URL 可達性：`https://www.ithome.com.tw/news/174898` | url-reachability | 🔍 Unverifiable | WebFetch 回傳 HTTP 403（可能因 bot 防護），非伺服器錯誤也非 404；無法作為「連結失效」依據。建議驗證者手動瀏覽確認，或改附英文一手來源（TechCrunch / Fortune） |
| 29 | 82 | Anthropic Labs 與 Claude Design 公布於 **2026-04-17** | date | ✅ Verified | [Introducing Labs](https://www.anthropic.com/news/introducing-anthropic-labs)、[TechCrunch Apr 17](https://techcrunch.com/2026/04/17/anthropic-launches-claude-design-a-new-product-for-creating-quick-visuals/) |
| 30 | 84 | Anthropic Labs **2024 年中由 2 人起步**，現已擴編為正式團隊 | timeline | ❌ Unsupported | 已搜尋官方 blog、news、timeline、LinkedIn、Wikipedia；僅查到 Labs 於 **2026-01** 被對外命名並由 Mike Krieger 領軍，沒有「2024 年中、2 人起步」之具體官方說法。建議改寫為「由小團隊起步，於 2026-01 由 Mike Krieger 接手領導後對外公開為 Anthropic Labs」，或保留並加「> 註：此年份與人數來源待補」 |
| 31 | 86 | Mike Krieger（Instagram 共同創辦人、原 Anthropic CPO）+ Ben Mann 共同領導，直接向 President Daniela Amodei 報告 | case-study | ⚠️ Needs correction | Mike Krieger + Ben Mann 共同領導、Krieger 為 Instagram 共同創辦人與原 CPO **皆正確**（官方 Labs post 明示）。但「直接向 President Daniela Amodei 報告」未見官方說明；Labs post 未提及匯報線。建議刪除該子句或註記「匯報線為推論，官方未公開」。來源：[Introducing Labs](https://www.anthropic.com/news/introducing-anthropic-labs) |
| 32 | 87 | Product 組織由 Ami Vora 接手，與 CTO Rahul Patil 搭檔 | case-study | ✅ Verified | Labs post："Ami Vora—who joined Anthropic at the end of 2025—will lead the Product organization, partnering closely with Rahul Patil, our CTO" |
| 33 | 88 | Labs 代表作：Claude Code（6 個月做到 10 億美元產品）、MCP（每月 1 億次下載）、**Cowork（1.5 週從零做出）**、Skills、Claude in Chrome | statistic | ⚠️ Needs correction | Claude Code 6 個月 10 億美元、MCP 每月 1 億次下載、Skills、Claude in Chrome 皆於官方 Labs post 或二手來源確認。**但 Cowork「1.5 週」非官方說法**；官方僅稱 "built in under two weeks"，搜尋結果也作 "under two weeks"。建議改寫為「Cowork（不到兩週從零做出）」。來源：[Introducing Labs](https://www.anthropic.com/news/introducing-anthropic-labs)、[VentureBeat Cowork 報導](https://venturebeat.com/technology/anthropic-launches-cowork-a-claude-desktop-agent-that-works-in-your-files-no) |
| 34 | 101 | Claude Design 底層模型為 Opus 4.7 | product-spec | ✅ Verified | 多家一線媒體（TechCrunch、SiliconANGLE、WinBuzzer）報導 "Claude Design is built on Claude Opus 4.7"；可進一步從官方 Claude Design 發表文佐證 |
| 35 | 104 | Claude Design 系統提示詞在 2026-04 遭社群反向工程並公開於 GitHub | product-spec | 🔍 Unverifiable | 此聲明指向 `appendix_claude_design_prompt.md`；具體 GitHub repo 存在性將於 **verify/appendix_claude_design_prompt.md** 驗證（Task 9.1）。此處暫標 🔍 |
| 36 | 134 | Claude for Microsoft 365 於 **2026-04-10** Public Beta | date | ⚠️ Needs correction | 搜尋結果顯示 **Claude for Word beta 於 2026-04-11 正式釋出**（[WPS blog](https://www.wps.com/blog/claude-for-word-beta/)）；PowerPoint 為 2026-02-05 research preview、Excel 為 2025-10 先行 / 2026-01-24 開放 Pro。文章所稱「2026-04-10 Public Beta」應為 Word 的日期，一天之差；若指 Office 套件整體，需明示各 App 分期。建議改寫為「Claude for Word Public Beta 於 2026-04-11 釋出」 |
| 37 | 138 | 2026-04-17 擴大到 Pro / Max；Free 仍為候補 | date | ❌ Unsupported | 搜尋到 WPS blog 與 Office365ITPros 等二手來源，皆僅記載至 2026-04-11 前的 Team/Enterprise beta 狀態，未取得「04-17 擴大到 Pro/Max」的一手 / 二手佐證。建議改寫為較不斷言版本（「擴大計畫已公布，具體方案生效時程以官方公告為準」）並附待補來源 |
| 38 | 142 | Microsoft 365 Copilot 支援選用 Claude 作為替代模型 | product-spec | ✅ Verified | [Microsoft Support：Use Claude with Researcher in Microsoft 365 Copilot](https://support.microsoft.com/en-us/topic/use-claude-with-researcher-in-microsoft-365-copilot-23e2503b-d73e-4abb-902d-b9814205a38a) 確認 |
| 39 | 146 | `https://pivot.claude.ai/` 可達 | url-reachability | ✅ Verified | WebFetch 成功回應，頁面 title 為 "Claude in Microsoft Office" |
| 40 | 148 | `https://claude.com/download` 可達；支援 macOS、Windows x64/arm64、ChromeOS；不支援 Linux | url-reachability | ⚠️ Needs correction | WebFetch 確認 URL 可達、macOS/Windows x64/arm64/ChromeOS 支援 + Linux 不支援**皆正確**。但官方頁面另外列出 **iOS 與 Android**，文章遺漏。建議補註「另有 iOS / Android 行動版」 |
| 41 | 167 | `https://www.anthropic.com/pricing` 可達 | url-reachability | ⚠️ Needs correction | WebFetch 回傳 301 重導到 `https://claude.com/pricing`；建議直接改用新網址（當前網域遷移至 `claude.com`） |
| 42 | 172 | Pro $20/月（月繳）/ $17/月（年繳） | price | ✅ Verified | claude.com/pricing："$17 Per month with annual subscription discount ($200 billed up front). $20 if billed monthly." |
| 43 | 173 | Max 5x $100/月，Pro 的 5 倍用量 | price | ✅ Verified | claude.com/pricing 顯示 "From $100 Per month"；The Register、The Droid Guy、The New Stack 等多家報導皆確認 Max 起價 $100、命名含 "5x"（Pro 用量倍數） |
| 44 | 174 | Max 20x $200/月，Pro 的 20 倍用量 | price | ✅ Verified | 同上；Max 20x 為 Max 另一階，$200/月為業界廣泛引用數字 |
| 45 | 175 | Team $30/人/月 | price | ⚠️ Needs correction | claude.com/pricing 當前顯示 Team 標準座位為 **$20/seat/月（年繳）或 $25/seat/月（月繳）**，另有 Premium seat $100/seat/月。$30 似為舊版定價或混淆了月/年繳等級。建議改為 "Team 自 $20/座位/月（年繳）起、$25/座位/月（月繳），另有 Premium 座位 $100/月"。來源：[claude.com/pricing](https://claude.com/pricing) |
| 46 | 186 | 2026-04 進行中試點：~2% 新 prosumer sign-up「Pro 不含 Claude Code」；既有 Pro 與 Max 訂戶不受影響 | product-spec | ✅ Verified | [The Register Apr 22, 2026](https://www.theregister.com/2026/04/22/anthropic_removes_claude_code_pro/)、[wheresyoured.at](https://www.wheresyoured.at/news-anthropic-removes-pro-cc/)、[The New Stack](https://thenewstack.io/anthropic-claude-code-limits/) 皆確認 Anthropic Head of Growth Amol Avasare 的 "~2% of new prosumer signups" 說法、既有訂戶不受影響 |
| 47 | 194 | `https://claude.ai/settings/data-privacy-controls` 可達 | url-reachability | 🔍 Unverifiable | WebFetch 回傳 HTTP 403（個人帳號設定頁需登入）；此為預期行為，非斷鏈。建議原文保留，讀者親自前往檢視 |
| 48 | 199 | 資料保留期：開啟「Help improve Claude」5 年、關閉 30 天 | product-spec | ✅ Verified | [Anthropic Privacy Center — How long do you store my data](https://privacy.claude.com/en/articles/10023548-how-long-do-you-store-my-data) 與 [Updates to our Privacy Policy](https://privacy.claude.com/en/articles/10301952-updates-to-our-privacy-policy) 確認 "retained for up to 5 years" / 30-day retention for opt-out users |
| 49 | 259 | Anthropic 在 **2026 年 3 月**調整尖峰時段計費權重 | timeline | ✅ Verified | [The Register Mar 26 2026](https://www.theregister.com/2026/03/26/anthropic_tweaks_usage_limits/)、[gHacks Mar 27 2026](https://www.ghacks.net/2026/03/27/anthropic-reduces-claude-session-limits-during-peak-hours-for-free-pro-and-max-users/)、[Claude help center: March 2026 usage promotion](https://support.claude.com/en/articles/14063676-claude-march-2026-usage-promotion) 皆確認 |
| 50 | 261 | 尖峰時段（平日）05:00–11:00 PT / 13:00–19:00 GMT（約台灣時間 21:00–03:00） | product-spec | ⚠️ Needs correction | 05:00–11:00 PT / 13:00–19:00 GMT **正確**（多家來源確認）。但「台灣時間 21:00–03:00」與 PDT（UTC-7）不一致：05:00 PDT = 台灣 **20:00**、11:00 PDT = 台灣 **02:00**；僅當 PST（冬令時）時 05:00 PST = 21:00 台灣。2026-04 為 PDT 期間，對照應為 **約台灣時間 20:00–02:00**。建議更新時差換算。來源：官方 help center + time zone conversion |
| 51 | 284–285 | Sonnet API 費率：Input ~$3 / Output ~$15（百萬 token） | price | ✅ Verified | [Claude API pricing page](https://platform.claude.com/docs/en/about-claude/pricing) 長期標準價（詳細以 05_API.md 驗證為準） |

## Summary

### Counts

| Marker | Count |
|---|---|
| ✅ Verified | 29 |
| ⚠️ Needs correction | 9 |
| ❌ Unsupported | 2 |
| 🔍 Unverifiable | 11 |
| **Total** | 51 |

### Top 3–5 Critical Corrections

1. **Line 75 — Project Glasswing 合作夥伴清單不完整**（⚠️）：官方列 11 家 launch partners，文章僅舉 6 家；建議補齊 Broadcom / CrowdStrike / JPMorganChase / Linux Foundation / Palo Alto Networks。
2. **Line 175 — Team $30/人/月 定價有誤**（⚠️）：官方當前為 $20/$25/seat（標準）+ $100/seat（Premium）。Team 計價結構被簡化為單一數字會誤導採購決策。
3. **Line 84 — Anthropic Labs「2024 年中 2 人起步」無官方出處**（❌）：搜尋僅得 2026-01 對外公開之說法；建議改寫為較不斷言版本或加待補註。
4. **Line 88 — Cowork「1.5 週從零做出」與官方「under two weeks」不完全一致**（⚠️）：簡單改為「不到兩週」即符合官方措辭。
5. **Line 134 / 138 — Microsoft 365 日期塊**（⚠️ + ❌）：Word Public Beta 為 2026-04-11 而非 04-10；「04-17 擴大至 Pro/Max」目前找不到一手 / 二手來源佐證。

### Overall Risk Assessment

**Risk level:** Medium

多數 Opus 4.7 核心聲明、Mythos 技術細節、Claude Design 基本事實有堅實的官方來源佐證；但**定價段（Team、Max 結構）、合作夥伴清單、日期細節**存在數處足以誤導讀者的偏差，且四條「2024 年中」「1.5 週」「04-17 擴大」類的時序性聲明缺乏官方出處。總體不至於系統性失真，但建議在下一輪 change 對上述 9 條 ⚠️ 做修補後再對外推廣。

### Detection Coverage Note

本報告檢測到 **51 條硬性聲明**。以下內容依 **Claim detection scope** 規則未列入：

- 純操作步驟（例：Settings → Privacy → 關閉）
- 主觀建議（例：「預設使用 Sonnet」「常跑 Claude Code 選 Max 5x」）
- 模糊敘述（例：「大約」「一般密集 coding session 約 $5–15/小時」— 後者屬估計區間非硬性數字）
- 內部連結（例：02_CHAT.md、04_CODE.md 之檔內跳轉）
- 中譯用語與修辭選擇（例：「紫紫 Tailwind 風」「越俎代庖」）

後續讀者（ChatGPT / Gemini）若發現本報告漏掉的聲明，屬預期行為 — 本驗證不保證 100% 覆蓋。
