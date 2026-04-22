<!--
========================================================================
  VERIFIER GUIDELINES — READ BEFORE USING THIS TEMPLATE
========================================================================

  這份模板是產出驗證報告的唯一標準樣式。以下守則一併遵守，不得省略。

  1. Do not modify source articles
  ---------------------------------
  禁止修改任何 `0[1-8]_*.md` 或 `appendix_*.md`。
  查無根據的聲明只在本報告中給出建議，不得編輯原文。

  2. Claim detection scope（何謂「硬性聲明」）
  ----------------------------------------------
  ✅ 白名單（必須列入 Claims Table）:
    - 具體日期（「2026-03-23 推出 Computer Use Mac 版」）
    - 版本號（「Claude Code v2.1.113」）
    - benchmark 數字（「SWE-bench Verified 87.6%」）
    - 定價（「$0.08 / session-hour」）
    - URL 可達性（文中出現的外部連結）
    - 事件時序（「npm 於 2026-03-30 發生源碼地圖外洩」）
    - 引用（「Anthropic 工程師 Boris Cherny 表示…」）
    - 命名產品功能（「Claude Code Routines」「Managed Agents API」）
    - 已命名企業案例（「Notion / Rakuten / Asana 採用 Managed Agents」）
    - 統計數字（「44 條外部連結」「2% 試點用戶」）

  ❌ 黑名單（禁止列入）:
    - 操作步驟（「1. 打開 Settings 2. 點選 Capabilities」）
    - 按鈕標籤與 UI 描述（「左上角的 New Chat 按鈕」）
    - 主觀建議（「建議先熟悉 prompt engineering」）
    - 已明示的模糊敘述（「大約」「通常」「大多數情況」）
    - 純修辭與中譯措辭偏好
    - 安裝順序、參數範例之類的純流程

  邊界灰區（需判斷）:
    - 「Claude Opus 4.7 是 Anthropic 目前最新模型」— 算硬性聲明（事實可驗證）。
    - 「Claude Opus 4.7 表現最好」— 不算，屬主觀比較。
    - 「$0.08/session-hour 定價合理」— 「$0.08/session-hour」是硬性聲明、「合理」是主觀，只驗前半。

  3. Web verification method（查證腳本）
  ----------------------------------------
  每條硬性聲明依下列步驟查證：

  Step A：取證來源（**Source credibility hierarchy** 從上往下試）
    1. 官方一手：anthropic.com / claude.com / code.claude.com /
       platform.claude.com / modelcontextprotocol.io / Anthropic
       官方 GitHub / 官方 blog / Engineering blog / 官方 YouTube。
    2. 官方附屬：support.claude.com / 官方 changelog / API reference。
    3. 一線媒體（僅引述官方發言或 benchmark 時採用）：TechCrunch /
       The Verge / The Register / iThome / TheNewStack / Bloomberg / WSJ。
    4. 社群（僅作為補充，不得作為唯一證據）：HN 討論、知名個人部落格。

    ❌ 絕不引用：推文、Reddit 貼文、未具名論壇（除非能連到一手出處）。

  Step B：查詢策略
    - 英文優先（官方來源多為英文）。
    - 使用 `WebSearch` 找候選頁面，確認後再 `WebFetch` 讀原文。
    - 文中已附連結者，必須 `WebFetch` 驗證可達性與主題相符。
    - HTTP 500/404 ≠ 聲明為偽；須區分「伺服器錯誤」vs「頁面不存在」，
      必要時查 Web Archive 快照。
    - 不重複 fetch 同一 URL；token 過長時只摘錄關鍵段。

  Step C：衝突處理
    - 官方與媒體數字衝突 → 以官方為準，但在 Source/Suggestion 註記衝突。
    - 同層級多來源不一致 → 以時間最新者為主，並註記。

  4. Verifier self-bias mitigation（防自我肯定）
  -----------------------------------------------
  絕對禁止以「模型覺得合理」作為 ✅ 的理由。
  任何 ✅ Verified 都必須有可查核的外部 URL。
  找不到證據即標為 ❌ 或 🔍，不得放行。

  5. 查無根據時的處置 — 只建議，不刪除
  ---------------------------------------
  ❌ Unsupported 的原文處置**由作者決定**，本報告只能建議三選：
    (a) delete         — 整條聲明移除
    (b) rewrite        — 改寫為較不斷言版本（例：把具體數字改為一般描述）
    (c) retain + note  — 原文保留但加「> 註：本數字來源待補」

  6. 填寫格式注意
  -------------------
  - 行號以原文實際行號為準（1-based）。
  - 摘錄不超過 120 字；超長時用「…」縮略並保留關鍵動詞與數字。
  - 四分類標記一律用 emoji + 文字：✅ Verified / ⚠️ Needs correction /
    ❌ Unsupported / 🔍 Unverifiable。
  - 一條聲明跨多行時，以起始行為主、補註結束行。

========================================================================
  以下是報告本體，請填入具體內容。
========================================================================
-->

# Verification Report: `<SOURCE_FILENAME>.md`

## Header

| Field | Value |
|---|---|
| Source filename | `<SOURCE_FILENAME>.md` |
| Source line count | `<N>` |
| External URL count in source | `<K>` |
| Verification date | `YYYY-MM-DD` |
| Verifier | Claude Opus 4.7 |

## Claims Table

| # | Line | Claim (excerpt) | Type | Result | Source / Suggestion |
|---|---|---|---|---|---|
| 1 |  |  |  |  |  |
| 2 |  |  |  |  |  |

**類型（Type）列舉**：date、version、benchmark、price、url-reachability、
timeline、quotation、product-spec、case-study、statistic、other。

## Summary

### Counts

| Marker | Count |
|---|---|
| ✅ Verified | 0 |
| ⚠️ Needs correction | 0 |
| ❌ Unsupported | 0 |
| 🔍 Unverifiable | 0 |
| **Total** | 0 |

### Top 3–5 Critical Corrections

1. *(填入最關鍵的修正，含行號與建議動作)*
2.
3.

### Overall Risk Assessment

**Risk level:** Low / Medium / High

*(一句話說明整體風險：例「多數 benchmark 數字缺乏官方來源佐證，建議補連結或改寫為非斷言版本」)*

### Detection Coverage Note

本報告檢測到 N 條硬性聲明。模糊敘述、操作步驟、純主觀評價未列入。
後續讀者（ChatGPT / Gemini）若發現本報告漏掉的聲明，屬預期行為，
因為本驗證不保證 100% 覆蓋。
