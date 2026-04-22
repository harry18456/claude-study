[← 上一篇：Skill](07_SKILL.md)

# Claude Managed Agents

> **知識截止日期：** 2026 年 4 月（公開 Beta 於 2026/04/08 發布）  
> 官方文件：[platform.claude.com/docs/en/managed-agents](https://platform.claude.com/docs/en/managed-agents/overview)  
> 官方介紹影片：[What is Claude Managed Agents?](https://www.youtube.com/watch?v=NLWiIj47IdI)

---

## 什麼是 Managed Agents？

Claude Managed Agents 是 Anthropic 推出的**全託管 agent 基礎設施**，讓你不需要自己建構 agent loop、工具執行層和 runtime，就能部署生產級的自主 agent。

官方定位（原文）：

> **Pre-built, configurable agent harness that runs in managed infrastructure.**

### Agent Harness 是什麼？

2026 年 AI 業界最熱門的架構概念之一。簡單說：

- **模型**負責生成回應
- **Harness**負責其他一切 — 工具編排、context 管理、錯誤恢復、權限控管、生命週期管理

業界共識：2025 年證明了 agent 可行，2026 年的重點是讓 agent **可靠運作**。決定 agent 成敗的不是模型本身，是 harness。

### 與 Messages API 的定位差異

| | Messages API | Managed Agents |
|---|---|---|
| **本質** | 直接存取模型的 prompting 介面 | 預建的 agent harness + 託管基礎設施 |
| **適合** | 自建 agent loop、需要細粒度控制 | 長時間執行的任務、非同步工作 |
| **你負責** | 一切（loop、工具執行、狀態管理、沙箱） | 定義 agent 設定 + 傳送任務 |
| **Anthropic 負責** | 模型推論 | 模型推論 + 工具編排 + context 管理 + 錯誤恢復 + 沙箱 |

---

## 核心概念

Managed Agents 圍繞四個核心元素運作：

| 概念 | 說明 |
|------|------|
| **Agent** | 模型 + system prompt + 工具 + MCP servers + Skills 的組合定義 |
| **Environment** | 容器範本設定：預裝套件、網路存取規則、掛載檔案 |
| **Session** | 一個正在執行的 agent 實例，在 environment 中執行特定任務並產生輸出 |
| **Events** | 你的應用程式與 agent 之間交換的訊息（user turns、tool results、status updates） |

```
你的應用程式                     Anthropic 託管基礎設施
┌──────────────┐                ┌─────────────────────────────────┐
│              │   建立 Agent   │  Agent（模型 + prompt + 工具）   │
│              │───────────────▶│                                 │
│              │   建立 Env     │  Environment（容器範本）         │
│              │───────────────▶│                                 │
│              │   啟動 Session │  Session（執行中的 agent 實例）  │
│              │───────────────▶│  ┌───────────────────────────┐  │
│              │                │  │ Claude 自主決定工具呼叫    │  │
│  傳送 Event  │───────────────▶│  │ Bash / 檔案操作 / 搜尋   │  │
│              │                │  │ 在安全沙箱中執行           │  │
│  接收 SSE    │◀───────────────│  │ 串流回傳結果              │  │
│              │                │  └───────────────────────────┘  │
└──────────────┘                └─────────────────────────────────┘
```

---

## 運作流程

```
1. 建立 Agent        定義模型、system prompt、工具、Skills
        ↓
2. 建立 Environment  設定容器：套件、網路、檔案
        ↓
3. 啟動 Session      將 Agent + Environment 組合為執行實例
        ↓
4. 傳送 Events       送出 user message → Claude 自主執行工具 → SSE 串流回應
        ↓
5. 引導或中斷        隨時送新 event 引導方向，或中斷重新調整
```

**背後發生的事：**

1. **佈建容器** — 依你的 Environment 設定啟動
2. **執行 agent loop** — Claude 根據你的訊息決定使用哪些工具
3. **工具執行** — 檔案寫入、bash 指令等在容器內安全執行
4. **串流事件** — 即時回傳 agent 的工作進度
5. **閒置** — agent 完成後發出 `session.status_idle` 事件

---

## Quick Start

> 所有 Managed Agents API 請求都需要 `managed-agents-2026-04-01` beta header。SDK 會自動設定。

### Step 1：建立 Agent

```python
# Python
from anthropic import Anthropic

client = Anthropic()

agent = client.beta.agents.create(
    name="Coding Assistant",
    model="claude-sonnet-4-6",
    system="You are a helpful coding assistant. Write clean, well-documented code.",
    tools=[
        {"type": "agent_toolset_20260401"},
    ],
)

print(f"Agent ID: {agent.id}, version: {agent.version}")
```

```typescript
// TypeScript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic();

const agent = await client.beta.agents.create({
  name: "Coding Assistant",
  model: "claude-sonnet-4-6",
  system: "You are a helpful coding assistant. Write clean, well-documented code.",
  tools: [{ type: "agent_toolset_20260401" }],
});

console.log(`Agent ID: ${agent.id}, version: ${agent.version}`);
```

`agent_toolset_20260401` 啟用完整的預建工具集（bash、檔案操作、web search 等）。建立後保存 `agent.id`，後續建立 session 時需要引用。

### Step 2：建立 Environment

```python
environment = client.beta.environments.create(
    name="quickstart-env",
    config={
        "type": "cloud",
        "networking": {"type": "unrestricted"},
    },
)

print(f"Environment ID: {environment.id}")
```

```typescript
const environment = await client.beta.environments.create({
  name: "quickstart-env",
  config: {
    type: "cloud",
    networking: { type: "unrestricted" },
  },
});

console.log(`Environment ID: ${environment.id}`);
```

### Step 3：啟動 Session

```python
session = client.beta.sessions.create(
    agent=agent.id,
    environment_id=environment.id,
    title="Quickstart session",
)

print(f"Session ID: {session.id}")
```

```typescript
const session = await client.beta.sessions.create({
  agent: agent.id,
  environment_id: environment.id,
  title: "Quickstart session",
});

console.log(`Session ID: ${session.id}`);
```

### Step 4：傳送訊息 + 串流回應

```python
with client.beta.sessions.events.stream(session.id) as stream:
    client.beta.sessions.events.send(
        session.id,
        events=[
            {
                "type": "user.message",
                "content": [
                    {
                        "type": "text",
                        "text": "Create a Python script that generates the first 20 Fibonacci numbers and saves them to fibonacci.txt",
                    },
                ],
            },
        ],
    )

    for event in stream:
        match event.type:
            case "agent.message":
                for block in event.content:
                    print(block.text, end="")
            case "agent.tool_use":
                print(f"\n[Using tool: {event.name}]")
            case "session.status_idle":
                print("\n\nAgent finished.")
                break
```

```typescript
const stream = await client.beta.sessions.events.stream(session.id);

await client.beta.sessions.events.send(session.id, {
  events: [
    {
      type: "user.message",
      content: [
        {
          type: "text",
          text: "Create a Python script that generates the first 20 Fibonacci numbers and saves them to fibonacci.txt",
        },
      ],
    },
  ],
});

for await (const event of stream) {
  if (event.type === "agent.message") {
    for (const block of event.content) {
      process.stdout.write(block.text);
    }
  } else if (event.type === "agent.tool_use") {
    console.log(`\n[Using tool: ${event.name}]`);
  } else if (event.type === "session.status_idle") {
    console.log("\n\nAgent finished.");
    break;
  }
}
```

**預期輸出：**

```text
I'll create a Python script that generates the first 20 Fibonacci numbers and saves them to a file.
[Using tool: write]
[Using tool: bash]
The script ran successfully. Let me verify the output file.
[Using tool: bash]
fibonacci.txt contains the first 20 Fibonacci numbers (0 through 4181).

Agent finished.
```

---

## 內建工具

| 工具類別 | 說明 |
|----------|------|
| **Bash** | 在容器中執行 shell 指令 |
| **File operations** | 讀取、寫入、編輯、glob、grep 容器中的檔案 |
| **Web search & fetch** | 搜尋網路並擷取 URL 內容 |
| **MCP servers** | 連接外部工具提供者（第三方 API、資料庫等） |

使用 `agent_toolset_20260401` 可一次啟用全部預建工具。也可以個別設定，或透過 `custom tools` 自定義工具。

---

## 架構設計亮點

Anthropic 的 Engineering Blog 揭示了 Managed Agents 的內部架構，核心思想是**三層虛擬化解耦**：

```
┌─────────────────────────────────────────────────┐
│  Session（持久化 event log）                     │
│  ── append-only，儲存在容器外部，可恢復 ──       │
├─────────────────────────────────────────────────┤
│  Brain / Harness（agent loop）                   │
│  ── 呼叫 Claude + 路由工具輸出 ──               │
│  ── 無狀態：crash 後透過 event log 恢復 ──      │
├─────────────────────────────────────────────────┤
│  Hands / Sandbox（執行環境）                     │
│  ── 執行程式碼與檔案操作 ──                      │
│  ── 容器化，失敗即重建，互不影響 ──              │
└─────────────────────────────────────────────────┘
```

### 關鍵設計決策

| 決策 | 效果 |
|------|------|
| **Harness 與容器解耦** | 容器從「寵物」變「牛群」— 隨建隨棄，失敗時觸發工具錯誤讓 Claude 自動重試 |
| **Harness 無狀態化** | crash 後透過 `wake(sessionId)` 取回 event log 即可恢復，不遺失進度 |
| **認證隔離** | 不受信任的生成程式碼與認證資訊分離執行，防止 prompt injection 竊取 token |
| **Session 作為持久 context** | 長時間任務超出 context window 時，session 提供可回溯的完整歷史 |

**效能影響：** 解耦後 Time-to-First-Token（TTFT）在 p50 降低約 **60%**，p95 降低超過 **90%**（推論不需等待容器啟動）。

> 來源：[Scaling Managed Agents: Decoupling the brain from...](https://www.anthropic.com/engineering/managed-agents)

---

## 與其他方案的比較

| | Messages API | Claude Code Subagent | Managed Agents |
|---|---|---|---|
| **執行位置** | 你的伺服器 | 本地開發機 | Anthropic 雲端 |
| **Agent Loop** | 自己建構 | Claude Code 內建 | Anthropic 託管 |
| **沙箱** | 自己處理 | 本地 process | 安全容器 |
| **工具執行** | 自己實作 | Claude Code 工具集 | 預建工具集 |
| **狀態管理** | 自己處理 | Session 內記憶 | 持久化 event log |
| **長時間任務** | 需自行管理 | 受限於本地 session | 原生支援（數小時） |
| **適用情境** | 需要完全控制的自訂 agent | 開發期間的本地自動化 | 生產級部署、非同步工作 |
| **成本模型** | token 計費 | Claude Code 訂閱 | token + session-hour |

**選擇原則：**
- 需要完全控制 agent 行為 → **Messages API**
- 開發期間的本地自動化 → **Claude Code Subagent**（見 [04_CODE.md](04_CODE.md) Subagent 章節）
- 快速部署生產級 agent → **Managed Agents**

---

## 企業案例

| 公司 | 應用場景 | 成果 |
|------|----------|------|
| **Notion** | 程式碼撰寫、網站生成、簡報製作 | 支援數十個並行任務 |
| **Rakuten** | 產品、銷售、行銷、財務、HR 各專科 agent | 每個 agent **一週內**部署 |
| **Asana** | AI 隊友，與人類協作完成任務 | 快速添加進階功能 |
| **Vibecode** | AI 原生應用開發 | 基礎設施部署速度快 **10 倍** |
| **Sentry** | 除錯 agent → 撰寫修補 → 開 PR | 從數月縮短到**數週** |

> 來源：[Claude Managed Agents: get to production 10x faster](https://claude.com/blog/claude-managed-agents)

---

## 定價

| 項目 | 費用 |
|------|------|
| 模型 token 使用 | 依照標準 API 定價（與 Messages API 相同） |
| 活躍 runtime | $0.08 / session-hour（以毫秒計量） |
| Web search | $10 / 1,000 次搜尋 |

> 定價資訊來自發布時的報導，實際費用以 [官方定價頁面](https://www.anthropic.com/pricing) 為準。

**成本控制建議：**
- Agent 與 Environment 建立一次、重複引用 — 不需每次重建
- 善用內建的 prompt caching 和 compaction — Managed Agents 自動處理
- 輕量任務考慮用 Haiku 模型降低 token 成本

---

## Agent 生命週期管理

Agent 建立後可以更新和封存：

| 操作 | 行為 |
|------|------|
| **Update** | 產生新版本（version +1），支援版本歷史追蹤 |
| **List versions** | 查看完整版本歷史 |
| **Archive** | Agent 變為唯讀，新 session 不能引用，但既有 session 繼續執行 |

更新時只需傳入要改的欄位，省略的欄位會保留原值。傳入 `version` 確保從已知狀態更新（樂觀鎖定）。

---

## 限制與注意事項

| 項目 | 說明 |
|------|------|
| **Beta 狀態** | 所有 endpoint 需附加 `managed-agents-2026-04-01` beta header（SDK 自動處理） |
| **Rate Limits** | 建立類 endpoint：60 req/min；讀取類 endpoint：600 req/min |
| **Research Preview 功能** | Multi-agent、Memory、Outcomes 需[申請存取](https://claude.com/form/claude-managed-agents) |
| **模型支援** | Claude 4.5 及更新版本 |
| **品牌規範** | 整合時可用「Claude Agent」或「Powered by Claude」，**不可**使用「Claude Code」或「Claude Cowork」相關品牌 |

---

## 延伸閱讀

- [官方文件：Overview](https://platform.claude.com/docs/en/managed-agents/overview)
- [官方文件：Quickstart](https://platform.claude.com/docs/en/managed-agents/quickstart)
- [官方文件：Agent Setup](https://platform.claude.com/docs/en/managed-agents/agent-setup)
- [Blog：Claude Managed Agents: get to production 10x faster](https://claude.com/blog/claude-managed-agents)
- [Engineering Blog：Scaling Managed Agents](https://www.anthropic.com/engineering/managed-agents)
