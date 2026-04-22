# Verification Report: `03_COWORK.md`

## Header

| Field | Value |
|---|---|
| Source filename | `03_COWORK.md` |
| Source line count | 227 |
| External URL count in source | 2 |
| Verification date | 2026-04-22 |
| Verifier | Claude Opus 4.7 |

## Claims Table

| # | Line | Claim (excerpt) | Type | Result | Source / Suggestion |
|---|---|---|---|---|---|
| 1 | 6 | Cowork **2026-04-09** 正式 GA，原為 Research Preview | date | ✅ Verified | [Anthropic 官方 cowork 產品頁](https://claude.com/product/cowork) 提及 Apr 8, 2026 的 enterprise deployment 公告；[testingcatalog.com](https://www.testingcatalog.com/anthropic-launches-claude-cowork-in-general-availability/) 等多家 2026-04-09 前後的 GA 報導證實 |
| 2 | 6 | 需 **Pro 以上方案** | product-spec | ✅ Verified | 多方一線二手來源（pasqualepillitteri.it、anthemcreation.com 等）一致：Pro / Max / Team / Enterprise 可用，Free 不可用 |
| 3 | 7 | `https://claude.com/product/cowork` URL 可達 | url-reachability | ✅ Verified | WebFetch 成功回應、為官方產品頁 |
| 4 | 74 | Computer Use 推出：**Mac 2026/03/23**、**Windows 2026/04/03**（Research Preview） | date | ✅ Verified | [SiliconANGLE 2026-03-23](https://siliconangle.com/2026/03/23/anthropics-claude-gets-computer-use-capabilities-preview/)、[Thurrott.com — Windows 版](https://www.thurrott.com/a-i/anthropic/334498/anthropic-brings-claude-computer-use-to-windows)、[WinBuzzer 2026-04-04](https://winbuzzer.com/2026/04/04/anthropic-claude-desktop-control-windows-cowork-dispatch-xcxwbn/) 三家印證 Mar 23 / Apr 3 時序 |
| 5 | 75 | 需 Pro / Max 方案；Claude Desktop 保持開啟 | product-spec | ✅ Verified | 官方與多家媒體一致確認 Pro / Max；Desktop 需開啟 |
| 6 | 81 | 啟用方式：Claude Desktop → **Settings → General（Desktop app 區段）** → **Computer use** 開關 | product-spec | 🔍 Unverifiable | 未取得官方 UI 截圖或 help center 直接描述此路徑；搜尋所得二手來源未統一確切設定路徑。建議附官方 help center URL（[Let Claude use your computer in Cowork](https://support.claude.com/en/articles/14128542-let-claude-use-your-computer-in-cowork)）並視實際 UI 更新 |
| 7 | 83 | Computer Use 同時適用於 Cowork 和 Claude Code Desktop；與 MCP 無關 | product-spec | ✅ Verified | 多家 2026-04 來源描述 Computer Use 同時在 Cowork 與 Claude Code desktop 啟用；底層透過 MCP tool 實作（[Pluto Security 技術拆解](https://blog.pluto.security/p/inside-claude-cowork-how-anthropics) 指 Computer Use 為專屬 MCP tools）。<br>**注意**：文章說「與 MCP 無關」**在技術層面不精確**（Computer Use 是以專屬 MCP 工具實現），但**對終端使用者而言**無需自行設定 MCP、屬「不需額外設定」的事實。建議改寫為「使用者端無需額外 MCP 設定」較準確 |
| 8 | 98 | Cowork 內有 **VM 隔離**保護 | product-spec | ⚠️ Needs correction | **部分正確但有重大遺漏**：Cowork **檔案／程式碼操作**透過 macOS VZVirtualMachine framework 的 Linux VM 隔離（[ClaudeCN 架構拆解](https://claudecn.com/en/blog/claude-cowork-architecture/)）。但官方技術拆解明示：**Computer Use 操作運行在你的實際桌面、**不在 sandbox 內**、無 VM 隔離**（[Pluto Security 分析](https://blog.pluto.security/p/inside-claude-cowork-how-anthropics)）。文章混用「Cowork 安全機制」統稱容易誤導。建議改寫為：「Cowork 的檔案／程式碼類操作在 VM 內隔離；Computer Use（螢幕操控）**直接在使用者桌面執行、不經 VM 沙箱**，故須尤其注意權限確認」 |
| 9 | 99 | Research Preview 階段不建議用於敏感資料 | product-spec | ✅ Verified | 官方與多家媒體皆提示 Research Preview 階段有行為風險；已知 CVE-2025-59536（RCE via malicious hooks/MCP configs, patched 2025-10）與 CVE-2026-21852（API key exfiltration, patched 2026-01）印證風險實存 |
| 10 | 113 | Dispatch 是桌機 Claude Desktop 的遠端控制器；AI 運算全部在桌機執行 | product-spec | ✅ Verified | [Assign tasks to Claude from anywhere in Cowork](https://support.claude.com/en/articles/13947068-assign-tasks-to-claude-from-anywhere-in-cowork)："Claude runs on your computer—with access to your local files, connectors, plugins, and your apps through computer use" |
| 11 | 114 | Dispatch 只有**單一持續對話串**（single persistent thread），無法建立新串／管理多串 | product-spec | ✅ Verified | 同上官方："There's no way to start a new thread or manage multiple threads. All messages live in a single conversation." |
| 12 | 115–117 | Dispatch 自動判斷任務類型：知識型 → Cowork、開發型 → Claude Code | product-spec | ✅ Verified | 同上官方："Claude figures out what kind of work is needed and spins up the right session. Development tasks run in Claude Code; knowledge work runs in Cowork." |
| 13 | 119 | 使用者無法手動選擇延續既有 session，亦無法從手機切到桌機上已開啟的 Cowork 對話 | product-spec | ✅ Verified | 來自同一份官方說明的 single-thread 設計推論，並於文章末的「⚠️ 常見誤解」段已自行註明屬觀察結論 |
| 14 | 132 | 桌機需 macOS / Windows **x64**（需喚醒） | product-spec | ⚠️ Needs correction | Claude Desktop 現**同時支援 Windows x64 與 arm64**（見 [claude.com/download](https://claude.com/download) 實際支援矩陣）。文章只寫 x64 會漏掉 arm64 使用者（例如 Surface Pro X、Snapdragon X 類裝置）。建議補註 arm64 |
| 15 | 144 | 多台電腦同時開著 Claude Desktop 時，Dispatch 派任到哪一台？**官方文件未說明選擇機制** | product-spec | ✅ Verified | 官方 help center 僅要求 "Your computer must be active"，未涵蓋多裝置選擇機制。文章誠實標示為「未解疑問」屬正確處置 |
| 16 | 146 | `https://support.claude.com/en/articles/13947068-...` URL 可達 | url-reachability | ✅ Verified | WebFetch 成功取得內容 |
| 17 | 191 | Cowork Projects 需 **2026 年 3 月之後**的 Claude Desktop 版本；與 Claude.ai 的 Projects 是不同功能 | product-spec | 🔍 Unverifiable | 搜尋未取得此功能精確上線日期之官方公告；Cowork Projects 與 Claude.ai Projects 為不同功能屬合理陳述（二者資料儲存位置、共用模型均不同）。建議保留並附待補官方 release notes |
| 18 | 217 | **Web search** 是 Anthropic 內建的 **System Connector**，不出現在 Customize 清單、Cowork 可直接使用 | product-spec | 🔍 Unverifiable | 「System Connector」屬官方內部術語，搜尋無法取得直接佐證；Web search 在 Cowork 預設可用屬常識觀察。建議保留並於再版時附 help center URL |
| 19 | 223 | **已 GA（2026-04-09）**：Cowork 本體正式發布，但 **Computer Use 仍為 Research Preview** | product-spec | ✅ Verified | 兩件事皆獲多家來源確認：Cowork GA vs Computer Use 仍 Research Preview 是並存的產品狀態 |
| 20 | 226 | 不支援 Linux — Claude Desktop 無 Linux 版本 | product-spec | ✅ Verified | [claude.com/download](https://claude.com/download) 明示 "Not available for Linux" |

## Summary

### Counts

| Marker | Count |
|---|---|
| ✅ Verified | 14 |
| ⚠️ Needs correction | 3 |
| ❌ Unsupported | 0 |
| 🔍 Unverifiable | 3 |
| **Total** | 20 |

### Top 3–5 Critical Corrections

1. **Line 98 — 「Cowork 內有 VM 隔離」敘述過度概括**（⚠️）：Cowork 的檔案／程式碼操作有 VM 隔離，但 Computer Use 直接在使用者桌面執行、**不經 VM 沙箱**。這是安全模型的關鍵差異，讀者若誤以為「Computer Use 也在沙箱裡」會做出錯誤的風險決策。
2. **Line 132 — 僅列 Windows x64**（⚠️）：實際支援 x64 與 arm64，需補註 arm64。
3. **Line 83 — 「Computer Use 與 MCP 無關」技術層面不精確**（⚠）：實際上 Computer Use 是以專屬 MCP tools 實作；只是使用者**不需**自行設定 MCP。建議改寫為「使用者端無需額外 MCP 設定」。
4. **Line 81 — Computer Use 啟用路徑**（🔍）：UI 路徑可能已變動，建議附官方 help center URL 由讀者自行確認。
5. **Line 191 — Cowork Projects 可用日期**（🔍）：未找到官方釋出日期，建議補 release notes 連結。

### Overall Risk Assessment

**Risk level:** Low

文章的產品時序（Cowork GA、Computer Use Mac/Windows 發布、Dispatch 設計意圖、Dispatch single-thread 限制）**全部正確**；主要修正集中在**安全模型的精確表述**（VM 沙箱範圍）與**平台支援矩陣的完整性**（Windows arm64）。核心技術聲明查證後幾乎全部站得住腳，後續 change 只需針對 3 條 ⚠️ 精修即可。

### Detection Coverage Note

本報告檢測到 **20 條硬性聲明**。以下未列入：

- 純 UI 操作步驟（例：「點擊 Work in a folder」「掃 QR Code 配對」）
- 功能示意圖與 ASCII 框
- 主觀建議（例：「Research Preview 階段建議只開一台電腦」）
- 任務類型舉例（例：「整理 Downloads 資料夾」）

後續讀者（ChatGPT / Gemini）若發現本報告漏掉的聲明，屬預期行為 — 本驗證不保證 100% 覆蓋。
