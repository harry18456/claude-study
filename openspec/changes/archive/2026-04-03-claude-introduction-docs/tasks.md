## 1. 基礎概覽文件

- [x] 1.1 建立 GENERAL.md：撰寫 Document Claude model families（Haiku/Sonnet/Opus 差異與適用情境）
- [x] 1.2 建立 GENERAL.md：撰寫 Document access methods（Claude.ai、Claude Code、API 三種入口）
- [x] 1.3 建立 GENERAL.md：撰寫 Document pricing tiers（Free/Pro/Team/Enterprise 方案比較）

## 2. Claude.ai 介面文件

- [x] 2.1 建立 CHAT.md：撰寫 Document Artifacts feature（可產生的內容類型與互動方式）
- [x] 2.2 建立 CHAT.md：撰寫 Document Projects and Memory（Projects 建立與跨對話記憶）
- [x] 2.3 建立 CHAT.md：撰寫 Document file upload capabilities（支援的檔案類型與使用情境）
- [x] 2.4 建立 CHAT.md：撰寫 Document conversation management（對話歷史、分支、分享功能）

## 3. 協作應用文件

- [x] 3.1 建立 COWORK.md：撰寫 Document team collaboration workflows（Code Review 輔助、技術決策等場景）
- [x] 3.2 建立 COWORK.md：撰寫 Document prompt sharing practices（團隊共用 Prompt 模板與系統指neur）
- [x] 3.3 建立 COWORK.md：撰寫 Document Claude for documentation generation（技術文件自動生成）

## 4. Claude Code CLI 文件

- [x] 4.1 建立 CODE.md：撰寫 Document installation and setup（安裝步驟與認證設定）
- [x] 4.2 建立 CODE.md：撰寫 Document core CLI commands（主要指令與使用模式）
- [x] 4.3 建立 CODE.md：撰寫 Document CLAUDE.md configuration（全域與專案層級設定）
- [x] 4.4 建立 CODE.md：撰寫 Document hooks system（PreToolUse/PostToolUse/Stop 鉤子設定）
- [x] 4.5 建立 CODE.md：撰寫 Document agent and subagent capabilities（專業 Agent 清單與觸發時機）
- [x] 4.6 建立 CODE.md：撰寫 Document No Flicker Mode (Fullscreen Rendering)（`CLAUDE_CODE_NO_FLICKER=1`、v2.1.89+、vim/htop 介面風格、滑鼠支援、`CLAUDE_CODE_DISABLE_MOUSE=1` 選項、`Ctrl+o` Transcript Mode 搜尋）
- [x] 4.7 建立 CODE.md：撰寫 Document complete CLI commands（所有 terminal 指令含 `claude auth`、`claude doctor`、`claude mcp`、`claude remote-control`）
- [x] 4.8 建立 CODE.md：撰寫 Document complete slash commands（60+ 指令分類列表：session、config、tools、advanced、info）
- [x] 4.9 建立 CODE.md：撰寫 Document /buddy command（v2.1.89 新增、ASCII 虛擬寵物、確定性生成算法、18 種族、稀有度、客製化）

## 5. Claude API / SDK 文件

- [x] 5.1 建立 API.md：撰寫 Document API authentication and setup（API Key、環境變數、SDK 初始化）
- [x] 5.2 建立 API.md：撰寫 Document Messages API（請求參數、回應格式）
- [x] 5.3 建立 API.md：撰寫 Document model selection for API usage（成本 vs 能力取捨）
- [x] 5.4 建立 API.md：撰寫 Document streaming responses（Python/TypeScript 串流實作）
- [x] 5.5 建立 API.md：撰寫 Document token usage and rate limits（Token 計算與速率限制處理）

## 6. MCP 整合文件

- [x] 6.1 建立 MCP.md：撰寫 Document MCP protocol fundamentals（MCP 概念與和 API 的差異）
- [x] 6.2 建立 MCP.md：撰寫 Document MCP server types（檔案系統、資料庫、Web 服務等類別）
- [x] 6.3 建立 MCP.md：撰寫 Document MCP configuration in Claude Code（設定格式與連線驗證）
- [x] 6.4 建立 MCP.md：撰寫 Document MCP configuration in Claude.ai（網頁介面 MCP 設定）

## 7. Skill 系統文件

- [x] 7.1 建立 SKILL.md：撰寫 Document Skill system architecture（Skill 定義、儲存位置、全域 vs 專案層級）
- [x] 7.2 建立 SKILL.md：撰寫 Document skill invocation（Slash Command 呼叫方式與參數傳遞）
- [x] 7.3 建立 SKILL.md：撰寫 Document creating custom skills（Markdown 結構、Frontmatter 欄位）
- [x] 7.4 建立 SKILL.md：撰寫 Document skill best practices（觸發條件、Guardrails、Checklist 設計）
- [x] 7.5 建立 SKILL.md：撰寫 Document skill sharing（版本控制整合與團隊分享機制）

## 8. 文件審查與收尾

- [x] 8.1 確認文件結構採用「功能導向」而非「使用者旅程導向」，符合 design.md 決策說明
- [x] 8.2 確認 CODE.md 與 API.md 分開，符合 design.md 關於 CODE.md 與 API.md 分開的決策
- [x] 8.3 確認 MCP.md 獨立，符合 design.md 關於 MCP 獨立成 MCP.md 的決策
- [x] 8.4 在每份文件標註知識截止日期（緩解文件過時風險）

## 9. 內容驗證

- [x] 9.1 人工驗證 No Flicker Mode：確認 `CLAUDE_CODE_NO_FLICKER=1` 啟動、滑鼠功能、`Ctrl+o` Transcript Mode 行為符合文件描述
- [x] 9.2 人工驗證 /buddy 指令：執行 `/buddy` 確認寵物生成與文件描述一致（需 v2.1.89+）
- [x] 9.3 人工驗證完整 slash 指令清單：在 Claude Code 執行 `/help` 比對文件列出的指令是否齊全
- [x] 9.4 人工驗證 CLI 指令：確認 `claude auth`、`claude doctor`、`claude agents` 等指令輸出符合描述
- [x] 9.5 確認 No Flicker Mode 在 tmux 環境下需要 `set -g mouse on` 的警告說明已加入
