[← 上一篇：Code](04_CODE.md) | [下一篇：MCP →](06_MCP.md)

# Claude API / SDK

> **知識截止日期：** 2026 年 4 月  
> 最新 API 文件：[platform.claude.com/docs](https://platform.claude.com/docs)

---

## 取得 API Key 與環境設定

### 1. 取得 API Key

1. 前往 [console.anthropic.com](https://console.anthropic.com)
2. 登入後 → **API Keys** → **Create Key**
3. 複製 key（只顯示一次）

### 2. 設定環境變數

```bash
# ~/.zshrc 或 ~/.bashrc
export ANTHROPIC_API_KEY="sk-ant-api..."

# 或使用 .env 檔案（加入 .gitignore！）
ANTHROPIC_API_KEY=sk-ant-api...
```

**安全原則：絕對不要把 API Key 硬寫進程式碼或提交到 git。**

### 3. 安裝 SDK

```bash
# Python
pip install anthropic

# Node.js / TypeScript
npm install @anthropic-ai/sdk
```

### 4. 初始化客戶端

```python
# Python
import anthropic

client = anthropic.Anthropic()  # 自動讀取 ANTHROPIC_API_KEY
```

```typescript
// TypeScript
import Anthropic from "@anthropic-ai/sdk";

const client = new Anthropic(); // 自動讀取 ANTHROPIC_API_KEY
```

---

## Messages API

Messages API 是 Claude 的核心介面，以對話形式傳送請求。

### 基本呼叫

```python
# Python
message = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "解釋什麼是 dependency injection"}
    ]
)
print(message.content[0].text)
```

```typescript
// TypeScript
const message = await client.messages.create({
  model: "claude-sonnet-4-6",
  max_tokens: 1024,
  messages: [{ role: "user", content: "解釋什麼是 dependency injection" }],
});
console.log(message.content[0].text);
```

### 請求參數說明

| 參數 | 必填 | 說明 |
|------|------|------|
| `model` | ✓ | 模型 ID（見下方模型選擇） |
| `max_tokens` | ✓ | 回應的最大 token 數 |
| `messages` | ✓ | 對話陣列，`role` 為 `user` 或 `assistant` |
| `system` | — | 系統指令，設定 Claude 的角色與行為 |
| `temperature` | — | 創意程度 0.0–1.0（預設 1.0） |
| `top_p` | — | 核採樣參數（通常不需調整） |

### 回應格式

```json
{
  "id": "msg_01XFDUDYJgAACzvnptvVoYEL",
  "type": "message",
  "role": "assistant",
  "content": [
    {
      "type": "text",
      "text": "Dependency injection 是一種..."
    }
  ],
  "model": "claude-sonnet-4-6",
  "stop_reason": "end_turn",
  "usage": {
    "input_tokens": 15,
    "output_tokens": 120
  }
}
```

### 多輪對話

```python
messages = [
    {"role": "user", "content": "Python 的 list 和 tuple 有什麼差異？"},
    {"role": "assistant", "content": "主要差異在於..."},  # 上一輪回應
    {"role": "user", "content": "那什麼時候該用 tuple？"},  # 繼續對話
]

response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    messages=messages
)
```

---

## 模型選擇

根據應用場景選擇適合的模型：

| 模型 | ID | 速度 | 成本 | 適用情境 |
|------|----|----|------|---------|
| **Haiku 4.5** | `claude-haiku-4-5-20251001` | 最快 | 最低 | 分類、摘要、簡單問答、高頻 Agent worker |
| **Sonnet 4.6** | `claude-sonnet-4-6` | 中等 | 中等 | 程式碼生成、複雜分析、主要工作流程 |
| **Opus 4.7** | `claude-opus-4-7` | 較慢 | 最高 | 複雜推理、agentic coding、架構決策、需要最佳品質 |

> **Opus 4.7 遷移注意（2026-04-16）：**
> - 定價與 4.6 相同（$5 / $25 per MTok）；4.6 仍可用但已列為 legacy
> - **Extended thinking → Adaptive thinking**：4.7 不再支援 `thinking` block / `type: "thinking"` 的 Extended thinking API。若你的程式碼用了 Extended thinking，遷移時需改用 Adaptive thinking 參數，否則 4.7 會忽略或報錯
> - 新增 **`xhigh`** effort level（介於 `high` 與 `max` 之間），官方建議 coding / agentic 預設用 `xhigh`
> - **新 tokenizer**：同樣 1M context 但壓縮比不同（~555k 字 vs 4.6 的 ~750k 字），prompt 如果已經接近 context 上限，升級後可能需要重新調整
> - 另外：Claude Sonnet 4 / Opus 4 已 deprecated，**2026-06-15 退役**；Claude Haiku 3 **2026-04-19 退役**

**API 費用計算方式：** 依 input tokens + output tokens 計費，詳見 [官方定價](https://www.anthropic.com/pricing)。

---

## Streaming（串流）回應

對於需要即時顯示的使用情境（聊天介面、即時反饋），使用串流可大幅改善使用者體驗。

### Python 串流

```python
with client.messages.stream(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    messages=[{"role": "user", "content": "寫一個 bubble sort 的說明"}],
) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)
```

### TypeScript 串流

```typescript
const stream = await client.messages.stream({
  model: "claude-sonnet-4-6",
  max_tokens: 1024,
  messages: [{ role: "user", content: "寫一個 bubble sort 的說明" }],
});

for await (const chunk of stream) {
  if (
    chunk.type === "content_block_delta" &&
    chunk.delta.type === "text_delta"
  ) {
    process.stdout.write(chunk.delta.text);
  }
}
```

### 串流事件類型

| 事件 | 說明 |
|------|------|
| `message_start` | 開始新訊息 |
| `content_block_start` | 開始新內容區塊 |
| `content_block_delta` | 文字增量（最常用） |
| `content_block_stop` | 內容區塊結束 |
| `message_delta` | 訊息 metadata 更新（stop_reason 等） |
| `message_stop` | 串流結束 |

---

## Token 計算與速率限制

### Token 基本概念

- 1 個 token ≈ 0.75 個英文單字 ≈ 0.5 個中文字
- 每次 API 呼叫消耗 input tokens（你的問題）+ output tokens（Claude 的回答）
- 使用 `response.usage` 查看實際用量

```python
response = client.messages.create(...)
print(f"Input: {response.usage.input_tokens} tokens")
print(f"Output: {response.usage.output_tokens} tokens")
```

### 速率限制（Rate Limits）

Anthropic 依帳號等級設有速率限制：

| 限制類型 | 說明 |
|----------|------|
| RPM | 每分鐘請求數 |
| TPM | 每分鐘 token 數 |
| TPD | 每天 token 數 |

### 處理速率限制錯誤

```python
import anthropic
import time

def call_with_retry(client, **kwargs, max_retries=3):
    for attempt in range(max_retries):
        try:
            return client.messages.create(**kwargs)
        except anthropic.RateLimitError as e:
            if attempt == max_retries - 1:
                raise
            wait_time = 2 ** attempt  # exponential backoff
            print(f"Rate limited, waiting {wait_time}s...")
            time.sleep(wait_time)
```

### 成本控制建議

1. **快速路徑用 Haiku**：分類、判斷、簡單擷取 → 用 Haiku 節省 3–5 倍成本
2. **設定合理的 max_tokens**：避免不必要的長回應
3. **快取 system prompt**：使用 prompt caching 減少重複 input tokens
4. **監控使用量**：在 console.anthropic.com 查看 usage dashboard
