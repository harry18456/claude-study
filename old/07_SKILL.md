[← 上一篇：MCP](06_MCP.md) | [下一篇：Managed Agents →](08_MANAGED_AGENTS.md)

# Claude Code Skill 系統

> **知識截止日期：** 2026 年 4 月

---

## Skill 系統架構

Skill 是以 Markdown 格式定義的**可重用工作流程**。Claude Code 讀取 Skill 檔案，依照其中的指示執行結構化任務。

### Skill 的本質

```
一般提問：                    使用 Skill：
┌─────────────┐               ┌─────────────────────────────┐
│ 每次都要    │               │ 一次定義，反覆使用          │
│ 重新描述   │               │                             │
│ 需求        │               │ /code-review                │
│             │               │ → 自動按照團隊標準執行       │
└─────────────┘               └─────────────────────────────┘
```

### Skill 儲存位置

| 層級 | 路徑 | 作用範圍 |
|------|------|---------|
| **全域** | `~/.claude/skills/` | 所有 Claude Code session |
| **專案** | `.claude/skills/`（repo 根目錄） | 只在此 repo 有效 |
| **插件包** | `~/.claude/plugins/` | 由插件系統管理 |

**載入優先順序：** 專案 skills > 全域 skills > 插件 skills

> **Commands 與 Skills 的關係：** 早期 Claude Code 有兩個獨立系統——`~/.claude/commands/*.md`（指令）和 `~/.claude/skills/*/SKILL.md`（技能）。**兩者現已合併**，放在任一位置都會產生相同的 slash command 介面。建議統一用 Skills（`skills/` 目錄），因為它支援更多功能（frontmatter 控制、supporting files、動態 context 注入）。`commands/` 目錄仍可用，但屬舊式寫法。

### 內建 Skill 範例

Claude Code 預設附帶的 skill 包含：
- `/compact` — 壓縮對話歷史
- `/help` — 列出所有可用指令
- 其他由插件提供的 skills（如 `/tdd`、`/code-review`）

---

## Skill 目錄結構

Skill 可以是**單一 Markdown 檔**，也可以是**目錄**（附帶輔助資源）：

```
skill-name/
├── SKILL.md              # 必要，主指示檔
└── 可選資源/
    ├── scripts/          # 腳本（deterministic 任務，不用每次重寫）
    ├── references/       # 文件（大型參考資料，按需載入）
    └── assets/           # 模板、字型、靜態檔案
```

### 三層載入機制

Claude Code 用三個層次載入 skill 內容，避免浪費 context：

| 層級 | 內容 | 時機 | 大小建議 |
|------|------|------|---------|
| **1. Metadata** | `name` + `description` | **永遠在 context** | ~100 字 |
| **2. SKILL.md body** | 完整指示內容 | skill 觸發時載入 | 500 行以內 |
| **3. Bundled resources** | scripts / references / assets | 按需載入 | 無限制 |

**實務意義：**
- `description` 字數雖少，卻是**觸發機制的核心**，要寫得具體
- SKILL.md 超過 500 行時，把詳細內容移到 `references/`，在 SKILL.md 中用指標引用
- `scripts/` 裡的腳本可以直接執行，**不用先載入才能用**

---

## 呼叫 Skill

### 方法 1：Slash Command

在 Claude Code 輸入框直接輸入：

```bash
/skill-name
/skill-name argument1 argument2
```

範例：
```bash
/tdd                    # 無參數
/code-review auth.ts    # 帶參數：指定要 review 的檔案
/commit "feat: add login"
```

### 方法 2：自然語言觸發

Claude 根據 `description` 的內容自動判斷何時呼叫：

```
你：我想在寫程式碼前先做測試
Claude：[自動觸發 tdd-guide skill，引導你完成 TDD 流程]
```

### 傳入參數

參數在 skill 檔案中以 `$ARGUMENTS` 接收：

```bash
# 呼叫時
/deploy production

# skill 檔案內使用
部署目標：$ARGUMENTS（即 "production"）
```

---

## 建立自訂 Skill

### 基本結構

```markdown
---
name: my-skill
description: 一句話說明這個 skill 的用途（會顯示在 /help 清單）
---

# My Skill 名稱

## 觸發條件

當使用者 [描述何時觸發]...

## 流程

1. 第一步：...
2. 第二步：...
3. 第三步：...

## 注意事項（Guardrails）

- 不要做 X
- 必須確認 Y 後才執行 Z
```

### 實際範例：PR 準備 Skill

建立 `.claude/skills/prepare-pr.md`：

````markdown
---
name: prepare-pr
description: 分析 git diff 並準備 PR 說明
---

# PR 準備工具

## 觸發條件

當使用者要送出 PR、需要寫 PR 說明時使用。

## 流程

1. 執行 `git diff main...HEAD` 取得所有變更
2. 分析變更的範圍和影響
3. 生成 PR 標題（格式：`type: description`）
4. 生成 PR 說明，包含：
   - **Why**：為什麼需要這個變更
   - **What Changes**：主要改動清單
   - **Testing**：如何驗證（TODO 形式）
5. 詢問使用者是否需要調整

## Guardrails

- 不要直接 push 或建立 PR，只生成內容
- 若 diff 超過 500 行，先詢問使用者要 review 哪個部分
````

### Frontmatter 欄位說明

| 欄位 | 必填 | 說明 |
|------|------|------|
| `name` | ✓ | skill 的識別名稱（即 slash command 名稱） |
| `description` | ✓ | **觸發機制的核心**，Claude 靠此判斷何時使用（詳見下方） |
| `compatibility` | — | 相依需求說明（e.g., `需要 gh CLI`） |
| `allowed-tools` | — | 限制可用工具（e.g., `[Bash, Read, Write]`） |
| `context` | — | `fork` = 在獨立子 agent 執行，隔離 context |
| `agent` | — | `true` = 以 agent 模式運行 |
| `disable-model-invocation` | — | `true` = 只有使用者能手動呼叫，Claude 不會自動觸發（適合 `/deploy`、`/commit` 等有副作用的指令） |
| `user-invocable` | — | `false` = 只有 Claude 能自動觸發，不出現在 `/help`（適合背景知識型 skill） |

### description 的重要性

`description` 是 Claude 判斷「是否要用這個 skill」的**唯一依據**。寫得太模糊會導致 Claude 在應該用的時候不用（undertrigger）。

**原則：同時說明「做什麼」和「什麼時候用」，並且稍微積極一點：**

```yaml
# 不好：太模糊
description: Code review 工具

# 好：具體說明觸發情境
description: >
  對程式碼進行安全性與品質審查。
  當使用者提交程式碼、準備 PR、或要求 review 任何檔案時使用，
  即使使用者沒有明確說「code review」也應觸發。
```

---

## 撰寫 Skill 的最佳實踐

### 1. 說明「為什麼」而非只寫「做什麼」

Claude 理解指令背後的理由時，執行效果更好。與其寫死規則，不如解釋原因：

```markdown
# 不好：死規則
## Guardrails
- 永遠不要直接刪除檔案

# 好：解釋原因
## Guardrails
- 刪除是不可逆操作，建議先移到暫存目錄，確認後再清除，
  避免誤刪造成無法復原的損失
```

### 2. 加入 Guardrails（防護欄）

Guardrails 防止 Claude 做超出預期的事：

```markdown
## Guardrails

- 在執行任何寫入操作前，先顯示預覽並要求確認
- 若有不確定的地方，先問使用者而不是猜測
```

### 3. 使用 Checklist 確保完整性

```markdown
## 完成標準

在結束前確認以下都已完成：
- [ ] 所有修改的檔案已儲存
- [ ] 測試通過（若有相關測試）
- [ ] 輸出結果已顯示給使用者
```

### 4. 定義輸出格式

需要固定輸出格式時，直接在 skill 裡給模板：

```markdown
## 輸出格式

永遠用以下格式回報：

# [標題]
## 摘要
## 主要發現
## 建議行動
```

### 5. 保持單一職責

一個 skill 只做一件事。需要複合功能時，用多個 skill 串接，而非塞在一個 skill 裡。

### 6. 把重複腳本放進 scripts/

如果每次觸發 skill 時 Claude 都會寫同一段 helper script，就該把它放進 `scripts/`：

```
my-skill/
├── SKILL.md
└── scripts/
    └── parse_output.py   # 每次都用的解析腳本
```

在 SKILL.md 中引用：
```markdown
使用 `scripts/parse_output.py` 解析輸出，不要重新寫一遍。
```

---

## skill-creator：官方 Skill 建立工具

> **來源：** `claude-plugins-official`（Anthropic 官方插件）  
> **呼叫：** `/skill-creator`

`skill-creator` 是 Anthropic 官方提供的 skill，用來**把任何工作流程轉成 skill**。它實現了「萬物皆可 skill」的哲學——只要是重複性的工作，都值得封裝。

### 它幫你做什麼

1. **訪談**：問你想要什麼、何時觸發、輸出格式
2. **寫草稿**：生成 SKILL.md 初稿
3. **跑測試**：用 2-3 個真實 prompt 測試效果
4. **評估迭代**：開啟 browser viewer 讓你比較有/無 skill 的差異
5. **優化 description**：自動調整觸發描述，提高準確率
6. **打包輸出**：生成 `.skill` 檔案，可分享安裝

### 何時使用

- 你有一個重複性工作流程，每次都要重新描述
- 你想把現在這個 session 的工作方式「保存下來」
- 你想分享工作流給團隊成員

### 使用方式

直接呼叫，告訴它你想做什麼：

```
/skill-creator
我每次 deploy 前都要做一堆檢查，幫我把這個流程變成 skill
```

或把現有 session 的工作方式轉成 skill：

```
/skill-creator
把剛才我們做的 code review 流程轉成 skill
```

---

## 團隊共用 Skill

### 提交到版本控制

將 skill 放在 `.claude/skills/` 並提交到 repo，所有團隊成員 clone 後即可使用：

```bash
.claude/
└── skills/
    ├── code-review.md    # 團隊標準 code review
    ├── prepare-pr.md     # PR 準備
    ├── db-migration.md   # 資料庫遷移流程
    └── deploy.md         # 部署檢查清單
```

### 使用插件包分享

若要跨多個 repo 共用（或分享給社群）：

1. 建立一個獨立的 skill 插件 repo
2. 透過 Claude Code 的插件系統安裝：
   ```bash
   claude plugins install github:your-org/claude-skills-pack
   ```

### 版本控制建議

```markdown
---
name: code-review
description: 標準 code review（v2.1）
version: 2.1
---
```
