<!-- SPECTRA:START v1.0.1 -->

# Spectra Instructions

This project uses Spectra for Spec-Driven Development(SDD). Specs live in `openspec/specs/`, change proposals in `openspec/changes/`.

## Use `/spectra:*` skills when:

- A discussion needs structure before coding → `/spectra:discuss`
- User wants to plan, propose, or design a change → `/spectra:propose`
- Tasks are ready to implement → `/spectra:apply`
- There's an in-progress change to continue → `/spectra:ingest`
- User asks about specs or how something works → `/spectra:ask`
- Implementation is done → `/spectra:archive`

## Workflow

discuss? → propose → apply ⇄ ingest → archive

- `discuss` is optional — skip if requirements are clear
- Requirements change mid-work? Plan mode → `ingest` → resume `apply`

## Parked Changes

Changes can be parked（暫存）— temporarily moved out of `openspec/changes/`. Parked changes won't appear in `spectra list` but can be found with `spectra list --parked`. To restore: `spectra unpark <name>`. The `/spectra:apply` and `/spectra:ingest` skills handle parked changes automatically.

<!-- SPECTRA:END -->

## 驗證報告 `verify/`

2026-04-22 對 9 份文章（01_GENERAL、02_CHAT、03_COWORK、04_CODE、05_API、06_MCP、07_SKILL、08_MANAGED_AGENTS、appendix_claude_design_prompt）做了**逐條硬性聲明查證**。報告位於 `verify/`：

- `verify/SUMMARY.md` — 全局摘要與跨文件 Top 10 修正清單
- `verify/<filename>.md` — 各篇文章對應的驗證報告
- `verify/README.md` — 目錄總覽、四分類標準（✅/⚠️/❌/🔍）、archive 政策
- `verify/_template.md` — 未來續驗用的報告模板

**重要原則：**
- **2026-04-22 已套用 Top 10 + 跨文件通用修正**至原文（`0*_*.md`）；修正前原版備份於 `old/`。具體改動清單見 `verify/FIX_RECOMMENDATIONS.md`。
- 未處理段落（如 07_SKILL `.skill` 副檔名、06_MCP Claude.ai Connectors 命名等）見 `verify/FIX_RECOMMENDATIONS.md`「未處理段落」章節。
- 04_CODE 為 scoped 驗證，Chrome / Figma MCP / Hooks / Routines / Plugins / Desktop 段落待後續 session 補強。
- 驗證流程正式規格位於 `openspec/specs/article-verification/spec.md`。

讀者／ChatGPT / Gemini 要交叉比對事實時，請先讀 `verify/SUMMARY.md` 定位高風險段落。

