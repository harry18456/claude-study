## Context

本專案為 Claude 全家桶使用指南（9 份 markdown 文章、約 5,300 行），由作者 AI 以 2026-01 知識截止日生成，但大量內容日期落在 2026-02 到 2026-04-22 之間。差集部分有**用模型記憶合理推論**而非查證的高風險，具體集中在：

- **benchmark 數字**（如 Opus 4.7 SWE-bench Verified 87.6%、CursorBench 70%、Mythos 的 Terminal-Bench 82.0%）
- **版本號**（如 Claude Code v2.1.113、v2.1.114、Claude Desktop v1.2581.0）
- **事件時序**（如 npm source map 外洩 2026-03-30~31、GitHub Copilot 2026-04-20 暫停新訂閱、Anthropic Pro 2% 試點 2026-04-21）
- **定價**（如 Managed Agents $0.08/session-hour、Web search $10/1,000 次）
- **企業案例**（Notion / Rakuten / Asana / Vibecode / Sentry 的 Managed Agents 成效）
- **API 規格**（`agent_toolset_20260401`、`managed-agents-2026-04-01` beta header、`experimental-cc-routine-2026-04-01`）

現況連結密度不均：04_CODE 44 條、08_MANAGED_AGENTS 11 條，但 07_SKILL 0 條、02_CHAT 1 條、06_MCP 1 條、03_COWORK 2 條——**最終讀者為 ChatGPT / Gemini 進階模型且會上網查證**，無連結的硬性聲明會立即被標記為可疑。

## Goals / Non-Goals

**Goals:**

- 為 9 份文章各產出一份結構一致的驗證報告，列出所有硬性聲明與其查證結果。
- 報告要**機器可讀、可對照行號回溯**，使 ChatGPT / Gemini 後續檢視時能快速定位。
- 給每條「需修正」或「查無根據」的聲明附上**可操作建議**（改寫範例、刪除、降級為較不斷言的版本）。
- 過程產生的外部連結，必須是**首選官方、次選一線媒體、再次社群**的分層可信來源。

**Non-Goals:**

- **不改原文**：本 change 不編輯 `0*_*.md` 與 `appendix_*.md`。最終處置由後續 change 執行。
- **不重寫章節結構**：排版、小節順序、語氣、中譯措辭皆不在審查範圍內。
- **不評論主觀品質**：不做寫作風格評論、不批判取材角度。
- **不做完整性審查**：只檢查「已寫出的聲明是否有根據」，不檢查「是否漏寫該寫的聲明」。
- **不保證 100% 覆蓋**：模糊敘述（例如「大約」「常見」）、純流程描述（例如操作步驟 1/2/3）不列為硬性聲明。

## Decisions

### 驗證報告檔案位置與命名

所有報告放在 `verify/` 目錄下，檔名直接對應原文：

```
verify/
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

**理由：** `verify/` 與 `openspec/` 平行且獨立於文章本身，避免污染原內容；檔名一一對應方便肉眼比對。**替代方案（已排除）：** 放進 `openspec/changes/verify-articles-2026-04/` 目錄——但這會在 archive 時被帶走，使後續查閱困難，故排除。

### 報告格式：統一欄位與四分類

每份報告分三段：

1. **Header** — 原文檔名、總行數、外部連結數、驗證日期、驗證者（Claude + 模型版本）。
2. **聲明清單表** — 欄位如下：

   | # | 行號 | 聲明（摘錄） | 類型 | 驗證結果 | 來源 / 修正建議 |
   |---|---|---|---|---|---|

3. **Summary** — ✅/⚠️/❌/🔍 各計數、最關鍵的 3–5 項修正、對原文的整體風險評估（低/中/高）。

**類型列舉**：日期、版本號、benchmark、定價、URL 可達性、事件時序、引用、產品規格、統計、其他。

**四分類定義：**

| 標記 | 意義 | 必要欄位內容 |
|---|---|---|
| ✅ 已驗證 | 查到可信來源，原文正確 | 來源 URL；若文章未附則註明「建議補連結」 |
| ⚠️ 需修正 | 有來源但原文有誤 | 指出誤差（如數字、日期、名稱），並給正確版本 + 來源 |
| ❌ 查無根據 | 查遍無可信支撐 | 說明查過哪些來源、建議刪除或改寫為無斷言版本 |
| 🔍 無法驗證 | 主觀 / 非公開 / 需內部資訊 | 說明為何無法查證（例：Anthropic 內部路線圖） |

**理由：** 四分類足以覆蓋所有驗證結果，避免作者面對「部分正確」之類的模糊標記。**替代方案（已排除）：** 三分類（只有 ✅/❌/🔍）——但會把「部分錯」併入「查無根據」，失去可修正性資訊。

### 來源可信度優先序

查證時依序嘗試以下來源，**優先採用排序靠前者**：

1. **官方一手來源**：`anthropic.com`、`claude.com`、`code.claude.com`、`platform.claude.com`、`modelcontextprotocol.io`、Anthropic 官方 GitHub repo、Anthropic 官方部落格/工程部落格、官方 YouTube。
2. **官方附屬文件**：Anthropic Help Center（`support.claude.com`）、官方發布的 changelog 與 API reference。
3. **一線媒體報導**（僅引述 Anthropic 官方發言或 benchmark 發表時採用）：TechCrunch、The Verge、The Register、iThome、TheNewStack、Bloomberg、WSJ。
4. **社群 / 個人網誌**：Hacker News 討論串、知名個人部落格（例：Simon Willison、wheresyoured.at）——**僅作為補充證據，不得作為唯一來源**。

**刻意排除**：推文、Reddit 貼文、未具名匿名論壇——除非能連到一手出處。

**理由：** ChatGPT / Gemini 高階模型會審查來源層級，單靠社群二手來源難過審。

### 查無根據時的處置：不刪、只建議

報告中「❌ 查無根據」的聲明，**不在本 change 中刪除原文**。報告只給出三選建議讓作者決定：

- **(a) 刪除**：整條聲明移除。
- **(b) 改寫為較不斷言**：例如把「SWE-bench Verified 87.6%」改為「在多項 SWE 相關基準測試取得 SOTA 等級表現」。
- **(c) 保留但加附註**：原文保留但加 `> 註：本數字來源待補`。

**理由：** 作者是文章主人，驗證者不該越俎代庖做內容決定。

### 執行順序：由簡到繁

依以下順序逐份處理，每份完成後建立 checkpoint，避免中途被迫重來：

1. `01_GENERAL.md` — 入口、benchmark 重災區，建立流程 template。
2. `07_SKILL.md` — 連結數 0，急需補；內容穩定、容易驗。
3. `02_CHAT.md` — 連結數 1，Memory / 無痕模式規則。
4. `06_MCP.md` — 連結數 1，MCP server 清單 + 官方 URL。
5. `03_COWORK.md` — Computer Use 日期與平台差異。
6. `05_API.md` — Opus 4.7 遷移規則、模型 deprecated 日期。
7. `08_MANAGED_AGENTS.md` — 企業案例、定價、API header。
8. `appendix_claude_design_prompt.md` — 兩個來源 GitHub repo 存在性。
9. `04_CODE.md` — 最大、最多外部連結（44 條）、事件時序最複雜，最後收尾。

**理由：** 01 建立方法論、07 補零連結降最大風險、04 留到最後由已累積的經驗處理。

### WebFetch / WebSearch 使用策略

- **URL 可達性**：使用 `WebFetch` 直連既有連結，驗證 HTTP 回應與頁面主題是否符合標示。
- **事實查證**：使用 `WebSearch` 以 `官方名稱 + 具體數字/日期/版本號` 為查詢關鍵字；找到候選後再用 `WebFetch` 讀原文確認。
- **語言**：查詢語種以英文為主（官方來源多為英文）；補充資料才用中文查詢。
- **副作用最小化**：同一 URL 不重複 fetch；若結果 token 過長則只摘錄關鍵段。

### 對 Spectra archive 的考量

驗證報告屬於**工作產出而非 spec 本身**，archive 本 change 時：

- `verify/` 目錄**留在 repo 根部**，不隨 change 被歸檔移走。
- `openspec/specs/article-verification/spec.md` 紀錄「驗證流程」的正式規格，方便未來做第二輪驗證時沿用。

## Risks / Trade-offs

- **[風險] 本 AI 自己是資訊污染源**：驗證者與撰寫者是同一個模型家族，對同一個事實可能做出同樣「合理推論」的誤判，導致**查證時自我肯定**。
  → **緩解**：硬性要求「查無實體可信來源即標為 ❌」，絕不以「模型認為合理」為通過條件。

- **[風險] 網路查證結果亦可能包含二手錯誤**：部分媒體報導可能誤傳 benchmark 或定價。
  → **緩解**：當官方與媒體數字衝突時，以官方為準；衝突本身也要在報告中註記。

- **[風險] URL 可能暫時不可達**：查證當下 500 / 404 ≠ 該連結是偽造的。
  → **緩解**：HTTP 錯誤要區分「伺服器暫時錯誤」與「頁面確實不存在」；必要時試 Web Archive 快照。

- **[風險] 9 份文章個別工作量差異大**：04_CODE 可能需要拆成多個 session。
  → **緩解**：`tasks.md` 為每份文章建立獨立 task，失敗時可單獨重跑；04_CODE 允許進一步分章處理。

- **[風險] ChatGPT / Gemini 後續審查可能標出本 AI 漏掉的聲明**：本次驗證不保證 100% 覆蓋硬性聲明。
  → **緩解**：在 summary 中明示「檢測到 N 條硬性聲明」，讓後續審查者知道驗證者的捕獲範圍，不誤以為 ✅ 以外沒有其他問題。

- **[Trade-off] 驗證報告會佔用大量 session 資源**：9 份 × 數十條聲明 × 每條 1~2 次 fetch。
  → **取捨**：接受此成本，因為讀者體驗與文件可信度優先；必要時允許在每份文章完成後 compact context。

## Migration Plan

本 change 僅新增檔案、不動既有檔案，無 migration 需求。

- **Deploy**：建立 `verify/` 目錄與 9 份報告 + `openspec/specs/article-verification/spec.md`。
- **Rollback**：刪除 `verify/` 目錄與新增的 spec 檔即可；原文章完全未動。

## Open Questions

- **是否要驗證 appendix 的中譯正確性？** 當前 Non-Goals 表示不做主觀品質審查，但「中譯是否忠於英文原文」屬於事實性判斷。建議：**僅在中譯與英文原文出現明顯語意偏離時列入報告**，純修辭差異不算。
- **第二輪驗證的觸發條件？** 若文章 6 個月後要重新發布，是否自動觸發重新驗證？暫不在本 change 決定，由作者之後視需求提出。
