# 如何使用 roadmap-note-generator SKILL

## 使用方式

使用方式： 在 Copilot Chat 輸入 `/`，從選單選取 `roadmap-note-generator`，或直接輸入：

```bash
/roadmap-note-generator 💫Chapter 2 — Locator 與 DOM 操作 Chapter_2—Locator_與_DOM_操作/README.md

# 說明
/roadmap-note-generator <章節名稱> <輸出 README 路徑>
第一個參數為 **章節名稱**（需與 roadmap 中的標題完全一致）。
第二個參數為 **輸出 README 路徑**（相對於 workspace 根目錄）。
```

## 說明
結構符合官方規格：
- 目錄名稱 `roadmap-note-generator` 與 frontmatter `name` 完全一致
- `disable-model-invocation: true` —— 只在手動輸入 `/roadmap-note-generator` 時觸發，不會自動載入
- `argument-hint` 顯示 slash command 提示文字