[下一篇：Chat →](02_CHAT.md)

# Claude 概覽

> **知識截止日期：** 2026 年 4 月（部分功能資訊來自官方文件與社群查詢）

---

## 什麼是 Claude？

Claude 是 Anthropic 開發的 AI 助理，定位為「有用、無害、誠實」的大型語言模型（LLM）。與其他 AI 工具相比，Claude 特別強調：

- **長上下文處理**：支援超長對話與大型文件分析
- **程式碼能力**：理解、生成、重構、審查各類程式語言
- **推理品質**：複雜邏輯分析、技術決策輔助
- **安全導向**：Anthropic 的 Constitutional AI 訓練方法

---

## 模型家族

Claude 提供三個等級的模型，分別針對不同的速度與能力需求：

| 模型 | 代表 ID | 定位 | 適用情境 |
|------|---------|------|---------|
| **Haiku** | `claude-haiku-4-5` | 輕量、快速、低成本 | 高頻呼叫的 Agent worker、簡單問答、程式碼補全 |
| **Sonnet** | `claude-sonnet-4-6` | 平衡性能與成本 | 主要開發工作、複雜程式任務、日常工程使用 |
| **Opus** | `claude-opus-4-7` | 最強推理與 agentic coding | 架構設計決策、深度研究、最複雜的分析與多步驟工作流 |

> **Opus 4.7（2026-04-16 發布）重點變化：**
> - **Adaptive thinking** 取代 Extended thinking——4.7 不再支援 Extended thinking API
> - 新增 **`xhigh`** effort level（介於 `high` 與 `max` 之間，官方建議 coding / agentic 預設用 `xhigh`）
> - **新 tokenizer**：同樣 1M token context，但壓縮比不同（~555k 字 vs 4.6 的 ~750k 字），單次能塞的原文字數**變少**
> - Vision 解析度 3 倍提升（最長邊 2,576 px，~3.75 MP）
> - 首次內建來自 Mythos / Project Glasswing 的**自動偵測 / 阻擋禁止資安行為**
> - 定價與 4.6 相同（$5 / $25 per MTok），4.6 仍可用但已列為 legacy
> - Model ID：`claude-opus-4-7`（AWS Bedrock 為研究預覽）
>
> **Opus 4.7 vs 4.6 Benchmark 對照：**
>
> | Benchmark | Opus 4.7 | Opus 4.6 | 競品參考 |
> |---|---|---|---|
> | SWE-bench Verified | **87.6%** | 80.8% | Gemini 3.1 Pro 80.6% |
> | SWE-bench Pro | **64.3%** | 53.4% | GPT-5.4 57.7% |
> | CursorBench（真實開發情境） | **70%** | 58% | — |
> | GPQA Diamond（博士級推理） | **94.2%** | — | GPT-5.4 Pro 94.4%, Gemini 3.1 Pro 94.3% |
> | Terminal-Bench 2.0 | **69.4%** | — | Mythos 82.0% |
> | Finance Agent | **64.4%**（SOTA） | — | — |
> | MCP-Atlas（scaled tool use） | **77.3%** | — | — |
> | BrowseComp（web 研究） | 79.3% | — | GPT-5.4 Pro 89.3% |
>
> 整體表現：比 4.6 在多步驟 agentic workflow **+14%**，tool error 降到 **1/3**。

**選擇原則：**
- 預設使用 **Sonnet** — 90% 的情況下最佳性價比
- 需要快速大量呼叫（如 multi-agent worker）→ **Haiku**
- 需要最深度的推理（如架構評估）→ **Opus**

### Mythos Preview（2026 年 4 月 7 日發表）

Anthropic 最新發表的資安專用模型，**不屬於上述三個等級**，定位為獨立的特殊用途模型。

**核心能力：**
- 自主發現數千個橫跨作業系統、瀏覽器及開源軟體的**零日漏洞**
- 包括 OpenBSD 存在 27 年、FFmpeg 存在 16 年（經 500 萬次自動測試未被發現）的漏洞
- 能串連多個漏洞進行複合攻擊鏈，並生成漏洞利用程式
- 與 Opus 4.6 對比：Opus 數百次測試僅成功 2 次，Mythos 成功 181 次

**Project Glasswing（全球資安防禦計畫）：**
- 合作夥伴：AWS、Apple、Google、Microsoft、Nvidia、Cisco 等，涵蓋 40+ 關鍵基礎設施組織
- 投資規模：最高 1 億美元使用額度，另捐贈 400 萬美元給開源資安組織

**定價：** Input $25 / Output $125（每百萬 token），遠高於其他模型

> 來源：[iThome 報導（2026/04/08）](https://www.ithome.com.tw/news/174898)

---

## 存取方式

Claude 有四種主要存取管道，對工程師來說定位各不相同：

| 存取方式 | 工具 | 平台 | 說明 |
|----------|------|------|------|
| **Claude.ai** | 網頁瀏覽器 | 全平台 | 對話介面，支援檔案上傳、Artifacts、Projects |
| **Claude Desktop** | 桌面應用程式 | macOS / Windows | 整合 Cowork、Code 分頁（IDE 模式，多 session 並行）、Computer Use、MCP |
| **Claude Code CLI** | 終端機工具 | 全平台 | 直接整合到開發環境，可操作本機檔案與程式碼 |
| **Claude for Microsoft 365** | Office Add-in | macOS / Windows | 在 Word、Excel、PowerPoint 內以側邊欄形式使用 Claude |
| **Claude API** | HTTP API / SDK | — | 程式化串接，用於建立 AI 應用或自動化流程 |

### Claude Desktop 特色功能

Claude Desktop 不只是 Claude.ai 的桌面版，它有幾個網頁版沒有的獨特功能：

- **Cowork**：自主 Agent 模式，可直接操作本機的 Excel、PowerPoint、Chrome、Slack 等 App，執行多步驟知識工作（整理文件、分析資料、自動化作業）
- **Claude Code（Code 分頁）**：2026-04 大改版後的內建 IDE 模式——多 session 並排、拖拉自訂面板、內建 terminal / 檔案編輯 / Diff / Preview / Tasks，每個 session 自動用 Git worktree 隔離，Side Chat（`⌘+;`）分流問題不污染主對話。詳見 [04_CODE.md](04_CODE.md)
- **Computer Use**（Pro/Max）：讓 Claude 控制你的螢幕——點擊、輸入、導覽——無需額外設定
- **本機 MCP 整合**：一鍵安裝 `.mcpb` 擴充包，直接連接本機資料庫、檔案系統等工具
- **Dispatch**：從手機派發任務給 Desktop 的 session，並行處理多個工作
- **跨裝置同步**：桌機開始的對話，手機和網頁版可無縫接續

> **Remote Control 補充**：從手機遠端控制**終端機裡的 Claude Code CLI** 是另一個功能（`claude remote-control` 指令），不需要安裝 Desktop App，詳見 04_CODE.md。

### Claude for Microsoft 365（2026 年 4 月 10 日 Public Beta）

Claude 以 Office Add-in 形式直接嵌入 Word、Excel、PowerPoint 的側邊欄，目前為 Team / Enterprise 方案的 Public Beta：

- 在 Word 中撰寫、編輯、修改文件，所有 AI 修改以**追蹤修訂（Tracked Changes）**呈現
- 跨 Word、Excel、PowerPoint 可在同一個對話中操作
- 另外，Microsoft 365 Copilot 也已支援選用 Claude 作為替代模型

> 入口：[pivot.claude.ai](https://pivot.claude.ai/)

> 下載：[claude.com/download](https://claude.com/download)（支援 macOS、Windows x64/arm64、ChromeOS，不支援 Linux）

```
工程師使用 Claude 的四個入口：

┌───────────┐  ┌────────────────┐  ┌─────────────────┐  ┌─────────────┐
│ Claude.ai │  │ Claude Desktop │  │ Claude Code CLI │  │ Claude API  │
│  (網頁)   │  │  (桌面 App)    │  │  (終端機工具)   │  │ (程式串接)  │
└───────────┘  └────────────────┘  └─────────────────┘  └─────────────┘
     ↓                ↓                     ↓                    ↓
  日常對話        本機 App 整合          開發工作流            建立應用
  文件分析        Cowork 自動化          程式碼操作           自動化流程
  快速查詢        Computer Use           多 Agent 協作         產品整合
```

---

## 方案與定價

> 定價會隨時調整，以 [官方定價頁](https://www.anthropic.com/pricing) 為準。

| 方案 | 費用 | 對象 | 特點 |
|------|------|------|------|
| **Free** | 免費 | 個人試用 | 有限使用量，可體驗基本功能 |
| **Pro** | $20/月（月繳）或 $17/月（年繳） | 個人工程師 | 更高使用量、優先存取新功能、Projects |
| **Max 5x** | $100/月 | 重度 Claude Code 使用者 | Pro 的 5 倍用量、優先存取、適合長時間跑 Agent 任務 |
| **Max 20x** | $200/月 | 超重度使用者 | Pro 的 20 倍用量、功能與 Max 5x 相同，僅用量不同 |
| **Team** | $30/人/月 | 工程團隊 | Pro 功能 + 團隊協作、更高速率限制、管理員控制台 |
| **Enterprise** | 洽談 | 企業 | 自訂合約、SSO、更高速率、資料隱私合規、專屬支援 |

**工程師評估建議：**
- 個人探索 → **Free** 或 **Pro**
- 常跑 Claude Code、常碰到用量上限 → **Max 5x**（$100/月）
- 小團隊採用（5–20 人）→ **Team**
- 有合規需求或大規模使用 → **Enterprise**

**Claude API 另有獨立計費**（依 token 用量），不包含在以上方案內，詳見 05_API.md。

---

## 隱私與資料訓練

Anthropic **預設會使用你的對話來訓練模型**。若不希望被使用，需手動關閉。

**關閉方式：** [Settings → Privacy](https://claude.ai/settings/data-privacy-controls) → **「Help improve Claude」→ 關閉**

| | 開啟（預設） | 關閉 |
|--|------------|------|
| 新對話用於訓練 | 是 | 否 |
| 資料保留期 | 最長 **5 年** | **30 天** |
| 已完成的訓練 | — | 無法撤回（已訓練的模型不受影響） |
| 安全例外 | 仍可能用於安全研究 | 仍可能用於安全研究 |

**不同方案的預設行為：**

| 方案 | 預設 |
|------|------|
| Free / Pro / Max | 預設**開啟**，需手動關閉 |
| Team / Enterprise | 預設**關閉**，不用於訓練 |

> 關閉後只影響**新的對話**。之前已被用於訓練的資料無法撤回。

---

## 用量限制說明

### 共用用量池

**Claude.ai、Claude Code、Claude Desktop 三者共用同一個用量池。** 不管在哪個介面使用，都計入同一個限額。

### 計算週期

用量有兩個層次的限制：

| 週期 | 說明 |
|------|------|
| **5 小時 session** | 從第一則訊息起算，5 小時後重置 |
| **每週總量** | 跨所有 session 的累計上限，週期結束才重置 |

5 小時 session 用完後需等待重置，但**每週總量**仍是最終天花板。

### 影響用量消耗的因素

| 因素 | 說明 |
|------|------|
| 對話長度 | 越長的對話每次訊息消耗越多 token |
| 檔案附件 | PDF、文件等大幅增加 token 消耗 |
| 模型選擇 | Opus 消耗遠多於 Sonnet |
| 功能使用 | Web search、MCP 工具、Cowork 等都會增加消耗 |
| 伺服器負載 | 尖峰時段每個 token 計費更重，5 小時額度消耗更快 |

### Max 方案的兩層限額

Max 方案有**兩個獨立的用量限額**：

```
┌─────────────────────────────────────────────┐
│  限額 1：所有模型共用（Opus + Sonnet + ...）  │  ← 先耗盡
└─────────────────────────────────────────────┘
         ↓ 耗盡後自動切換
┌─────────────────────────────────────────────┐
│  限額 2：Sonnet only（專屬較高配額）         │  ← 仍可繼續用
└─────────────────────────────────────────────┘
```

這就是升級 Max 5x 後出現「**Sonnet only**」選項的原因——當全模型配額用完，Sonnet 仍有獨立的較高配額可繼續使用。

### 尖峰時段注意

Anthropic 在 2026 年 3 月調整了尖峰時段的計費權重：

> **尖峰時段（平日）：** 05:00–11:00 PT / 13:00–19:00 GMT（約台灣時間 21:00–03:00）

尖峰期間每則訊息消耗的 token 成本更高，**5 小時額度可能不到 5 小時就用完**。每週總量不受影響，只有 session 速度變快。

### 達到限額後的選項

- **等待**：5 小時週期結束後自動重置
- **啟用 Extra Usage**：超過限額後以 API 計費繼續使用（詳見下方）
- **升級方案**：Max 5x 升級至 Max 20x

### Extra Usage

Extra Usage 讓你在用完方案額度後**無縫繼續使用**，超出部分以 API 標準費率計費。

**本質上就是 API 計費**，只是入口掛在訂閱帳號下：

| | Extra Usage | Claude API |
|--|-------------|------------|
| 入口 | 訂閱帳號（Claude.ai / Claude Code 登入） | 獨立 API Key |
| 費率 | 相同（按 token 計費） | 相同（按 token 計費） |
| 對象 | 訂閱用戶額度用完後繼續用 | 開發者串接應用 |

**費率參考（以 Sonnet 為例）：**
- Input：約 $3 / 百萬 token
- Output：約 $15 / 百萬 token
- 一般密集 coding session：約 $5–15 / 小時

**啟用方式：**
1. Settings → Usage → Extra usage → **Enable**
2. 設定月消費上限（或選 Unlimited）
3. 額度用完後系統提示時選擇繼續，即開始計費

**關閉方式：** 同頁面 Disable，關閉後達到限額就只能等待重置。

> Extra Usage 費用獨立計費，不含在方案月費內，會另外出現在帳單上。