# Verification Report: `02_CHAT.md`

## Header

| Field | Value |
|---|---|
| Source filename | `02_CHAT.md` |
| Source line count | 270 |
| External URL count in source | 1 |
| Verification date | 2026-04-22 |
| Verifier | Claude Opus 4.7 |

## Claims Table

| # | Line | Claim (excerpt) | Type | Result | Source / Suggestion |
|---|---|---|---|---|---|
| 1 | 98 | Memory 每 **24 小時**更新一次 | product-spec | ✅ Verified | [Use Claude's chat search and memory（官方 help center）](https://support.claude.com/en/articles/11817273-use-claude-s-chat-search-and-memory-to-build-on-previous-context)："This synthesis is updated every 24 hours." |
| 2 | 98 | 不會記錄無痕模式的對話（Memory 不讀取不寫入） | product-spec | ✅ Verified | 同官方文件："Claude won't remember your chats, so they won't be saved to Claude's memory"。另見 [Using incognito chats](https://support.claude.com/en/articles/12260368-using-incognito-chats) |
| 3 | 104–105 | 兩種記憶空間：**Memory Summary**（跨所有非 Project 對話）+ **Project Memory**（各 Project 獨立）；皆每 24 小時更新 | product-spec | ✅ Verified | 官方 help center 定義 Memory summary 與 Project memory 為兩種空間；更新頻率一致 |
| 4 | 111–112 | Pause Memory：保留現有記憶但暫停使用；Reset Memory：**永久刪除所有記憶（不可復原）** | product-spec | ✅ Verified | 官方："**Pause memory**: Claude keeps its existing memory but won't use memory or make new memories. **Reset memory**: Permanently deletes all memories including project memories. Once you select this option... this cannot be undone." |
| 5 | 114 | 組織管理員可在 **Organization Settings → Capabilities** 全域停用 Memory；停用將**永久刪除**所有用戶記憶 | product-spec | ✅ Verified | 官方："Owners can disable the memory summary feature for their entire organization by navigating to Organization settings > Capabilities... Disabling Claude's memory at the organization level will automatically and permanently delete all memory data for all users in your organization." |
| 6 | 114 | Team 方案則由各成員**自行管理** | product-spec | 🔍 Unverifiable | 官方文件未見明確提及 Team 方案 memory 由成員自管 vs 由 owner 統管之差異；Organization settings 的描述涵蓋「enterprise」身份但 Team 方案是否同樣列在 owner 控制範圍，未明確。建議改寫為「Team 與 Enterprise 具 owner 權限者可在 Organization Settings → Capabilities 控制；無權限的一般成員於個人設定頁面操作」並附待補來源 |
| 7 | 116 | `https://support.claude.com/en/articles/11817273-...` URL 可達並為官方 help center 文件 | url-reachability | ✅ Verified | WebFetch 成功取得內容，頁面主題為 "chat search and memory" |
| 8 | 124 | 無痕模式快捷鍵 `Ctrl + Shift + I` | product-spec | ⚠️ Needs correction | Windows / Linux 為 **Ctrl + Shift + I**，**Mac 為 ⌘ Cmd + Shift + I**。文章僅列 Ctrl，對 Mac 使用者會誤導。建議加入平台變體：「`Ctrl + Shift + I`（Windows/Linux）／`⌘ + Shift + I`（macOS）」。來源：[Using incognito chats](https://support.claude.com/en/articles/12260368-using-incognito-chats) |
| 9 | 129 | 無痕模式**只在 Project 外**的對話可用（Project 內不顯示幽靈圖示） | product-spec | ✅ Verified | 官方："Incognito mode is currently only available for chats outside of projects, so you will not see the ghost icon when starting a chat within a project." |
| 10 | 130 | 無痕模式開始後**無法轉為一般對話或儲存紀錄** | product-spec | ✅ Verified | 官方："once closed, incognito chats cannot be reopened"；不會被儲存到歷史紀錄 |
| 11 | 165 | 單次上傳上限：**~20 MB** | product-spec | ⚠️ Needs correction | 官方上限為 **30 MB per file**（[Uploading files to Claude](https://support.claude.com/en/articles/8241126-uploading-files-to-claude)）；20 MB 可能為舊版或保守預估。建議更新為「30 MB per 檔」 |
| 12 | 166 | 單次對話最多上傳 **5 個檔案** | product-spec | ⚠️ Needs correction | 官方：Claude Chat 允許 **20 個檔案 per conversation**；Claude Projects 則檔案數無上限。「5 個」為錯誤。建議更新為「Chat 對話 20 檔／Project 不限」 |
| 13 | 167 | 無法執行上傳的程式碼（執行功能限於 Artifacts） | product-spec | 🔍 Unverifiable | 官方 help center 未針對「上傳的 .py/.js 不會實際執行」做明示聲明；此敘述為操作型事實但缺少正式文件佐證。建議保留並加「執行權在 Artifacts 與 Code Interpreter tool；上傳檔案本身為 context 而非執行目標」 |
| 14 | 183 | Memory 是 Skill 的一種（與 Web search 列於 Skills 範例） | product-spec | ⚠️ Needs correction | Memory 是 **Capability（可於 Settings → Capabilities 啟用/停用）**，**不是 Skill**；官方 help center 與 Claude Code skills docs 皆將兩者視為獨立概念（Memory 無 `SKILL.md` 定義）。建議將 Memory 從 Skills 範例中移除，改列於 Capabilities 段或另立說明 |
| 15 | 212–214 | 介面支援矩陣：Chat（Skills ✓、Connectors ✓、Plugins ✗）／Cowork（三者皆 ✓，付費方案）／Claude Code（三者皆 ✓） | product-spec | ✅ Verified | [Use plugins in Claude Cowork](https://support.claude.com/en/articles/13837440-use-plugins-in-claude-cowork)："Plugins are available to all Claude Cowork users on paid plans (Pro, Max, Team, Enterprise)"；[claude.com/plugins](https://claude.com/plugins) 明示 plugins 為 Cowork 與 Code 設計；Chat 介面未支援 Plugins |
| 16 | 240 | Chat Search 適用 **Pro / Max / Team / Enterprise** | product-spec | ✅ Verified | 官方 help center："available to users on paid plans (Pro, Max, Team, and Enterprise plans)" |
| 17 | 245 | 使用 RAG、附引用連結；範圍涵蓋**所有非 Project 對話**；Project 內搜尋**僅限該 Project** | product-spec | ✅ Verified | 官方："These searches use Retrieval-Augmented Generation (RAG)... you'll see citations linking back to the original chats... All chats outside of projects... Individual project conversations (searches are limited to within each specific project)" |
| 18 | 247 | 停用：Settings → Capabilities → 關閉「Search and reference chats」 | product-spec | ✅ Verified | 官方："navigate to Settings > Capabilities... Switch the toggle next to 'Search and reference chats' off" |
| 19 | 270 | 分享後的連結可被搜尋引擎索引 | product-spec | 🔍 Unverifiable | Claude help center 關於 share link 的官方文件未明確聲稱「可被搜尋引擎索引」；此聲明可能屬安全觀點推論（公開連結通常會被索引），但缺少官方可援引之說明。建議改寫為較不斷言版本（「公開連結的內容不受 robots.txt 或登入保護」）或附待補來源 |

## Summary

### Counts

| Marker | Count |
|---|---|
| ✅ Verified | 12 |
| ⚠️ Needs correction | 4 |
| ❌ Unsupported | 0 |
| 🔍 Unverifiable | 3 |
| **Total** | 19 |

### Top 3–5 Critical Corrections

1. **Line 166 — 單次對話 5 檔案上限錯誤**（⚠️）：官方為 20 檔，文章寫 5 檔會讓讀者誤判產品能力。
2. **Line 165 — 20 MB 檔案大小錯誤**（⚠️）：官方為 30 MB per file，建議更新。
3. **Line 124 — 無痕模式快捷鍵缺 Mac 版本**（⚠️）：Mac 用 ⌘ + Shift + I，文章只寫 Ctrl 版，易誤導多作業系統讀者。
4. **Line 183 — Memory 歸類為 Skill 範例**（⚠️）：Memory 是 Capability、不是 Skill；與 07_SKILL.md 的定義會互相矛盾。
5. **Line 114 — Team 方案的 Memory 管理歸屬**（🔍）：建議補「owner 權限」而非「成員自行管理」之對比。

### Overall Risk Assessment

**Risk level:** Low

文章核心概念（Memory 週期、無痕模式、Chat Search、Project）皆與官方一致；主要修正集中在**具體數字（20 vs 30 MB、5 vs 20 檔）**與**分類用語（Memory 是 Capability 非 Skill）**。結構與行為描述正確，屬次要修補，不影響整體可信度。

### Detection Coverage Note

本報告檢測到 **19 條硬性聲明**。以下未列入：

- 純操作步驟（例：New Project 建立流程、點擊 Share）
- 主觀建議（例：「對於嘗試不同提示策略非常實用」）
- Project Instructions 撰寫範例程式碼
- UI 示意（ASCII 方框）

後續讀者（ChatGPT / Gemini）若發現本報告漏掉的聲明，屬預期行為 — 本驗證不保證 100% 覆蓋。
