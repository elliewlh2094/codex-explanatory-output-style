# Codex 解釋型輸出風格技能

**[English](README.md)** | 中文

## 簡介

`explanatory-output-style` 是一個 OpenAI Codex 技能（skill）封裝，用來在 Codex 可行的範圍內重現 Claude 的解釋型輸出風格。當技能啟用時，Codex 會持續完成使用者的程式任務，同時補上簡短且具體的教學說明，內容聚焦在實作選擇、本地模式、權衡與儲存庫相關細節。

這個封裝以此儲存庫中的 `explanatory-output-style/` 目錄提供。安裝時請直接複製這個目錄，並把它當作最終技能目錄使用。

這個技能的參考來源是 Anthropic 官方的 Claude 插件：
`https://github.com/anthropics/claude-plugins-official/tree/main/plugins/explanatory-output-style`

Claude 原版插件透過 SessionStart hook 在每個 Claude 工作階段注入解釋型指令。Codex 技能沒有等價的 always-on hook，因此此轉換版本把較廣的觸發面放進技能描述與 metadata，讓它在程式協助與專案修改任務中更容易被隱式選用。

## 行為

- 將解釋型說明視為目前寫碼、規劃、除錯、review、設計或專案修改任務的回應模式。
- 在有意義的寫碼工作開始前加入一個簡短的 `★ Insight` 區塊。
- 在有意義的寫碼工作完成後加入對應的 `★ Insight` 區塊。
- 當任務進入新的主要寫碼階段時，再加入新的一組 `★ Insight` 前後區塊。
- 當 Codex 只是規劃、review、除錯、設計，或建議如何修改專案內容時，會在關鍵推理或建議處加入簡短的 `★ Insight` 區塊。
- 讓教學內容緊扣目前的檔案、模組、資料流、測試與失敗模式。
- 將教學內容保留在對話中，不寫進原始碼檔案。
- 透過 `explanatory-output-style/agents/openai.yaml` 支援隱式觸發。

## Insight 格式

這個技能使用以下 `★ Insight` 包裝格式：

```markdown
`★ Insight ──────────────────────────────────────`
[2-3 個教學重點]
`─────────────────────────────────────────────────`
```

區塊中的重點會著重在：

- 這次任務採用的具體實作方式。
- 儲存庫既有的本地模式或慣例。
- 這次修改背後的權衡或設計判斷。
- 幫助理解這個實作為何適合目前儲存庫的脈絡細節。

## 觸發範例

下列自然語句有助於讓 Codex 選用這個技能：

- `implement this change`
- `plan this project change`
- `how should I modify this project`
- `debug this issue`
- `review this code`
- `design this feature`
- `use explanatory mode`
- `use explanatory output style`
- `explain as you code`
- `teach while coding`
- `walk me through your implementation choices`

## 安裝方式

請將此儲存庫中的 `explanatory-output-style/` 目錄複製到下列任一位置：

```text
${CWD}/.codex/skills/explanatory-output-style/
${CODEX_HOME}/skills/explanatory-output-style/
```

安裝後的目錄內容應為：

```text
explanatory-output-style/
  SKILL.md
  agents/
    openai.yaml
```

完成複製後，重新啟動 Codex，讓技能在新的工作階段中被載入。
