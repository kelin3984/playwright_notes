# 💫 Chapter 5 — 表單與 UI 自動化

> 學會用 Playwright 穩定模擬真實使用者在表單與介面上的互動。

&nbsp;

## 目錄

- [💫 Chapter 5 — 表單與 UI 自動化](#-chapter-5--表單與-ui-自動化)
  - [目錄](#目錄)
  - [🍀 0501 - click](#-0501---click)
    - [click 的行為模型](#click-的行為模型)
    - [click 常用 API](#click-常用-api)
    - [click 實戰範例：商品加入購物車](#click-實戰範例商品加入購物車)
    - [click 最佳實踐](#click-最佳實踐)
    - [click 常見錯誤](#click-常見錯誤)
  - [🍀 0502 - fill](#-0502---fill)
    - [fill 的適用範圍](#fill-的適用範圍)
    - [fill 常用 API](#fill-常用-api)
    - [fill 實戰範例：登入表單](#fill-實戰範例登入表單)
    - [fill 最佳實踐](#fill-最佳實踐)
    - [fill 常見錯誤](#fill-常見錯誤)
  - [🍀 0503 - keyboard](#-0503---keyboard)
    - [keyboard 的使用情境](#keyboard-的使用情境)
    - [keyboard 常用 API](#keyboard-常用-api)
    - [keyboard 實戰範例：快捷搜尋與鍵盤導覽](#keyboard-實戰範例快捷搜尋與鍵盤導覽)
    - [keyboard 最佳實踐](#keyboard-最佳實踐)
    - [keyboard 常見錯誤](#keyboard-常見錯誤)
  - [🍀 0504 - select](#-0504---select)
    - [select 與自訂下拉選單的差異](#select-與自訂下拉選單的差異)
    - [select 常用 API](#select-常用-api)
    - [select 實戰範例：配送方式切換](#select-實戰範例配送方式切換)
    - [select 最佳實踐](#select-最佳實踐)
    - [select 常見錯誤](#select-常見錯誤)
  - [🍀 0505 - upload](#-0505---upload)
    - [upload 的核心觀念](#upload-的核心觀念)
    - [upload 常用 API](#upload-常用-api)
    - [upload 實戰範例：會員頭像上傳](#upload-實戰範例會員頭像上傳)
    - [upload 最佳實踐](#upload-最佳實踐)
    - [upload 常見錯誤](#upload-常見錯誤)
  - [🍀 0506 - drag and drop](#-0506---drag-and-drop)
    - [drag and drop 的事件特性](#drag-and-drop-的事件特性)
    - [drag and drop 常用 API](#drag-and-drop-常用-api)
    - [drag and drop 實戰範例：看板排序](#drag-and-drop-實戰範例看板排序)
    - [drag and drop 最佳實踐](#drag-and-drop-最佳實踐)
    - [drag and drop 常見錯誤](#drag-and-drop-常見錯誤)
  - [🍀 0507 - hover](#-0507---hover)
    - [hover 的常見用途](#hover-的常見用途)
    - [hover 常用 API](#hover-常用-api)
    - [hover 實戰範例：使用者選單](#hover-實戰範例使用者選單)
    - [hover 最佳實踐](#hover-最佳實踐)
    - [hover 常見錯誤](#hover-常見錯誤)
  - [🍀 0508 - dialog](#-0508---dialog)
    - [dialog 事件模型](#dialog-事件模型)
    - [dialog 常用 API](#dialog-常用-api)
    - [dialog 實戰範例：刪除確認](#dialog-實戰範例刪除確認)
    - [dialog 最佳實踐](#dialog-最佳實踐)
    - [dialog 常見錯誤](#dialog-常見錯誤)
  - [🍀 0509 - clipboard](#-0509---clipboard)
    - [clipboard 的測試限制](#clipboard-的測試限制)
    - [clipboard 常用 API](#clipboard-常用-api)
    - [clipboard 實戰範例：邀請連結複製](#clipboard-實戰範例邀請連結複製)
    - [clipboard 最佳實踐](#clipboard-最佳實踐)
    - [clipboard 常見錯誤](#clipboard-常見錯誤)
  - [🍀 0510 - file chooser](#-0510---file-chooser)
    - [file chooser 與 upload 的分工](#file-chooser-與-upload-的分工)
    - [file chooser 常用 API](#file-chooser-常用-api)
    - [file chooser 實戰範例：匯入 CSV](#file-chooser-實戰範例匯入-csv)
    - [file chooser 最佳實踐](#file-chooser-最佳實踐)
    - [file chooser 常見錯誤](#file-chooser-常見錯誤)
  - [⚡更多細節](#更多細節)
    - [🍀 0505.01 - upload 測試檔案策略](#-050501---upload-測試檔案策略)
      - [如何準備最小但有代表性的 fixture](#如何準備最小但有代表性的-fixture)
    - [🍀 0506.01 - drag and drop 失敗時的排查順序](#-050601---drag-and-drop-失敗時的排查順序)
      - [建議排查流程](#建議排查流程)
    - [🍀 0508.01 - dialog 與 beforeunload 的差異](#-050801---dialog-與-beforeunload-的差異)
      - [為什麼 `beforeunload` 容易被誤判](#為什麼-beforeunload-容易被誤判)
    - [🍀 0509.01 - clipboard 權限與瀏覽器限制](#-050901---clipboard-權限與瀏覽器限制)
      - [真正要驗證的是哪一層](#真正要驗證的是哪一層)
    - [🍀 0510.01 - file chooser 與隱藏 input 的測試策略](#-051001---file-chooser-與隱藏-input-的測試策略)
      - [什麼時候該測按鈕，什麼時候該測 input](#什麼時候該測按鈕什麼時候該測-input)

---

&nbsp;

## 🍀 0501 - click

`click` 是 UI 自動化最常見的操作，但它不是單純「找到元素就點下去」。Playwright 在點擊前會先檢查元素是否可見、可接收事件、沒有被遮擋，並在需要時自動等待，因此比傳統 `querySelector + element.click()` 穩定很多。

### click 的行為模型

| 階段 | Playwright 會做什麼 | 為什麼重要 |
|------|----------------------|------------|
| 定位元素 | 重新解析 `locator` | 避免抓到過期 DOM |
| 檢查狀態 | 確認元素可見、可互動、已穩定 | 降低動畫或尚未 render 完成導致的失敗 |
| 捲動到視窗 | 必要時自動 scroll into view | 避免點擊到螢幕外元素 |
| 觸發事件 | 發送真實 click 流程 | 更接近使用者行為 |
| 等待結果 | 配合 navigation / re-render | 減少 race condition |

### click 常用 API

| API | 用途 | 常見參數 |
|-----|------|----------|
| `locator.click()` | 點擊元素 | `button`, `clickCount`, `delay`, `force`, `trial` |
| `locator.dblclick()` | 雙擊 | `delay` |
| `locator.check()` | 勾選 checkbox / radio | `force` |
| `locator.uncheck()` | 取消 checkbox | `force` |
| `page.getByRole()` | 用語意方式找可點擊元素 | `name`, `exact` |

```ts
await page.getByRole('button', { name: '加入購物車' }).click()
await page.getByRole('checkbox', { name: '同意條款' }).check()
await page.getByRole('button', { name: '重新命名' }).dblclick()
```

### click 實戰範例：商品加入購物車

```ts
import { test, expect } from '@playwright/test'

test('使用者可以從商品卡片加入購物車', async ({ page }) => {
  await page.goto('https://example.com/products')

  const targetCard = page.getByRole('article', { name: /機械式鍵盤/i })
  await expect(targetCard).toBeVisible()

  await targetCard.getByRole('button', { name: '加入購物車' }).click()

  await expect(page.getByRole('status')).toContainText('已加入購物車')
  await expect(page.getByRole('link', { name: /購物車/i })).toContainText('1')
})
```

### click 最佳實踐

- 優先使用 `getByRole()` 搭配可見名稱，不要先想到 CSS selector。
- 點擊後若畫面應更新，立刻接 `expect()` 驗證結果，不要只做操作不驗證。
- `checkbox` / `radio` 盡量用 `check()`，語意比 `click()` 更清楚。
- 面對 hover menu、toast、loading overlay 時，先驗證畫面狀態再點。
- `force: true` 只在你很確定 UI 設計如此時使用，否則是在掩蓋真實問題。

### click 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| `await page.locator('.btn').click()` | selector 太脆弱，容易被樣式重構影響 | `await page.getByRole('button', { name: '送出' }).click()` |
| 點擊後直接 `waitForTimeout(1000)` | 人工等待不穩定又浪費時間 | 點擊後用 `expect()` 驗證 UI 結果 |
| 所有點擊都加 `force: true` | 會掩蓋遮罩、disabled、動畫等真實問題 | 先排查 actionability 失敗原因 |

---

&nbsp;

## 🍀 0502 - fill

`fill` 用來對輸入欄位直接寫入文字。它會先聚焦欄位、清空原值，再設定新內容，非常適合登入表單、搜尋框、收件資訊等標準 input 操作。

### fill 的適用範圍

| 元素類型 | 適合使用 `fill()` 嗎？ | 說明 |
|----------|------------------------|------|
| `<input type="text">` | ✅ | 最典型情境 |
| `<input type="email">` | ✅ | 也會觸發相關 input 事件 |
| `<textarea>` | ✅ | 多行文字輸入 |
| `contenteditable` 元素 | ⚠️ 視元件實作而定 | 有些框架自訂編輯器需要 `pressSequentially()` |
| 密碼管理器 / OTP UI | ⚠️ | 可能需要鍵盤輸入模擬更真實流程 |

### fill 常用 API

| API | 用途 | 備註 |
|-----|------|------|
| `locator.fill(value)` | 清空後填入內容 | 最常用 |
| `locator.clear()` | 單純清空欄位 | 適合測試 empty state |
| `locator.pressSequentially(text)` | 逐字輸入 | 適合 autocomplete / debounce 搜尋 |
| `expect(locator).toHaveValue(value)` | 驗證輸入內容 | 可 auto-retry |

### fill 實戰範例：登入表單

```ts
import { test, expect } from '@playwright/test'

test('登入表單可以正確提交', async ({ page }) => {
  await page.goto('https://example.com/login')

  const emailInput = page.getByLabel('Email')
  const passwordInput = page.getByLabel('Password')
  const submitButton = page.getByRole('button', { name: '登入' })

  await emailInput.fill('alice@example.com')
  await passwordInput.fill('P@ssw0rd123')

  await expect(emailInput).toHaveValue('alice@example.com')
  await expect(passwordInput).toHaveValue('P@ssw0rd123')

  await submitButton.click()

  await expect(page).toHaveURL(/dashboard/)
  await expect(page.getByRole('heading', { name: '控制台' })).toBeVisible()
})
```

### fill 最佳實踐

- 表單欄位優先用 `getByLabel()`，最符合可維護性與 a11y。
- 若欄位具有 debounce 搜尋或即時驗證，可考慮 `pressSequentially()` 模擬逐字輸入。
- 對重要欄位在送出前補一個 `toHaveValue()`，可快速定位輸入失敗問題。
- 測試驗證訊息時，除了 `fill()` 正常流程，也要測試空值與錯誤格式。
- 若輸入值來自測試資料工廠，避免硬編碼在多個測試中重複出現。

### fill 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| `await page.locator('#email').type('a@b.com')` | `type()` 並非最精簡解法，且舊 API 容易造成誤解 | 標準表單用 `fill()` 即可 |
| 對自訂 rich text editor 直接 `fill()` | 有些編輯器不是原生 input，`fill()` 不一定生效 | 先確認 DOM 結構，必要時改用 `click()` + `keyboard` |
| 只填欄位不驗證 | 欄位可能被 formatter 改寫 | 用 `toHaveValue()` 驗證最終值 |

---

&nbsp;

## 🍀 0503 - keyboard

鍵盤操作對搜尋框、快捷鍵、可及性導覽、表單送出都很重要。很多 UI 問題只會在 `Tab`、`Enter`、`Escape` 等情境下出現，因此 `keyboard` 不只是輔助操作，也是驗證互動邏輯的重要工具。

### keyboard 的使用情境

| 情境 | 常見按鍵 | 範例 |
|------|----------|------|
| 表單送出 | `Enter` | 搜尋框按 Enter 提交 |
| 鍵盤導覽 | `Tab`, `Shift+Tab` | 驗證 focus 順序 |
| 關閉浮層 | `Escape` | modal / dropdown 關閉 |
| 快捷鍵 | `Control+K`, `Meta+K` | 開啟 command palette |
| 選單導航 | `ArrowDown`, `ArrowUp` | 下拉建議清單 |

### keyboard 常用 API

| API | 用途 | 重點 |
|-----|------|------|
| `page.keyboard.press(key)` | 按下一組按鍵 | 支援 `Control+K` 這類組合鍵 |
| `page.keyboard.type(text)` | 逐字輸入文字 | 較接近真實輸入 |
| `page.keyboard.insertText(text)` | 直接插入文字 | 不一定觸發所有按鍵事件 |
| `locator.press(key)` | 對特定元素發送按鍵 | 更聚焦在目前欄位 |

### keyboard 實戰範例：快捷搜尋與鍵盤導覽

```ts
import { test, expect } from '@playwright/test'

test('使用者可用快捷鍵打開搜尋面板並用鍵盤選擇結果', async ({ page, browserName }) => {
  await page.goto('https://example.com/dashboard')

  const shortcut = browserName === 'webkit' ? 'Meta+K' : 'Control+K'
  await page.keyboard.press(shortcut)

  const searchDialog = page.getByRole('dialog', { name: '快速搜尋' })
  await expect(searchDialog).toBeVisible()

  const searchInput = searchDialog.getByRole('textbox', { name: '搜尋關鍵字' })
  await searchInput.fill('訂單')

  await page.keyboard.press('ArrowDown')
  await page.keyboard.press('Enter')

  await expect(page).toHaveURL(/orders/)
  await expect(page.getByRole('heading', { name: '訂單管理' })).toBeVisible()
})
```

### keyboard 最佳實踐

- 快捷鍵需考慮作業系統差異，`Control` 與 `Meta` 常常不同。
- 驗證 focus 流程時，可搭配 `toBeFocused()`，不要只按鍵不驗證。
- 若元件依賴 `keydown` / `keyup`，使用 `press()` 比 `insertText()` 更合適。
- 對 dropdown、command palette 這類鍵盤導向元件，至少測一次完整鍵盤流程。
- a11y 相關案例應納入 `Tab` 與 `Escape` 路徑，而不只測滑鼠。

### keyboard 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 直接 `page.keyboard.press('Enter')` 卻沒先聚焦欄位 | 事件可能發送到錯誤元素 | 先 `click()` 或使用 `locator.press()` |
| 把 `insertText()` 當成真實鍵盤輸入 | 不一定會觸發快捷鍵或 key event | 需要鍵盤事件時用 `press()` 或 `type()` |
| 忽略 Mac / Windows 差異 | CI 與本機行為可能不同 | 依情境切換 `Meta` / `Control` |

---

&nbsp;

## 🍀 0504 - select

`select` 主要對應原生 `<select>` 控件。這類元素不要用 `click()` 模擬打開後再點選 option，而應直接使用 `selectOption()`，可讀性與穩定性都更好。

### select 與自訂下拉選單的差異

| 類型 | 底層元素 | 建議操作 |
|------|----------|----------|
| 原生下拉選單 | `<select>` | `selectOption()` |
| Headless UI / Ant Design Select | `div`, `li`, `button` | `click()` + `getByRole('option')` |
| 自訂 autocomplete | input + listbox | `fill()` / `keyboard` / `click()` |

### select 常用 API

| API | 用途 | 範例 |
|-----|------|------|
| `locator.selectOption(value)` | 以 `value` 選取 | `'home-delivery'` |
| `locator.selectOption({ label })` | 以顯示文字選取 | `{ label: '宅配' }` |
| `locator.selectOption({ index })` | 以索引選取 | `{ index: 2 }` |
| `expect(locator).toHaveValue(value)` | 驗證選取結果 | `'pickup'` |

### select 實戰範例：配送方式切換

```ts
import { test, expect } from '@playwright/test'

test('使用者可以切換配送方式', async ({ page }) => {
  await page.goto('https://example.com/checkout')

  const shippingMethod = page.getByLabel('配送方式')
  await shippingMethod.selectOption({ label: '超商取貨' })

  await expect(shippingMethod).toHaveValue('store-pickup')
  await expect(page.getByText('請選擇取貨門市')).toBeVisible()

  await shippingMethod.selectOption('home-delivery')
  await expect(page.getByPlaceholder('請輸入收件地址')).toBeVisible()
})
```

### select 最佳實踐

- 先確認元件是不是真正的 `<select>`，不要看到下拉 UI 就直接用 `selectOption()`。
- 優先用 `label` 或 `value`，比 `index` 更穩定、可讀。
- 切換選項後要驗證下游影響，例如運費、表單欄位、說明文字是否同步變化。
- 多語系專案若顯示文字會變，建議優先用 `value`。
- 若是多選原生 select，可傳入陣列做驗證與選取。

### select 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 對原生 `<select>` 先 `click()` 再找 option | 實作冗長且跨瀏覽器差異較大 | 直接 `selectOption()` |
| 對自訂下拉元件使用 `selectOption()` | 元件不是 `<select>`，API 不會生效 | 改用對應角色與互動流程 |
| 選完不驗證依賴 UI | 只確認欄位值，沒確認實際業務效果 | 驗證後續區塊是否正確出現 |

---

&nbsp;

## 🍀 0505 - upload

檔案上傳是企業系統常見功能，例如會員頭像、商品圖片、CSV 匯入。Playwright 不需要真的操作作業系統檔案視窗，大多數情況可直接對 `<input type="file">` 使用 `setInputFiles()`。

### upload 的核心觀念

| 觀念 | 說明 |
|------|------|
| 真正的入口通常是 `input[type=file]` | 按鈕外觀常只是包裝 UI |
| `setInputFiles()` 是首選 | 穩定、快速、不依賴原生檔案視窗 |
| 可傳單檔、多檔、記憶體內容 | 適合測試圖片、CSV、匯出流程 |
| 清空上傳可用空陣列 | `setInputFiles([])` |

### upload 常用 API

| API | 用途 | 範例 |
|-----|------|------|
| `locator.setInputFiles(path)` | 上傳單一檔案 | `'fixtures/avatar.png'` |
| `locator.setInputFiles([path1, path2])` | 多檔上傳 | `['a.png', 'b.png']` |
| `locator.setInputFiles({ name, mimeType, buffer })` | 動態建立測試檔 | 適合 CSV / JSON |
| `expect(locator).toHaveJSProperty('files', ...)` | 進階驗證 | 可驗證 `FileList` |

### upload 實戰範例：會員頭像上傳

```ts
import { test, expect } from '@playwright/test'
import path from 'node:path'

test('使用者可以上傳會員頭像', async ({ page }) => {
  await page.goto('https://example.com/profile')

  const avatarInput = page.getByLabel('上傳頭像')
  const filePath = path.join(process.cwd(), 'tests', 'fixtures', 'avatar.png')

  await avatarInput.setInputFiles(filePath)
  await page.getByRole('button', { name: '儲存變更' }).click()

  await expect(page.getByRole('status')).toContainText('頭像更新成功')
  await expect(page.getByAltText('目前頭像')).toHaveAttribute('src', /avatar/)
})
```

### upload 最佳實踐

- 把測試用檔案集中放在 `tests/fixtures/`，避免路徑散落各處。
- 圖片、CSV、PDF 盡量各準備一個最小可用 fixture，減少 repo 膨脹。
- 若 UI 顯示預覽圖、檔名、大小，應在上傳後一併驗證。
- 測試前端驗證時，準備超大檔、錯誤 MIME type、空檔案等反例。
- 如果只是驗證上傳流程，不需要真的打開作業系統的 file dialog。

### upload 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 想用滑鼠操作系統檔案視窗 | Playwright 不直接控制原生檔案總管 | 直接用 `setInputFiles()` |
| 用相對路徑但不確認執行根目錄 | CI 可能找不到檔案 | 用 `path.join(process.cwd(), ...)` 組路徑 |
| 上傳後只驗證成功訊息 | 不足以確認檔案真的被接收 | 再驗證預覽、檔名或 network 結果 |

---

&nbsp;

## 🍀 0506 - drag and drop

拖拉互動比點擊複雜得多，因為它通常牽涉 `dragstart`、`dragover`、`drop` 等事件，部分前端框架還會用滑鼠座標或 pointer event 自行實作。因此 `dragAndDrop()` 很方便，但不是所有 UI 都保證適用。

### drag and drop 的事件特性

| 類型 | 實作方式 | 測試策略 |
|------|----------|----------|
| 原生 HTML Drag and Drop | `dragstart` / `drop` 事件 | 可優先試 `dragAndDrop()` |
| 座標型拖拉 | `mousedown` + `mousemove` + `mouseup` | 可能需滑鼠座標操作 |
| 框架自訂拖拉 | Pointer / sensor API | 需依元件實作調整 |

### drag and drop 常用 API

| API | 用途 | 適用情境 |
|-----|------|----------|
| `locator.dragTo(target)` | 從來源拖到目標 | Locator 層級最直覺 |
| `page.dragAndDrop(source, target)` | 用 selector 拖拉 | 快速腳本可用 |
| `page.mouse.move/down/up()` | 手動座標控制 | 自訂拖拉 UI |
| `expect(locator).toContainText()` | 驗證排序結果 | 不要只做拖拉不驗證 |

### drag and drop 實戰範例：看板排序

```ts
import { test, expect } from '@playwright/test'

test('專案看板任務可從待處理拖到進行中', async ({ page }) => {
  await page.goto('https://example.com/board')

  const todoCard = page.getByRole('article', { name: '串接金流 API' })
  const doingColumn = page.getByRole('region', { name: '進行中' })

  await todoCard.dragTo(doingColumn)

  await expect(doingColumn).toContainText('串接金流 API')
  await expect(page.getByRole('region', { name: '待處理' })).not.toContainText('串接金流 API')
})
```

### drag and drop 最佳實踐

- 先確認元件是原生 drag-and-drop 還是框架自訂拖拉。
- 拖拉完成後應驗證資料順序、欄位變更、後端請求或 toast，不要只驗證畫面動畫。
- 若 `dragTo()` 不穩定，改從 DOM 實作、事件模型、座標系統排查。
- 看板、sortable list、上傳區 dropzone 都建議各有一個代表性測試案例。
- 拖拉很容易受動畫影響，必要時等待容器穩定後再操作。

### drag and drop 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 看到拖拉失敗就直接加 `waitForTimeout()` | 只是在掩蓋實作相容性問題 | 先判斷是事件模型還是座標問題 |
| 拖拉後只看卡片還在畫面上 | 可能其實沒有完成排序 | 驗證目標欄位或順序結果 |
| 對高度客製元件硬用 `dragTo()` | 部分框架並不使用原生 drag 事件 | 改用 `mouse` 或元件公開 API |

---

&nbsp;

## 🍀 0507 - hover

`hover` 常用在顯示 tooltip、下拉選單、快捷操作按鈕。這類互動在桌面版後台很常見，但如果只測點擊，不測 `hover`，很多 UI bug 不會被發現。

### hover 的常見用途

| 情境 | 說明 |
|------|------|
| tooltip | 滑入後顯示提示文字 |
| 操作列 | 滑入卡片才顯示編輯 / 刪除按鈕 |
| 導航選單 | 滑入主選單展開子選單 |
| 商品圖片 | 滑入顯示快速預覽或收藏 |

### hover 常用 API

| API | 用途 | 備註 |
|-----|------|------|
| `locator.hover()` | 滑鼠移入元素 | 最常用 |
| `expect(locator).toBeVisible()` | 驗證 hover 後內容 | 建議搭配使用 |
| `locator.click()` | 對 hover 後出現的元素執行下一步 | 常組合出現 |

### hover 實戰範例：使用者選單

```ts
import { test, expect } from '@playwright/test'

test('滑入使用者頭像後顯示帳號選單', async ({ page }) => {
  await page.goto('https://example.com/app')

  const avatarButton = page.getByRole('button', { name: 'Alice Chen' })
  await avatarButton.hover()

  const userMenu = page.getByRole('menu', { name: '使用者選單' })
  await expect(userMenu).toBeVisible()
  await expect(userMenu.getByRole('menuitem', { name: '登出' })).toBeVisible()

  await userMenu.getByRole('menuitem', { name: '個人設定' }).click()
  await expect(page).toHaveURL(/settings/)
})
```

### hover 最佳實踐

- `hover()` 後一定要驗證 hover 產物是否出現，例如 tooltip、menu、action button。
- 若桌面與手機版 UI 行為不同，測試應明確區分 viewport。
- hover 觸發的元素如果會自動消失，要縮短驗證與點擊的間隔。
- 常與 `hasText`、`getByRole('menu')` 搭配，提升語意性。
- 對 tooltip 類內容，驗證文字時要小心 aria-hidden 或 duplicated text。

### hover 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| `hover()` 後立刻點不存在的元素 | hover menu 可能尚未 render | 先 `expect(menu).toBeVisible()` |
| 手機版流程仍使用 `hover()` | 觸控裝置通常沒有 hover 概念 | 視產品設計改測 click 或 long press |
| 只驗證 tooltip 文字存在於 DOM | 可能實際不可見 | 用 `toBeVisible()` 驗證 |

---

&nbsp;

## 🍀 0508 - dialog

瀏覽器原生 `dialog` 包含 `alert`、`confirm`、`prompt` 與 `beforeunload`。這些對話框不是一般 DOM 元素，因此不能用 `locator` 去抓，而是要監聽 `page.on('dialog')` 或 `page.waitForEvent('dialog')`。

### dialog 事件模型

| 類型 | 特性 | 可做操作 |
|------|------|----------|
| `alert` | 單純提示 | `accept()` |
| `confirm` | 確認 / 取消 | `accept()`、`dismiss()` |
| `prompt` | 可輸入文字 | `accept('text')`、`dismiss()` |
| `beforeunload` | 離頁前確認 | `accept()`、`dismiss()` |

### dialog 常用 API

| API | 用途 | 備註 |
|-----|------|------|
| `page.waitForEvent('dialog')` | 等待下一個 dialog | 常搭配 `Promise.all()` |
| `page.on('dialog', handler)` | 全域監聽 dialog | 適合共用流程 |
| `dialog.accept(promptText?)` | 接受對話框 | `prompt` 可帶文字 |
| `dialog.dismiss()` | 關閉 / 取消對話框 | `confirm` 很常用 |
| `dialog.message()` | 取得訊息文字 | 便於斷言 |

### dialog 實戰範例：刪除確認

```ts
import { test, expect } from '@playwright/test'

test('刪除商品前會出現確認 dialog', async ({ page }) => {
  await page.goto('https://example.com/admin/products')

  const dialogPromise = page.waitForEvent('dialog')
  await page.getByRole('button', { name: '刪除商品' }).click()

  const dialog = await dialogPromise
  expect(dialog.type()).toBe('confirm')
  expect(dialog.message()).toContain('確定要刪除')
  await dialog.accept()

  await expect(page.getByRole('status')).toContainText('商品已刪除')
})
```

### dialog 最佳實踐

- 一律在觸發前先建立 `dialog` 等待，避免錯過事件。
- 驗證 `dialog.message()`，確保不是只關掉視窗而已。
- `prompt` 測試應覆蓋接受與取消兩條路徑。
- 若整個專案多處會出現原生 dialog，可在 fixture 層統一攔截策略。
- 不要把一般 modal 與原生 dialog 混為一談，測法完全不同。

### dialog 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 點擊後才開始 `waitForEvent('dialog')` | 可能已錯過事件，測試卡住 | 先建立等待再觸發 |
| 用 `locator('.dialog')` 去找原生 alert | 原生 dialog 不在 DOM 中 | 改用 `page.waitForEvent('dialog')` |
| 只 `accept()` 不驗證訊息 | 可能誤關錯對話框 | 驗證 `type()` 與 `message()` |

---

&nbsp;

## 🍀 0509 - clipboard

clipboard 功能常出現在「複製邀請連結」、「複製 token」、「複製指令」等場景。它牽涉瀏覽器權限、安全限制與頁面上下文，因此比一般 click / fill 更容易出現環境差異。

### clipboard 的測試限制

| 限制 | 說明 |
|------|------|
| 需要安全上下文 | 多數瀏覽器要求 HTTPS 或 localhost |
| 可能需要使用者手勢 | 有些 API 只能在 click 後呼叫 |
| 不同瀏覽器支援度不同 | 尤其舊環境或受限容器 |
| CI 權限與本機不同 | 本機可過、CI 不一定可過 |

### clipboard 常用 API

| API | 用途 | 備註 |
|-----|------|------|
| `page.evaluate()` | 在頁面上下文操作 `navigator.clipboard` | 最常見 |
| `browserContext.grantPermissions()` | 授權 clipboard 權限 | 視瀏覽器與站點而定 |
| `expect(locator).toContainText()` | 驗證複製成功提示 | 常搭配 toast |

### clipboard 實戰範例：邀請連結複製

```ts
import { test, expect } from '@playwright/test'

test('使用者可以複製邀請連結', async ({ context, page }) => {
  await context.grantPermissions(['clipboard-read', 'clipboard-write'], {
    origin: 'https://example.com',
  })

  await page.goto('https://example.com/team')
  await page.getByRole('button', { name: '複製邀請連結' }).click()

  await expect(page.getByRole('status')).toContainText('連結已複製')

  const clipboardText = await page.evaluate(async () => {
    return await navigator.clipboard.readText()
  })

  expect(clipboardText).toContain('https://example.com/invite/')
})
```

### clipboard 最佳實踐

- 先確認產品是在前端真的寫入 clipboard，還是只是顯示「已複製」訊息。
- 若權限或瀏覽器限制太高，至少驗證 copy handler 被觸發後的 UI 反饋。
- 本機與 CI 若行為不同，應在測試文件中記錄環境前提。
- 對 token / secret 類內容，只驗證格式與存在性，不要在報告中輸出敏感值。
- 若是 fallback 實作（例如 `document.execCommand('copy')`），也要考慮相容性驗證。

### clipboard 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 直接讀 `navigator.clipboard` 卻沒授權 | 測試可能因權限失敗 | 先 `grantPermissions()` |
| 只驗證 toast 訊息就認定複製成功 | UI 與實際 clipboard 內容可能不同步 | 能讀取時再驗證 clipboard 內容 |
| 在不安全來源測 clipboard | API 可能不可用 | 用 `https` 或 `localhost` 測試 |

---

&nbsp;

## 🍀 0510 - file chooser

`file chooser` 指的是點擊「上傳檔案」按鈕後，瀏覽器準備開啟檔案選擇流程的事件。當 UI 將真正的 `<input type="file">` 隱藏起來，只暴露一個按鈕給使用者時，就很適合透過 `filechooser` 事件接管流程。

### file chooser 與 upload 的分工

| 主題 | 主要 API | 適用情境 |
|------|----------|----------|
| upload | `setInputFiles()` | 已經拿到 file input locator |
| file chooser | `page.waitForEvent('filechooser')` | 需要透過按鈕觸發檔案選擇流程 |

### file chooser 常用 API

| API | 用途 | 備註 |
|-----|------|------|
| `page.waitForEvent('filechooser')` | 等待檔案選擇器事件 | 需在觸發前先建立等待 |
| `fileChooser.setFiles()` | 指定要選的檔案 | 支援單檔 / 多檔 |
| `fileChooser.isMultiple()` | 檢查是否允許多選 | 便於驗證 UI 設計 |
| `fileChooser.element()` | 取得底層 input | 進階除錯時可用 |

### file chooser 實戰範例：匯入 CSV

```ts
import { test, expect } from '@playwright/test'
import path from 'node:path'

test('管理員可以透過 file chooser 匯入訂單 CSV', async ({ page }) => {
  await page.goto('https://example.com/admin/orders/import')

  const fileChooserPromise = page.waitForEvent('filechooser')
  await page.getByRole('button', { name: '選擇 CSV 檔案' }).click()

  const fileChooser = await fileChooserPromise
  expect(fileChooser.isMultiple()).toBe(false)

  const csvPath = path.join(process.cwd(), 'tests', 'fixtures', 'orders.csv')
  await fileChooser.setFiles(csvPath)

  await expect(page.getByText('orders.csv')).toBeVisible()
  await page.getByRole('button', { name: '開始匯入' }).click()

  await expect(page.getByRole('status')).toContainText('匯入成功')
})
```

### file chooser 最佳實踐

- 一定要在點擊觸發按鈕前先 `waitForEvent('filechooser')`。
- 若只是一般 file input，上傳測試優先用 `setInputFiles()`；不必每次都走 file chooser。
- 驗證檔名顯示、匯入結果、錯誤訊息，比單純成功提交更有價值。
- 匯入類功能應補負向案例，例如錯誤欄位格式、空白 CSV、過大檔案。
- 若 UI 允許多檔，記得驗證 `isMultiple()` 與多檔處理結果。

### file chooser 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 先點按鈕再等 `filechooser` | 容易錯過事件 | 先建立 promise，再觸發 click |
| 任何上傳流程都強制走 file chooser | 增加測試複雜度 | 有 input locator 時直接用 `setInputFiles()` |
| 匯入後不驗證檔名與結果 | 難確認是否真的選到正確檔案 | 驗證檔名、狀態與後續 UI |

---

&nbsp;

## ⚡更多細節

### 🍀 0505.01 - upload 測試檔案策略

#### 如何準備最小但有代表性的 fixture

建議依檔案類型建立固定測試資源：

| 類型 | 建議檔案 | 用途 |
|------|----------|------|
| 圖片 | `avatar.png` | 頭像、商品圖上傳 |
| 文件 | `policy.pdf` | 合約、附件上傳 |
| 表格 | `orders.csv` | 批次匯入 |
| 錯誤案例 | `invalid.txt` | MIME type 驗證 |

若只需要驗證前端能接收檔案，也可以直接在記憶體建立測試檔，避免 repo 存太多 fixture：

```ts
await page.getByLabel('上傳 CSV').setInputFiles({
  name: 'orders.csv',
  mimeType: 'text/csv',
  buffer: Buffer.from('orderId,amount\nA001,1200\nA002,800', 'utf-8'),
})
```

&nbsp;

### 🍀 0506.01 - drag and drop 失敗時的排查順序

#### 建議排查流程

當 `dragTo()` 失敗時，不要第一時間加等待，先依順序確認：

1. 來源與目標 locator 是否唯一且可見。
2. 元件是否真的使用原生 drag event。
3. 拖拉結果是否需要進入特定區域或排序位置。
4. 是否有動畫、sticky header、scroll 容器影響座標。
5. 是否應改用 `page.mouse` 進行更細粒度操作。

```ts
const source = page.getByRole('article', { name: '任務 A' })
const target = page.getByRole('article', { name: '任務 B' })

const sourceBox = await source.boundingBox()
const targetBox = await target.boundingBox()

if (!sourceBox || !targetBox) {
  throw new Error('拖拉目標不可見，無法取得座標')
}

await page.mouse.move(sourceBox.x + sourceBox.width / 2, sourceBox.y + sourceBox.height / 2)
await page.mouse.down()
await page.mouse.move(targetBox.x + targetBox.width / 2, targetBox.y + targetBox.height / 2, { steps: 10 })
await page.mouse.up()
```

&nbsp;

### 🍀 0508.01 - dialog 與 beforeunload 的差異

#### 為什麼 `beforeunload` 容易被誤判

`beforeunload` 雖然也屬於 `dialog`，但它通常不是按某個按鈕就立即彈出，而是與重新整理、關閉分頁、離開頁面有關。測試時通常會配合 `page.reload()`、`page.goto()` 或 `page.close({ runBeforeUnload: true })`。

```ts
const dialogPromise = page.waitForEvent('dialog')
await page.reload()

const dialog = await dialogPromise
expect(dialog.type()).toBe('beforeunload')
await dialog.dismiss()
```

如果產品有「表單尚未儲存」保護機制，這類案例值得獨立成一個測試，不要和一般 `confirm` 混在一起。

&nbsp;

### 🍀 0509.01 - clipboard 權限與瀏覽器限制

#### 真正要驗證的是哪一層

clipboard 測試可以分三層：

| 層級 | 驗證內容 | 穩定度 |
|------|----------|--------|
| UI 層 | 是否出現「已複製」提示 | 高 |
| 應用層 | 是否呼叫 copy handler / fallback 邏輯 | 中 |
| 瀏覽器層 | 是否真的寫入系統 clipboard | 視環境而定 |

在 CI 不穩定時，可以保留 UI 層為 smoke test，另用較少量的瀏覽器層測試驗證真正 clipboard 行為，避免整套測試因環境權限波動而變 flaky。

&nbsp;

### 🍀 0510.01 - file chooser 與隱藏 input 的測試策略

#### 什麼時候該測按鈕，什麼時候該測 input

如果你的產品重點是「點擊按鈕後流程正確打開選檔機制」，就應測 `filechooser`。如果重點只是「檔案被成功附加並送出」，直接對 `input[type=file]` 設檔案會更精簡。

| 測試目的 | 建議方法 |
|----------|----------|
| 驗證按鈕有正確綁定隱藏 input | `waitForEvent('filechooser')` |
| 驗證上傳業務流程 | `setInputFiles()` |
| 驗證多檔與檔名 UI 呈現 | 兩者皆可，視元件設計決定 |

對於設計系統封裝的 upload component，建議至少保留一個 `filechooser` 測試，確保使用者入口沒有被樣式或重構破壞。

---

&nbsp;
