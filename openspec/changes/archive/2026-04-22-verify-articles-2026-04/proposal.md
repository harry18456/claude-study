## Why

目前 9 份文章（01_GENERAL、02_CHAT、03_COWORK、04_CODE、05_API、06_MCP、07_SKILL、08_MANAGED_AGENTS、appendix_claude_design_prompt）有相當比例的內容日期落在 **2026-02 到 2026-04-22** 之間，這段時間超出作者 AI 的真實知識截止，許多聲明（benchmark 數字、版本號、事件時序、定價、企業案例）可能是憑模型記憶「合理推論」寫的，而非查證結果——這是最高的誤導讀者風險來源。

此外，部分篇章（尤其 **07_SKILL 連結數為 0**、02_CHAT 只有 1 條）幾乎沒有外部連結佐證。最終讀者 ChatGPT / Gemini 會上網交叉比對，若遇到無來源的硬性聲明便難以驗證，降低整份文件的可信度。

## What Changes

- **新增**：為 9 份文章各產出一份獨立的 **驗證報告** `verify/{filename}.md`，逐條列出硬性聲明、分類其驗證結果、附上可信來源連結或修正建議。
- **不動**：原文章檔案在本 change 內**完全不修改**——最終處置（補連結、改寫、刪除）由作者看完報告後在後續 change 決定。
- **驗證流程統一**：所有報告採相同欄位（行號、原文摘錄、類型、驗證結果、來源/建議）與相同四分類標準（✅ 已驗證、⚠️ 需修正、❌ 查無根據、🔍 無法驗證）。
- **執行順序**：01 → 07 → 02 → 06 → 03 → 05 → 08 → appendix → 04（從入口與低連結數開始，04_CODE 最後收尾）。

## Non-Goals (optional)

- **不修改原文**：本 change 只產報告、不動 `0*_*.md` 與 `appendix_*.md`。
- **不重寫文章結構**：不調整章節、不補新章節、不調整順序。
- **不做主觀評價**：報告只檢查「事實可否驗證」，不評論寫作風格、排版、語氣。
- **不追溯已歸檔事實**：與本文件主題無關的歷史資訊（例如 Claude 1.x 時代）不在驗證範圍內。

## Capabilities

### New Capabilities

- `article-verification`: 驗證報告的產出流程、欄位定義、四分類標準、來源可信度優先序、執行順序。

### Modified Capabilities

（無——本 change 只建立驗證報告，不修改既有文章的內容規格）

## Impact

- **Affected specs**: 新增 `openspec/specs/article-verification/spec.md`
- **Affected code / files**:
  - 新增目錄 `verify/` 及其下 9 份報告
  - 不異動既有 `*.md` 文章
  - 不異動既有 7 份 specs
