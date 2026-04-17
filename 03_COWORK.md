[← 上一篇：Chat](02_CHAT.md) | [下一篇：Code →](04_CODE.md)

# Claude Cowork

> **知識截止日期：** 2026 年 4 月  
> **狀態：** GA（2026-04-09 正式發布，原為 Research Preview；需 Pro 以上方案）  
> 官方介紹：[claude.com/product/cowork](https://claude.com/product/cowork)

---

## 什麼是 Cowork？

Cowork 是 **Claude Desktop 內建的自主 Agent 模式**，讓 Claude 能夠獨立操作你的本機應用程式和檔案，代替你完成完整的知識工作任務。

```
一般 Claude 對話：                  Cowork：
┌──────────────────────┐            ┌──────────────────────────────┐
│ 你 ⇄ Claude（來回）  │            │ 你：交派任務                  │
│ 手動複製貼上執行      │            │   ↓                          │
└──────────────────────┘            │ Claude：自主完成              │
                                    │  - 開啟 Excel                │
                                    │  - 讀取本機檔案               │
                                    │  - 操控瀏覽器                 │
                                    │  - 整理輸出結果               │
                                    │   ↓                          │
                                    │ 你：收到完成的成品             │
                                    └──────────────────────────────┘
```

---

## 啟用方式

1. 確認已安裝 **Claude Desktop**（需更新到最新版）
2. 開啟 Claude Desktop，從側欄或頂部 Tab 切換到 **Cowork 模式**
3. 點擊「Work in a folder」授予 Claude 存取本機資料夾的權限
4. 描述任務，Claude 開始自主執行

> 需要 **Pro 以上方案**（Free 方案目前不支援）

---

## 任務執行方式

Cowork 接到指令後會**自動拆解成多個步驟**依序執行，不需要你手動拆分：

```
你：「整理今天台灣新聞，存成 today-news.md」

Cowork 自動規劃：
  步驟 1：搜尋今日台灣新聞
  步驟 2：整理並分類新聞
  步驟 3：儲存新聞整理檔案
```

這是 Cowork 與 Chat 最本質的差異——Chat 是單一回合問答，Cowork 是自主規劃並執行的 Agent。

---

## 核心功能

### 本機檔案操作

Claude 可以直接讀寫你授權的資料夾內容：

```
「整理我的 Downloads 資料夾：
 依檔案類型分類、重新命名無意義的檔名、
 標記重複檔案、建立 Receipts / Work / Personal 子資料夾」
```

### Computer Use（電腦操控）

> **推出時間：** Mac 2026/03/23、Windows 2026/04/03（Research Preview）  
> **需求：** Pro / Max 方案，Claude Desktop 保持開啟

Claude 可以直接看著你的螢幕操控電腦，不需要 API 或連接器——就像人一樣點擊、輸入、捲動。

**啟用方式：**

Claude Desktop → **Settings → General（Desktop app 區段）** → **Computer use** 開關打開

> 啟用後同時適用於 Cowork 和 Claude Code Desktop，與 MCP 無關，不需要額外設定。

**可執行的操作：**
- 開啟應用程式、操作選單、點擊按鈕
- 在 Excel / PowerPoint / Chrome / Slack 等 App 內執行操作
- 瀏覽網頁、填表、擷取資料
- 截圖確認結果，自動修正錯誤

```
「幫我把這份 CSV 資料製作成 Excel 圖表，
 再貼到 PowerPoint 第 3 張投影片」
```

**使用時的安全機制：**
- 每次存取新 App 前，Claude 會先跳出**權限確認**，需你批准才能繼續
- Cowork 內有 **VM 隔離**保護
- Research Preview 階段：不建議用於敏感資料（財務、醫療、個人資料）

### Dispatch（跨裝置派任）

Dispatch 是 Cowork 的**遠端操控介面**——你在手機派任，Claude 在桌機執行，你不需要守在電腦前。

```
早上出門前 → 手機輸入任務
             → Claude 在桌機執行
             → 你回來看結果
```

**核心概念（重要）：**

- Dispatch 本身**不是**行動版 AI agent，而是桌機 Claude Desktop 的遠端控制器。手機只負責傳訊息、收結果，AI 運算全部在桌機執行
- Dispatch 只有**單一持續對話串**（single persistent thread），無法建立新對話串或管理多個對話串
- 下指令後，**Claude 自動判斷**要用哪種 session 類型：
  - 知識型任務 → 開 Cowork session
  - 開發型任務 → 開 Claude Code session
- 這些 session 會出現在桌機各自的側邊欄，你可以點進去看細節
- **使用者無法手動選擇**要延續哪個既有 session，也無法從手機切換到桌機上已經開啟的 Cowork 對話

**與桌機 Cowork 的差異：**

| | Cowork（桌機） | Dispatch（手機） |
|---|---|---|
| **互動方式** | 即時、同步 | 非同步 |
| **資料夾選擇** | 手動選擇 | 由 Claude 根據任務自動決定 |
| **對話延續性** | 可自由切換多個 session | 只有單一持續對話串 |
| **使用情境** | 你在電腦前監看 | 你不在電腦前 |

**使用前提：**

- 桌機必須安裝並**開啟**最新版 Claude Desktop（macOS / Windows x64）
- 桌機必須處於喚醒狀態
- 透過 QR Code 將手機與桌機配對

**使用步驟：**

1. 桌機開啟 Claude Desktop，在 Cowork 中啟用 Dispatch，掃 QR Code 配對手機
2. 手機 Claude App 中輸入任務指令
3. 桌機上的 Claude 自動開始執行，並在對應的 Cowork / Claude Code 側邊欄出現 session

> **⚠️ 常見誤解：** 無法把桌機上已經手動開啟的 Cowork 對話「切換到手機繼續指示」。Dispatch 走的是它自己的持續對話串，與桌機現有的 Cowork session 是分開的。

> **⚠️ 未解疑問：** 多台電腦同時開著 Claude Desktop 時，Dispatch 會派任到哪一台？官方文件未說明選擇機制。建議只開一台避免不確定行為。（待官方釐清）

> 官方文件：[Assign tasks to Claude from anywhere in Cowork](https://support.claude.com/en/articles/13947068-assign-tasks-to-claude-from-anywhere-in-cowork)

### 排程任務

設定定期自動執行的任務（類似 cron job，但用自然語言）：

```
「每天早上 9 點掃描我的收件匣，
 整理出需要我回覆的信件摘要」

「每週五下午 5 點產生本週工作摘要報告」
```

---

## 適合的任務類型

| 任務類型 | 範例 |
|----------|------|
| **文件整理** | 整理資料夾、重新命名、分類歸檔 |
| **資料處理** | CSV 分析、Excel 製表、資料清洗 |
| **簡報製作** | 從資料生成 PowerPoint 投影片 |
| **郵件處理** | 摘要收件匣、草擬回覆 |
| **網頁操作** | 填表、擷取資料、重複性瀏覽任務 |
| **跨 App 工作流** | 從 Slack 訊息整理成 Notion 文件 |

---

## 與 Claude Code 的差異

| | Cowork | Claude Code |
|---|---|---|
| **目標使用者** | 知識工作者（非工程師） | 工程師 |
| **操作對象** | 桌面 App、文件、瀏覽器 | 程式碼、終端機、版本控制 |
| **啟動方式** | Claude Desktop Cowork 分頁 | 終端機 `claude` 指令，或 Claude Desktop 的 Code 分頁（GUI IDE 模式） |
| **任務性質** | 一般辦公室工作流 | 開發工作流 |

兩者都是自主 Agent，但操作的領域不同。工程師可以**兩個都用**：Cowork 處理非程式相關的工作，Claude Code 處理開發任務。

---

## Cowork Projects

Cowork Projects 是把任務、資料夾、設定**打包成一個持久工作區**，適合長期或反覆執行的工作。

> 需要 2026 年 3 月之後的 Claude Desktop 版本，與 Claude.ai 的 Projects 是**不同功能**。

官方描述：
> *A dedicated place for ongoing work, where context builds over time. Files and instructions stay in a folder on your computer.*

### 和 Claude.ai Projects 的差異

| | Claude.ai Projects | Cowork Projects |
|---|---|---|
| **儲存位置** | 雲端 | 本機（不同步雲端） |
| **用途** | 跨對話保留上下文 | 組織自主任務的工作區 |
| **內容** | 指令、上傳檔案、對話記憶 | 本機資料夾、指令、排程任務、記憶 |
| **共用** | 可（Team 方案） | 不行（本機限定） |

### 適合使用情境

- **例行性任務**：每週報告、每日郵件整理等反覆執行的工作流
- **長期專案**：需要 Claude 記住背景、偏好、資料夾位置的持續性工作
- **多任務管理**：不同性質的工作分開用不同 Project 管理，設定互不干擾

---

## Skills 與 Connectors

Cowork 支援 Skills、Connectors、Plugins（同 Chat / Claude Code），透過 Customize 按鈕管理。

**Web search** 是 Anthropic 內建的 System Connector，**不會出現在 Customize 清單中**，Cowork 可直接使用，無需手動安裝。

---

## 注意事項

- **已 GA**（2026-04-09）：Cowork 本體已正式發布，但 Computer Use 仍為 Research Preview
- **授權範圍**：只能操作你明確授予的資料夾，Claude 不會主動存取未授權的位置
- **Computer Use 風險**：執行前 Claude 會說明計畫，建議確認後再允許執行
- **不支援 Linux**：Claude Desktop 目前不提供 Linux 版本
