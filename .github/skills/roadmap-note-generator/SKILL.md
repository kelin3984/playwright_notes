---
name: roadmap-note-generator
description: 根據 Playwright 學習 roadmap 檔案，為指定章節生成完整的繁體中文 markdown 學習筆記，並寫入對應的 README.md。當使用者要求「根據 roadmap 生成章節筆記」、「幫我寫 Chapter X 的筆記」時使用此 skill。
argument-hint: <章節名稱，例如：💫Chapter 2 — Locator 與 DOM 操作> <輸出檔案，例如：Chapter_2—Locator_與_DOM_操作/README.md>
disable-model-invocation: true
---

# Roadmap Note Generator

## 用途

根據 `roadmap/playwright-learning-roadmap-arranged.md` 中的指定章節大綱，
展開生成一份完整的繁體中文 Playwright 學習筆記，並寫入指定的 README.md。

---

## 使用方式

在 Copilot Chat 輸入：

```
/roadmap-note-generator 💫Chapter 2 — Locator 與 DOM 操作 Chapter_2—Locator_與_DOM_操作/README.md
```

第一個參數為 **章節名稱**（需與 roadmap 中的標題完全一致）。  
第二個參數為 **輸出 README 路徑**（相對於 workspace 根目錄）。

---

## 執行步驟

1. **讀取 roadmap**  
   開啟 `roadmap/playwright-learning-roadmap-arranged.md`，找到目標章節（`# 💫Chapter X`）直到下一個 `# 💫Chapter` 為止，抓取：
   - 學習目標（`🧩學習目標`）
   - 所有子主題（`🍀 XXXX - 主題名稱`）及其說明內容

2. **生成筆記內容**  
   依照下方「筆記結構」展開完整內容。

3. **寫入輸出檔案**  
   將生成的 markdown 完整覆寫（或建立）至指定的 README.md。

---

## 筆記結構

每份筆記必須包含以下區塊，**順序不可變更**：

### 1. 頁面標題與導言
```markdown
# 💫 Chapter X — 章節名稱

> 一句話說明本章節的學習目的。

&nbsp;
```

### 2. 目錄（Table of Contents）
列出所有 `🍀 XXXX` 子主題的錨點連結，並包含各子主題的重要小節。

### 3. 每個子主題區塊（`## 🍀 XXXX - 主題名稱`）

每個子主題必須包含：
- **核心觀念說明**：用繁體中文解釋概念，可搭配表格
- **API 說明**（如適用）：列出相關 API 名稱、參數、說明
- **實際範例**：可執行的 TypeScript 程式碼，使用真實前端情境（電商、登入表單、SPA 等）
- **最佳實踐**：條列式，具體可執行的建議
- **常見錯誤**：實際會犯的錯誤 + 正確做法對比

### 4. 文件結尾
```markdown
---

&nbsp;
```

---

## 風格規範

| 項目 | 規範 |
|------|------|
| 語言 | 繁體中文（技術術語保留英文原文） |
| 程式碼語言 | 優先使用 TypeScript |
| 程式碼情境 | 真實前端場景（登入、表單、列表、導航等） |
| 標題層級 | `#` 頁面標題 → `##` 子主題 → `###` 小節，層級清晰不跳號 |
| 表格 | 比較型資訊一律用 markdown 表格呈現 |
| 程式碼區塊 | 每個範例必須有語言標記（\`\`\`typescript 或 \`\`\`bash） |
| 空行 | 區塊間使用 `&nbsp;` 搭配空行增加可讀性 |
| 程式碼可執行性 | 所有範例需完整、可直接複製執行，不得使用 `// ...` 省略 |

### 參考風格範例

請參考以下既有筆記的風格（作為輸出格式基準）：

- [Chapter 0 筆記](../../Chapter_0—Playwright_與測試基礎觀念/README.md)
- [Chapter 1 筆記](../../Chapter_1—Playwright_基本操作/README.md)

---

## 注意事項

- 若輸出的 README.md 已存在內容，**完整覆寫**，不要只附加在末尾
- 生成完成後，回報「已生成 Chapter X 筆記，共 N 個子主題」
- 若 roadmap 中的章節子主題說明過於簡略，根據 Playwright 官方文件知識合理展開
