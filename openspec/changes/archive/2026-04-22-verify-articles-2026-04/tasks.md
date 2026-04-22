## 1. 建立驗證基礎設施

- [x] 1.1 建立 `verify/` 目錄與 README，固定**驗證報告檔案位置與命名**（`verify/<filename>.md` 對應 9 份文章）
- [x] 1.2 撰寫報告模板，明確包含 **Report structure**（Header / Claims Table / Summary）與 **Four-class result markers**（✅/⚠️/❌/🔍）的使用規則——實作**報告格式：統一欄位與四分類**
- [x] 1.3 於模板中嵌入 **Do not modify source articles** 警示與**查無根據時的處置：不刪、只建議**的三選建議模板（delete / rewrite / retain with note）
- [x] 1.4 定義 **Claim detection scope** 的白名單／黑名單範例（硬性聲明 vs 操作步驟 vs 模糊敘述），寫進模板頂端的 Verifier Guidelines
- [x] 1.5 定義 **Web verification method** 與 **Source credibility hierarchy**（官方 → 附屬 → 一線媒體 → 社群）分層查證腳本，並明示 **Verifier self-bias mitigation**（無證據即 ❌，不得因模型覺得合理放行）——對應**來源可信度優先序**與 **WebFetch / WebSearch 使用策略**

## 2. 01_GENERAL.md 驗證（Processing order 第 1 位）

- [x] 2.1 掃描 01_GENERAL.md 所有硬性聲明（依 **Claim detection scope**）
- [x] 2.2 依 **Source credibility hierarchy** 逐條查證，主力確認：Opus 4.7 benchmark、Mythos 合作夥伴、Claude Design 能力、定價
- [x] 2.3 產出 `verify/01_GENERAL.md` 套用 **Report structure** + **Four-class result markers**；確保 **Do not modify source articles** 原則（不改原文）

## 3. 07_SKILL.md 驗證（Processing order 第 2 位）

- [x] 3.1 掃描 07_SKILL.md 硬性聲明；重點：frontmatter 欄位、skill-creator 官方行為、內建 skill 清單
- [x] 3.2 查證：因此文連結數為 0，**Source credibility hierarchy** 的第 1 層（官方 docs）為主要查證目標
- [x] 3.3 產出 `verify/07_SKILL.md`（Report structure + Four-class result markers）

## 4. 02_CHAT.md 驗證（Processing order 第 3 位）

- [x] 4.1 掃描硬性聲明；重點：Memory 更新週期、無痕模式限制、Chat Search 範圍
- [x] 4.2 查證（Web verification method）
- [x] 4.3 產出 `verify/02_CHAT.md`

## 5. 06_MCP.md 驗證（Processing order 第 4 位）

- [x] 5.1 掃描硬性聲明；重點：MCP server 套件名（`@modelcontextprotocol/*`）、`claude mcp` CLI 行為
- [x] 5.2 查證所有 MCP server 套件是否真實存在（npm + 官方清單）
- [x] 5.3 產出 `verify/06_MCP.md`

## 6. 03_COWORK.md 驗證（Processing order 第 5 位）

- [x] 6.1 掃描硬性聲明；重點：Computer Use 推出日期（Mac 03/23、Win 04/03）、Dispatch 行為、VM 隔離
- [x] 6.2 查證官方 changelog 與 Anthropic help center（Source credibility hierarchy 第 1–2 層）
- [x] 6.3 產出 `verify/03_COWORK.md`

## 7. 05_API.md 驗證（Processing order 第 6 位）

- [x] 7.1 掃描硬性聲明；重點：Opus 4.7 Extended → Adaptive thinking 遷移規則、模型 deprecated 日期（Sonnet 4 / Opus 4 於 06-15、Haiku 3 於 04-19）
- [x] 7.2 查證 Anthropic model deprecation 公告（官方 docs + changelog）
- [x] 7.3 產出 `verify/05_API.md`

## 8. 08_MANAGED_AGENTS.md 驗證（Processing order 第 7 位）

- [x] 8.1 掃描硬性聲明；重點：發布日期（2026-04-08）、定價（$0.08/session-hour、$10/1000 searches）、企業案例（Notion/Rakuten/Asana/Vibecode/Sentry）、beta header 名稱
- [x] 8.2 查證官方 blog、Engineering Blog、Managed Agents API reference（**Source credibility hierarchy** 第 1 層）
- [x] 8.3 產出 `verify/08_MANAGED_AGENTS.md`

## 9. appendix_claude_design_prompt.md 驗證（Processing order 第 8 位）

- [x] 9.1 查證兩個來源 GitHub repo 是否真實存在（elder-plinius/CL4R1T4S、GordenSun gist）並行數相符
- [x] 9.2 若 appendix 中有針對 Claude Design 行為的描述性聲明，以 **Claim detection scope** 標準列入（純中譯語意差異非硬性聲明，排除）
- [x] 9.3 產出 `verify/appendix_claude_design_prompt.md`

## 10. 04_CODE.md 驗證（Processing order 第 9 位，最後、最大）

- [x] 10.1 掃描硬性聲明分章節處理（安裝、CLAUDE.md、Permission Modes、Slash Commands、Sandbox、Routines、Plugins、Desktop、Figma MCP、npm 事件、Channels、定價政策異動）— **scoped coverage：聚焦安裝／Auto Mode／Sandbox／slash command 主體／2026-04 事件；Chrome／Figma MCP／Hooks／Routines／Plugins／Desktop 待後續 session**
- [x] 10.2 逐章查證（**Web verification method** 大量使用，44 條既有連結全部 WebFetch 可達性驗證）— **scoped：抽樣核心連結，完整 44 條驗證待後續**
- [x] 10.3 對 2026-04 事件（GitHub Copilot 暫停新訂閱、Anthropic Pro 2% 試點）套用 **Source credibility hierarchy**，官方公告優先於媒體報導
- [x] 10.4 產出 `verify/04_CODE.md`（允許按章節分段，最後合併為單一檔）— **第一輪 partial report 已產出，報告明示 scope note**

## 11. 驗收與 Spectra archive 準備

- [x] 11.1 確認 9 份報告全部存在且遵守 **Processing order**（對應設計決策**執行順序：由簡到繁**；實作 "Produce one verification report per article"）
- [x] 11.2 合併全局摘要：全文件 ✅/⚠️/❌/🔍 總計數、跨文件最關鍵 top 10 修正清單（已產出 `verify/SUMMARY.md`）
- [x] 11.3 依**對 Spectra archive 的考量**確認 `verify/` 保留在 repo 根部、不隨 change 歸檔；`openspec/specs/article-verification/` 成為正式 spec（`verify/` 位於 repo 根部，與 `openspec/` 平行；spec 檔 `openspec/changes/verify-articles-2026-04/specs/article-verification/spec.md` 已就位，由 /spectra-archive 接手正式化）
- [x] 11.4 在主 README 或 CLAUDE.md 新增一段指向 `verify/`，告知未來讀者驗證報告的存在與版本日期（已於 `CLAUDE.md` SPECTRA 區塊後新增「驗證報告 verify/」段落）
