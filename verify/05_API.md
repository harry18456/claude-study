# Verification Report: `05_API.md`

## Header

| Field | Value |
|---|---|
| Source filename | `05_API.md` |
| Source line count | 253 |
| External URL count in source | 3 |
| Verification date | 2026-04-22 |
| Verifier | Claude Opus 4.7 |

## Claims Table

| # | Line | Claim (excerpt) | Type | Result | Source / Suggestion |
|---|---|---|---|---|---|
| 1 | 6 | `https://platform.claude.com/docs` 為最新 API 文件入口（URL 可達） | url-reachability | ✅ Verified | WebFetch 成功取得首頁（回傳 HomePage 元件） |
| 2 | 14 | `https://console.anthropic.com` 為 API Key 與 console 入口 | url-reachability | 🔍 Unverifiable | WebFetch 對此類登入型 console 會自動返回 403（需登入），無法直接驗證；該 URL 為 Anthropic 長期使用之 console 網址，預期可達。建議保留 |
| 3 | 34 | Python SDK 套件：`pip install anthropic` | product-spec | ✅ Verified | 官方 Python SDK 套件名為 `anthropic`（廣泛引用、search 結果一致） |
| 4 | 37 | Node.js / TypeScript SDK：`npm install @anthropic-ai/sdk` | product-spec | ✅ Verified | 官方 npm 套件名為 `@anthropic-ai/sdk`（search 結果與 Claude API docs 一致） |
| 5 | 94 | `temperature` 參數：創意程度 0.0–1.0（預設 1.0） | product-spec | ⚠️ Needs correction | 對 Sonnet 4.6 / Haiku 4.5 等前代模型正確。**但 Opus 4.7 breaking change**：設定 `temperature`、`top_p` 或 `top_k` 為非預設值會回傳 400 error（[whats-new-claude-4-7](https://platform.claude.com/docs/en/about-claude/models/whats-new-claude-4-7)）。建議補註「Opus 4.7 不支援 temperature / top_p / top_k 之非預設值，需從請求移除」 |
| 6 | 143 | Haiku 4.5 ID：`claude-haiku-4-5-20251001` | version | ✅ Verified | 官方 [Model deprecations 頁](https://platform.claude.com/docs/en/about-claude/model-deprecations) 的 model status 表格確認此為 API model name、Active 狀態 |
| 7 | 144 | Sonnet 4.6 ID：`claude-sonnet-4-6` | version | ✅ Verified | 同官方 model status 表格：Active 狀態 |
| 8 | 145 | Opus 4.7 ID：`claude-opus-4-7` | version | ✅ Verified | 同官方 model status 表格：Active，Tentative Retirement Date 2027-04-16 或以後 |
| 9 | 147 | Opus 4.7 遷移注意日期：2026-04-16 | date | ✅ Verified | 與 01_GENERAL 報告 claim #4 同源（[Introducing Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7)） |
| 10 | 148 | Opus 4.7 與 4.6 定價相同 $5 / $25 per MTok；4.6 legacy 但仍可用 | price | ✅ Verified | 官方 news post 明示；Model deprecations 頁確認 `claude-opus-4-6` 仍為 Active（非 deprecated），與「legacy 仍可用」一致 |
| 11 | 149 | **Extended thinking → Adaptive thinking**：4.7 不支援 Extended thinking API | product-spec | ✅ Verified | whats-new："Extended thinking budgets are removed in Claude Opus 4.7. Setting thinking: {type: 'enabled', budget_tokens: N} will return a 400 error." |
| 12 | 150 | 新增 `xhigh` effort level（介於 `high` 與 `max`）；官方建議 coding / agentic 預設 xhigh | product-spec | ✅ Verified | whats-new："Start with the new xhigh effort level for coding and agentic use cases" |
| 13 | 151 | 新 tokenizer：~555k 字 vs 4.6 ~750k 字；同 1M context | product-spec | ✅ Verified | whats-new "1x to 1.35x as many tokens"；Vellum AI 比較：1M→750k（4.6）vs 555k（4.7） |
| 14 | 152 | Claude Sonnet 4 / Opus 4 deprecated，**2026-06-15 退役** | date | ✅ Verified | [Model deprecations](https://platform.claude.com/docs/en/about-claude/model-deprecations) 明確列 `claude-sonnet-4-20250514` 與 `claude-opus-4-20250514` 的 Retirement Date 為 **June 15, 2026**，Deprecated 日期為 2026-04-14 |
| 15 | 152 | Claude Haiku 3 **2026-04-19 退役** | date | ⚠️ Needs correction | 官方 Model deprecations 頁 model status 表格與 2026-02-19 deprecation 公告**皆明示 Retirement Date 為 April 20, 2026**，且頁面已加註 "This model was retired April 20, 2026." 文章寫 **04-19** 差一天。（部分二手來源也用 04-19，為常見錯傳。）建議更新為 **2026-04-20** |
| 16 | 154 | `https://www.anthropic.com/pricing` 為 API 費用頁 | url-reachability | ⚠️ Needs correction | 01_GENERAL 報告 claim #41 已記：此 URL **301 重導至 `https://claude.com/pricing`**。建議直接改為新網址 |
| 17 | 210 | 1 token ≈ **0.75 個英文單字** ≈ **0.5 個中文字** | statistic | ⚠️ Needs correction | 英文近似「0.75 words per token」為 OpenAI tokenizer 常見換算，Claude Sonnet 4.6 以前亦相近；**但 Opus 4.7 新 tokenizer 壓縮比不同**（同內容需 1.0–1.35× 更多 token），中英文比例也相應變動。建議註記「此為 Sonnet 4.6 及以前模型的近似值；Opus 4.7 的 tokenizer 壓縮比更高（見 whats-new-claude-4-7）」 |
| 18 | 220–228 | 速率限制（RPM / TPM / TPD）依帳號等級設定 | product-spec | ✅ Verified | 官方 rate limits 概念通用；具體數字依 tier 不同，文章不列具體數字屬審慎做法 |

## Summary

### Counts

| Marker | Count |
|---|---|
| ✅ Verified | 13 |
| ⚠️ Needs correction | 4 |
| ❌ Unsupported | 0 |
| 🔍 Unverifiable | 1 |
| **Total** | 18 |

### Top 3–5 Critical Corrections

1. **Line 152 — Haiku 3 退役日期錯一天**（⚠️）：官方 Model deprecations 頁為 **April 20, 2026**，文章寫 04-19。因為讀者可能依此日期規劃 migration cut-off，錯一天足以引發實際 incident（多排一天沒事、少排一天服務中斷）。
2. **Line 210 — Tokenizer 換算未區分模型代別**（⚠️）：0.75 英文字 / 0.5 中文字的近似值在 Opus 4.7 上**低估** token 用量最多 35%，照此預估 context budget 會超支。
3. **Line 94 — `temperature` 參數在 Opus 4.7 有 breaking change**（⚠️）：文章通用表格未指出 Opus 4.7 不接受非預設 temperature / top_p / top_k。讀者在升級 Opus 4.7 時可能遭遇 400 錯誤。
4. **Line 154 — anthropic.com/pricing 已 301 重導**（⚠️）：應改為 claude.com/pricing。
5. **Line 2 (console.anthropic.com)** 與 Line 6 (`platform.claude.com/docs`) 皆為 Anthropic 長期資源（🔍 / ✅）；建議確認官方是否要遷移 console 網域到 claude.com。

### Overall Risk Assessment

**Risk level:** Low–Medium

API 規格的核心聲明（model IDs、deprecation dates 主體、SDK 名稱、Opus 4.7 突破性變更）基本全部正確，**只有 Haiku 3 退役差一天**是最嚴重的具體錯誤。其餘 3 條 ⚠️ 皆為「新 Opus 4.7 特性的 corner case 未被舊段落涵蓋」— 屬於文章主體寫於 Sonnet 4.6 世代、尚未完整吸收 4.7 新規範所致，整理一次即可。不影響 API 入門讀者的主流用法。

### Detection Coverage Note

本報告檢測到 **18 條硬性聲明**。以下未列入：

- 純 Python / TypeScript 範例程式碼
- 主觀建議（例：「絕對不要把 API Key 硬寫進程式碼」「設定合理 max_tokens」）
- `response.usage` 用法示範
- 串流事件類型表格（概念性說明，非版本敏感）
- Retry 範例 code block

後續讀者（ChatGPT / Gemini）若發現本報告漏掉的聲明，屬預期行為 — 本驗證不保證 100% 覆蓋。
