[← 上一篇：概覽](01_GENERAL.md) | [下一篇：Cowork →](03_COWORK.md)

# Claude.ai 介面功能

> **知識截止日期：** 2026 年 4 月

---

## Artifacts（生成物）

Artifacts 是 Claude.ai 的獨立輸出區塊，讓生成的內容可以即時預覽、獨立編輯，不會混入對話流。

### 支援的內容類型

| 類型 | 說明 | 使用情境 |
|------|------|---------|
| **程式碼** | 任何語言的程式碼片段 | 函式、腳本、設定檔 |
| **HTML** | 可即時預覽的網頁 | UI 原型、互動頁面 |
| **SVG** | 向量圖形 | 流程圖、圖示設計 |
| **Markdown** | 格式化文件 | 報告、說明文件 |
| **React** | 可執行的 React 元件 | 互動式 UI 展示 |

### 如何使用 Artifacts

1. 要求 Claude 生成可獨立展示的內容（例如「寫一個 HTML 頁面」）
2. Artifact 會出現在對話右側的獨立面板
3. 可直接在面板內編輯，或繼續對話要求修改
4. 點擊「Copy」複製內容，或點擊「Download」下載檔案

**工程師使用技巧：**
```
# 觸發 Artifact 的好提示方式
「幫我寫一個可以直接執行的 Python 腳本，用來...」
「生成一個 HTML 頁面，顯示...」
「畫一個系統架構圖（SVG 格式）」
```

---

## Projects（專案）與 Memory

Projects 讓你可以建立持久化的工作空間，Claude 在同一個 Project 內的所有對話都能共享上下文。

### 建立與設定 Project

1. 左側側欄 → **New Project**
2. 設定 Project 名稱
3. 在 **Project Instructions** 填寫你希望 Claude 在此 Project 內遵守的規則或背景資訊
4. 上傳相關文件（程式碼、規格、參考資料）

### Project 的持久化機制

```
┌─────────────────────────────────────────┐
│           Project: my-backend           │
│                                         │
│  📄 Project Instructions                │
│     (永久保存，每次對話都會讀取)          │
│                                         │
│  📁 上傳的文件                           │
│     - architecture.md                   │
│     - api-spec.yaml                     │
│                                         │
│  💬 對話 1   💬 對話 2   💬 對話 3       │
│     (每次對話可讀取 Project 內容)          │
└─────────────────────────────────────────┘
```

### Personal Project vs Team Project

| | Personal Project | Team Project |
|---|---|---|
| **方案** | Free / Pro / Max | Team / Enterprise |
| **分享** | 不支援 | 可分享給特定成員或整個組織 |
| **功能** | 與 Team 的 Private Project 相同 | 多了 visibility 設定 |

> Pro / Max 方案的介面直接標示為 **Personal Project**，是對應 Team Project 的區分用語，功能本身無差異，只差在無法分享。

### Project Instructions 撰寫建議

```markdown
你是我的後端開發助理。這個專案使用：
- Python 3.12 + FastAPI
- PostgreSQL 16
- 程式碼風格遵循 PEP 8
- 所有 API 回應格式：{ success, data, error }

重要背景：我們的使用者資料表結構是...
```

### Memory（記憶功能）

Memory 讓 Claude 在不同對話之間記住關於你的資訊（Free 以上方案皆可使用）：

- **設定位置**：Settings → Capabilities → Memory
- **新增／查看／刪除記憶**：同頁面點擊「View and edit your memory」進行管理

Claude 會自動從對話中摘取重點（工作背景、技術偏好、進行中的專案等），每 24 小時更新一次。不會記錄無痕模式的對話。

**兩種記憶空間：**

| | Memory Summary | Project Memory |
|---|---|---|
| **範圍** | 跨所有非 Project 對話 | 各 Project 獨立 |
| **更新** | 每 24 小時自動更新 | 每 24 小時自動更新 |

**管理操作：**

| 操作 | 效果 |
|------|------|
| **Pause Memory** | 保留現有記憶，但暫停使用與建立新記憶 |
| **Reset Memory** | 永久刪除所有記憶（**不可復原**） |

**企業管控：** 組織管理員可在 Organization Settings → Capabilities 全域停用 Memory（會**永久刪除**所有用戶記憶）。Team 方案則由各成員自行管理。

> 官方文件：[Use Claude's chat search and memory to build on previous context](https://support.claude.com/en/articles/11817273-use-claude-s-chat-search-and-memory-to-build-on-previous-context)

### 無痕模式（Incognito Chat）

對話不會儲存到歷史紀錄，也不會被 Memory 讀取或記錄。

**觸發方式：**
- 新對話右上角點擊**幽靈圖示**（Ghost icon）
- 快捷鍵 `Ctrl + Shift + I`

啟用後介面會出現黑色邊框與「Incognito chat」標籤。

**限制：**
- 只在 **Project 外**的對話可用（Project 內不顯示幽靈圖示）
- 開始後無法轉為一般對話或儲存紀錄

---

## 檔案上傳

Claude.ai 支援直接上傳各類檔案進行分析。

### 支援的檔案類型

| 類別 | 格式 |
|------|------|
| **文件** | PDF、Word (.docx)、TXT、Markdown |
| **程式碼** | 所有主流語言（.py、.js、.ts、.go 等） |
| **圖片** | PNG、JPG、GIF、WebP、SVG |
| **資料** | CSV、JSON、XML |
| **試算表** | Excel (.xlsx) |

### 工程師常用情境

```
# 分析程式碼
上傳 .py 或 .ts 檔案 → 「幫我 review 這段程式碼」

# 解讀文件
上傳 PDF 規格書 → 「這份文件的核心需求是什麼？」

# 資料分析
上傳 CSV → 「分析這份資料的趨勢」

# 截圖說明
上傳錯誤截圖 → 「這個錯誤是什麼原因？」
```

**限制：**
- 單次上傳上限：~20 MB
- 單次對話最多上傳 5 個檔案
- 無法執行上傳的程式碼（執行功能限於 Artifacts）

---

## Customize（Skills 與 Connectors）

點擊 Chat 輸入框旁的 **Customize** 按鈕，可為當前對話啟用額外能力。

### Skills

Skills 是可附加給 Claude 的工具能力，讓 Claude 在對話中執行特定動作。

**適用介面：Chat、Cowork、Claude Code（三者皆有）**

常見 Skills 範例：
- **Web search**：讓 Claude 搜尋網路上的最新資訊
- **Memory**：讓 Claude 記住跨對話的個人資訊

> Team / Enterprise 方案可將 Skills 分享給同事或整個組織。

### Connectors

Connectors 讓 Claude 連接到外部服務或資料來源，直接在對話中存取這些資源。

**適用介面：Chat、Cowork、Claude Code（三者皆有）**

常見 Connectors 範例：
- **Google Drive**：讀取雲端文件
- **GitHub**：存取 repo 內容
- **Slack / Notion / Linear**：存取工作協作工具

### Plugins

Plugin 是更高層的打包單元，將 Skills、Connectors、Sub-agents、Hooks 綁成一個可安裝、可分享的組合包。

```
Plugin = Skills + Connectors + Sub-agents + Hooks 的組合包
```

常見用途：依職能預先配置好的 AI 助理（銷售、法務、工程、設計等）。

**各介面支援狀況：**

| 介面 | Skills | Connectors | Plugins |
|------|--------|------------|---------|
| **Chat** | ✓ | ✓ | ✗ |
| **Cowork** | ✓ | ✓ | ✓（付費方案） |
| **Claude Code** | ✓ | ✓ | ✓ |

> Chat 不支援 Plugin，因為 Chat 定位為對話介面，Plugin 是為 agentic workflow 設計的。

### 在各介面中開啟 Customize

| 介面 | 操作方式 |
|------|---------|
| Chat / Cowork | 輸入框旁 **Customize** 按鈕 |
| Claude Code | 輸入框旁 `+` 按鈕 → Skills / Connectors / Plugins，或 Settings → Customize |

> **注意：** 可用項目依帳號方案與 Anthropic 推出進度而不同，實際介面顯示以當下為準。

---

## 對話管理

### 對話歷史

- 所有對話儲存於左側側欄
- 可搜尋歷史對話（側欄頂部搜尋框）
- 可重新命名對話（右鍵 → Rename）
- 可刪除對話（右鍵 → Delete）

### Chat Search（對話搜尋）

在對話中用自然語言請 Claude 搜尋過去的對話紀錄（Pro / Max / Team / Enterprise）：

- 「我們之前討論的 [主題] 是什麼？」
- 「找一下我們關於 [專案] 的對話」

搜尋使用 RAG 機制，結果會以工具呼叫形式出現，並附帶原始對話的引用連結。搜尋範圍涵蓋所有非 Project 對話；Project 內的搜尋僅限該 Project。

**停用方式：** Settings → Capabilities → 關閉「Search and reference chats」

### 對話分支（Branching）

Claude.ai 支援從任一訊息建立分支：

1. 將滑鼠移到任一訊息旁
2. 點擊「Edit」圖示重新編輯問題
3. 送出後自動建立新的對話分支
4. 可在原分支和新分支之間切換

這對於「嘗試不同提示策略」非常實用。

### 分享對話

1. 對話右上角 → **Share**
2. 選擇「Anyone with the link」
3. 複製連結分享給同事

**隱私注意事項：**
- 分享連結後，任何有連結的人都能看到對話內容
- 可隨時取消分享（Share → Unshare）
- **不要分享含有敏感資訊（API Key、密碼、個人資料）的對話**
- 分享後的連結可被搜尋引擎索引
