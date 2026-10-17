# Codex 解釋型輸出風格技能

**[English](README.md)** | 中文

**讓 Codex 在完成任務時，補上簡短的設計說明。**

`explanatory-output-style` 是一個 Codex 技能。當你希望從寫碼、除錯、規劃或審查中學習時，技能會加入 `★ Insight` 區塊。每個區塊提供二至三個具體重點，說明程式碼與實作選擇。

## 使用範例

安裝後，在請求中指定技能。以下以支援缺少標籤的輸入作為假設範例：

```text
請使用 $explanatory-output-style，讓 normalize_label(None) 回傳空字串，並教我這項修改如何保留既有的字串處理行為。
```

回應中額外的教學區塊可能如下：

```markdown
`★ Insight ──────────────────────────────────────`
- 明確檢查 value is None 可以處理缺少標籤的輸入，避免同時接受 0 或 False。
- 字串仍經過原有的 strip().lower()，保留去除首尾空白與轉為小寫的行為。
`─────────────────────────────────────────────────`
```

Codex 仍會完成原任務，並回報修改與驗證結果。Insight 區塊會補充相關實作選擇的理由。

## 適用情境

| 你的請求 | 預期行為 |
|---|---|
| 明確指定 `$explanatory-output-style` | 對目前要求的任務使用技能。 |
| 要求學習，例如「修正錯誤，並教我為什麼這樣改」 | Codex 可以自動選用技能。 |
| 要求理解計畫，或從審查中學習 | 說明相關取捨或發現，不要求必須修改程式碼。 |
| 只要求實作、除錯或審查 | 單憑這項請求，不應啟用教學技能。 |
| 要求只提供指令、簡短回答或停止教學 | 即使明確指定技能，也遵守這項要求。 |

`policy.allow_implicit_invocation: true` 允許自動選用，但不保證每次都能匹配。若 Codex 漏選，請明確指定技能。

## 安裝方式

本儲存庫以完整的 `explanatory-output-style/` 目錄提供技能。除了 Codex，不需要建置、伺服器或額外的執行期依賴。

請將目錄複製到[官方列出的任一本機技能位置](https://learn.chatgpt.com/docs/build-skills)：

| 適用範圍 | 目的位置 |
|---|---|
| 單一專案 | `<你的專案>/.agents/skills/explanatory-output-style/` |
| 自己的多個專案 | `~/.agents/skills/explanatory-output-style/` |

複製前先檢查目的位置，保留既有安裝內容，並避免在多個位置重複安裝同一技能。安裝後應包含：

```text
explanatory-output-style/
  SKILL.md
  agents/
    openai.yaml
```

Codex 會自動偵測技能變更。若技能未出現，請重新啟動 Codex。

## 行為與限制

- 根據實際程式碼、儲存庫慣例、測試或觀察到的錯誤解釋選擇。說明保持簡短，並使用使用者的語言。
- 在有教學價值的工作之前或之後加入 Insight 區塊，不要求前後成對。
- 將說明保留在對話中，除非使用者明確要求寫入檔案。避免通用教學、例行工具操作敘述與重複摘要。
- 教學適用於目前要求的任務。本封裝不設定整個工作階段的輸出風格，也不修改全域指令。不應假設教學模式會延續至無關的後續請求。

## 設計與來源

本技能參考 [Claude Code 現行的 Explanatory 輸出風格](https://code.claude.com/docs/en/output-styles)。Claude 的風格適用於整個工作階段；本技能則針對使用者要求的 Codex 任務提供教學，也涵蓋規劃與審查。

歷史起點是 Anthropic 的 [explanatory-output-style 外掛](https://github.com/anthropics/claude-code/tree/main/plugins/explanatory-output-style)。外掛透過 `SessionStart` 鉤子注入指令，其 README 仍稱 Explanatory 已棄用；請以現行 Claude 文件判斷目前行為。

實作依據 OpenAI 的[技能文件](https://learn.chatgpt.com/docs/build-skills)與[技能設計指引](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra)。來源查核日期為 2026-10-17。

## 貢獻與驗證

檔案結構、修改慣例與驗證案例請參閱[貢獻指南](AGENTS.md)。請維持雙語 README 與 `SKILL.md`、`agents/openai.yaml` 的內容一致。

修改文件時，請檢查 Markdown 連結與範例，並執行 `git diff --check` 檢查空白錯誤。修改行為時，請依貢獻指南與 OpenAI 的[技能評估指引](https://developers.openai.com/blog/eval-skills)進行比較驗證。分別記錄技能載入證據、輸出格式與任務結果。

本儲存庫沒有自動化測試套件或覆蓋率門檻。少量試跑只能提供初步證據，不能證明技能在不同模型或版本間的可靠性。
