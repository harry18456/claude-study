# verify/ — Article Verification Reports

此目錄存放 9 份文章的驗證報告，一文一報告，檔名一一對應原文。

## Scope

本目錄**僅存放驗證報告**，不存放規格（spec）、討論（discussion）或建議改稿。正式驗證流程規格位於 `openspec/specs/article-verification/spec.md`。

## File Layout

```
verify/
├── README.md                              （本檔）
├── _template.md                           報告模板（新報告請從此複製）
├── 01_GENERAL.md
├── 02_CHAT.md
├── 03_COWORK.md
├── 04_CODE.md
├── 05_API.md
├── 06_MCP.md
├── 07_SKILL.md
├── 08_MANAGED_AGENTS.md
└── appendix_claude_design_prompt.md
```

## Processing Order

依設計決策「由簡到繁」，固定處理順序為：

1. `01_GENERAL.md`
2. `07_SKILL.md`
3. `02_CHAT.md`
4. `06_MCP.md`
5. `03_COWORK.md`
6. `05_API.md`
7. `08_MANAGED_AGENTS.md`
8. `appendix_claude_design_prompt.md`
9. `04_CODE.md`

每份報告必須完成並儲存後才能開始下一份。中斷後恢復時，仍依此順序往下，不可跳到任意一份。

## Do Not Modify Source Articles

本驗證流程**禁止修改**任何 `0[1-8]_*.md` 或 `appendix_*.md` 原文。
查無根據的聲明只能在報告中給出建議（delete / rewrite / retain with note），不得直接編輯原文。

## Reading the Reports

每份報告結構固定為三段：

- **Header** — 原文檔名、行數、外部連結數、驗證日期、驗證者。
- **Claims Table** — 以表格列出每條硬性聲明（含行號、摘錄、類型、驗證結果、來源 / 建議）。
- **Summary** — 四分類計數、3–5 條最關鍵修正、整體風險評估（Low / Medium / High）。

### Four-class Result Markers

| 標記 | 意義 |
|---|---|
| ✅ Verified | 查到可信來源，原文正確 |
| ⚠️ Needs correction | 有來源但原文有誤 |
| ❌ Unsupported | 查無可信支撐 |
| 🔍 Unverifiable | 主觀／非公開／需內部資訊 |

## Archive Policy

`verify/` 目錄**不隨 change 歸檔**——報告是持續參考資料，非規格本身。歸檔時 `openspec/changes/verify-articles-2026-04/` 會被移到 `archive/`，但 `verify/` 保留在 repo 根部。

## Version

- 首次驗證完成日期：2026-04-22（進行中）
- 驗證者：Claude Opus 4.7
- 本次驗證基於原文 2026-04-22 版本。若原文日後修改，請重新驗證。
