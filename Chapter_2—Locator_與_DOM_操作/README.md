# 💫 Chapter 2 — Locator 與 DOM 操作

> 掌握 Playwright 最核心的元素定位能力，學會用語意化的方式找到頁面元素並進行可靠的 DOM 操作。

&nbsp;

## 目錄

- [💫 Chapter 2 — Locator 與 DOM 操作](#-chapter-2--locator-與-dom-操作)
  - [目錄](#目錄)
  - [🍀 0201 - Locator 概念](#-0201---locator-概念)
    - [核心觀念](#核心觀念)
    - [Locator vs querySelector](#locator-vs-queryselector)
    - [實際範例](#實際範例)
    - [最佳實踐](#最佳實踐)
    - [常見錯誤](#常見錯誤)
  - [🍀 0202 - getByRole](#-0202---getbyrole)
    - [核心觀念](#核心觀念-1)
    - [API 說明](#api-說明)
    - [常用 ARIA Role 表](#常用-aria-role-表)
    - [實際範例](#實際範例-1)
    - [最佳實踐](#最佳實踐-1)
    - [常見錯誤](#常見錯誤-1)
  - [🍀 0203 - getByText](#-0203---getbytext)
    - [核心觀念](#核心觀念-2)
    - [API 說明](#api-說明-1)
    - [實際範例](#實際範例-2)
    - [最佳實踐](#最佳實踐-2)
    - [常見錯誤](#常見錯誤-2)
  - [🍀 0204 - getByLabel](#-0204---getbylabel)
    - [核心觀念](#核心觀念-3)
    - [API 說明](#api-說明-2)
    - [實際範例](#實際範例-3)
    - [最佳實踐](#最佳實踐-3)
    - [常見錯誤](#常見錯誤-3)
  - [🍀 0205 - getByPlaceholder](#-0205---getbyplaceholder)
    - [核心觀念](#核心觀念-4)
    - [API 說明](#api-說明-3)
    - [實際範例](#實際範例-4)
    - [最佳實踐](#最佳實踐-4)
    - [常見錯誤](#常見錯誤-4)
  - [🍀 0206 - getByTestId](#-0206---getbytestid)
    - [核心觀念](#核心觀念-5)
    - [API 說明](#api-說明-4)
    - [實際範例](#實際範例-5)
    - [最佳實踐](#最佳實踐-5)
    - [常見錯誤](#常見錯誤-5)
  - [🍀 0207 - locator chaining](#-0207---locator-chaining)
    - [核心觀念](#核心觀念-6)
    - [API 說明](#api-說明-5)
    - [實際範例](#實際範例-6)
    - [最佳實踐](#最佳實踐-6)
    - [常見錯誤](#常見錯誤-6)
  - [🍀 0208 - strict mode](#-0208---strict-mode)
    - [核心觀念](#核心觀念-7)
    - [API 說明](#api-說明-6)
    - [實際範例](#實際範例-7)
    - [最佳實踐](#最佳實踐-7)
    - [常見錯誤](#常見錯誤-7)
  - [🍀 0209 - Shadow DOM](#-0209---shadow-dom)
    - [核心觀念](#核心觀念-8)
    - [API 說明](#api-說明-7)
    - [實際範例](#實際範例-8)
    - [最佳實踐](#最佳實踐-8)
    - [常見錯誤](#常見錯誤-8)
  - [🍀 0210 - iframe](#-0210---iframe)
    - [核心觀念](#核心觀念-9)
    - [API 說明](#api-說明-8)
    - [實際範例](#實際範例-9)
    - [最佳實踐](#最佳實踐-9)
    - [常見錯誤](#常見錯誤-9)
  - [🍀 0211 - 最佳 selector 策略](#-0211---最佳-selector-策略)
    - [核心觀念](#核心觀念-10)
    - [Selector 優先序表](#selector-優先序表)
    - [實際範例](#實際範例-10)
    - [最佳實踐](#最佳實踐-10)
    - [常見錯誤](#常見錯誤-10)

---

&nbsp;

## 🍀 0201 - Locator 概念

### 核心觀念

Locator 是 Playwright 用來描述「如何找到頁面元素」的物件。它與傳統的 `querySelector` 有本質上的不同：

```txt
Locator != querySelector
```

Locator 的核心特性：

| 特性 | 說明 |
|------|------|
| **Lazy（延遲求值）** | 建立 Locator 時**不會**立即查詢 DOM，只有在執行動作時才真正搜尋元素 |
| **Auto-retry（自動重試）** | 元素不存在或不可互動時，Playwright 會持續重試，直到 timeout |
| **Auto-wait（自動等待）** | 點擊前自動等待元素出現、可見、穩定（不在動畫中）、可互動 |
| **可組合（Chainable）** | Locator 可以鏈式組合，先縮小範圍再精確定位 |
| **Strict mode** | 若同時匹配多個元素，Playwright 預設拋出錯誤，強迫撰寫精確的定位 |

### Locator vs querySelector

| 面向 | `querySelector` | Playwright Locator |
|------|----------------|--------------------|
| **執行時機** | 立即查詢當前 DOM 快照 | 延遲至動作執行時才查詢 |
| **等待機制** | ❌ 無，需手動 `waitFor` | ✅ 自動等待元素出現 |
| **重試機制** | ❌ 無 | ✅ 自動重試到 timeout |
| **動畫穩定性** | ❌ 不管動畫狀態 | ✅ 等待元素停止移動 |
| **Strict 檢查** | ❌ 靜默返回第一個 | ✅ 多個匹配時報錯（保護測試正確性） |
| **可組合性** | 有限 | ✅ `.filter()` / `.locator()` 任意組合 |

```txt
Auto-wait 等待順序：
① 元素存在於 DOM
② 元素可見（非 display:none / visibility:hidden）
③ 元素穩定（不在 CSS transition 或 animation 中）
④ 元素可互動（非 disabled / 非被其他元素覆蓋）
⑤ 元素啟用（非 readonly）
```

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('Locator 基本概念示範', async ({ page }) => {
  await page.goto('https://demo.playwright.dev/todomvc')

  // 建立 Locator -- 此時尚未查詢 DOM
  const input = page.getByPlaceholder('What needs to be done?')

  // 執行動作時才真正查詢 DOM，並自動等待元素可見、可互動
  await input.fill('購買牛奶')
  await input.press('Enter')

  // 驗證新增的待辦事項
  const todoItem = page.getByRole('listitem').filter({ hasText: '購買牛奶' })
  await expect(todoItem).toBeVisible()

  // Locator 可以反覆使用，每次使用都會重新查詢最新的 DOM
  await input.fill('購買麵包')
  await input.press('Enter')

  await expect(page.getByRole('listitem')).toHaveCount(2)
})
```

### 最佳實踐

- 優先使用語意化的 Locator（`getByRole`、`getByLabel`），而非 CSS selector
- 將常用的 Locator 提取為變數，提高可讀性和維護性
- 不需要手動 `waitFor`，讓 Playwright 的 auto-wait 機制處理時序問題
- 避免在 Locator 建立後立即斷言元素數量，因為 DOM 可能還在更新

### 常見錯誤

**❌ 錯誤：用 `waitForSelector` 手動等待**

```typescript
// 不必要的手動等待
await page.waitForSelector('#submit-btn')
await page.click('#submit-btn')
```

**✅ 正確：直接使用 Locator，auto-wait 會處理等待**

```typescript
// Locator 自動等待元素可見且可互動
await page.getByRole('button', { name: '送出' }).click()
```

---

&nbsp;

## 🍀 0202 - getByRole

### 核心觀念

`getByRole` 是 Playwright 最推薦的定位方式。它依照 **ARIA role** 來尋找元素，與真實使用者（和螢幕閱讀器）看待頁面的方式一致。這讓測試更具語意性，且對 UI 重構（如改變 CSS class 或 DOM 結構）具有高度抗性。

### API 說明

```typescript
page.getByRole(role, options?)
```

| 參數 | 型別 | 說明 |
|------|------|------|
| `role` | `AriaRole` | ARIA role 名稱（字串），如 `'button'`、`'textbox'` |
| `options.name` | `string \| RegExp` | 可存取名稱（Accessible Name），通常為可見文字或 `aria-label` |
| `options.exact` | `boolean` | `name` 是否完全匹配（預設 `false`，部分匹配） |
| `options.checked` | `boolean` | 篩選 checkbox / radio 的勾選狀態 |
| `options.disabled` | `boolean` | 篩選 disabled 狀態的元素 |
| `options.expanded` | `boolean` | 篩選 expanded 狀態（如下拉選單） |
| `options.selected` | `boolean` | 篩選 selected 狀態（如 `<option>`） |
| `options.level` | `number` | 篩選標題層級（`heading` role 專用，如 `level: 2` = `<h2>`） |
| `options.hidden` | `boolean` | 是否包含隱藏元素（預設 `false`，只匹配可見元素） |

### 常用 ARIA Role 表

| HTML 元素 | 對應 ARIA Role | 說明 |
|-----------|---------------|------|
| `<button>` | `button` | 按鈕 |
| `<input type="text">` | `textbox` | 文字輸入框 |
| `<input type="checkbox">` | `checkbox` | 核取方塊 |
| `<input type="radio">` | `radio` | 單選按鈕 |
| `<select>` | `combobox` | 下拉選單 |
| `<a href>` | `link` | 超連結 |
| `<h1>` ~ `<h6>` | `heading` | 標題 |
| `<img alt>` | `img` | 圖片 |
| `<table>` | `table` | 表格 |
| `<li>` | `listitem` | 列表項目 |
| `<ul>` / `<ol>` | `list` | 列表 |
| `<nav>` | `navigation` | 導覽區域 |
| `<main>` | `main` | 主要內容區域 |
| `<dialog>` | `dialog` | 對話框 |
| `<input type="search">` | `searchbox` | 搜尋輸入框 |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('電商登入表單操作', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com/auth/login')

  // 用 role + name 定位輸入框（比 CSS selector 更語意化）
  await page.getByRole('textbox', { name: 'Email address' }).fill('customer@practicesoftwaretesting.com')
  await page.getByRole('textbox', { name: 'Password' }).fill('welcome01')

  // 定位登入按鈕
  await page.getByRole('button', { name: 'Login' }).click()

  // 驗證登入後跳轉
  await expect(page).toHaveURL(/account/)

  // 確認歡迎標題出現
  await expect(page.getByRole('heading', { name: /Hello/i })).toBeVisible()
})

test('導覽選單操作', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // 點擊導覽列連結
  await page.getByRole('link', { name: 'Categories' }).click()

  // 驗證下拉選單出現
  await expect(page.getByRole('list', { name: 'Categories' })).toBeVisible()
})

test('篩選 checkbox 狀態', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // 選取特定 checkbox
  const hammer = page.getByRole('checkbox', { name: 'Hammer' })
  await hammer.check()
  await expect(hammer).toBeChecked()
})
```

### 最佳實踐

- 優先使用 `getByRole`，它是 Playwright 推薦的第一選擇
- 配合 `name` 參數精確鎖定元素，避免只靠 role 造成匹配多個元素
- 使用 `exact: true` 避免部分文字匹配到非預期的元素
- 當 `name` 為動態文字時，改用 `RegExp`：`{ name: /確認/i }`
- `getByRole` 也能測試 UI 的無障礙性（Accessibility），一石二鳥

### 常見錯誤

**❌ 錯誤：只靠 role，沒有指定 name，導致匹配多個按鈕**

```typescript
// 頁面上有多個 button，Playwright 會拋出 strict mode 錯誤
await page.getByRole('button').click()
```

**✅ 正確：搭配 name 精確定位**

```typescript
await page.getByRole('button', { name: '加入購物車' }).click()
```

**❌ 錯誤：用 CSS class 定位按鈕**

```typescript
// class 名稱可能因 CSS module hash 而改變，脆弱
await page.locator('.btn-primary').click()
```

**✅ 正確：用 ARIA role + accessible name**

```typescript
await page.getByRole('button', { name: '結帳' }).click()
```

---

&nbsp;

## 🍀 0203 - getByText

### 核心觀念

`getByText` 透過元素的**可見文字內容**來定位元素。適合用於定位純文字節點（如段落、標籤、提示訊息），但不建議用來定位按鈕或連結（優先用 `getByRole`）。

### API 說明

```typescript
page.getByText(text, options?)
```

| 參數 | 型別 | 說明 |
|------|------|------|
| `text` | `string \| RegExp` | 要匹配的文字內容 |
| `options.exact` | `boolean` | `true` 時完全匹配（含大小寫），`false` 時部分匹配（預設 `false`） |

**匹配規則：**

| 情況 | 行為 |
|------|------|
| `string`（exact: false） | 部分匹配，大小寫不敏感 |
| `string`（exact: true） | 完全匹配，大小寫敏感 |
| `RegExp` | 依 regex flag 決定大小寫敏感性 |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('商品列表文字定位', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // 部分匹配商品名稱
  const product = page.getByText('Hammer')
  await expect(product).toBeVisible()

  // 完全匹配（避免匹配到包含 "Hammer" 的其他文字）
  const exactProduct = page.getByText('Claw Hammer', { exact: true })
  await expect(exactProduct).toBeVisible()

  // 使用 RegExp 忽略大小寫
  const anyHammer = page.getByText(/hammer/i)
  await expect(anyHammer.first()).toBeVisible()
})

test('驗證錯誤提示訊息', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com/auth/login')

  // 送出空表單
  await page.getByRole('button', { name: 'Login' }).click()

  // 驗證錯誤訊息文字
  await expect(page.getByText('Email is required')).toBeVisible()
  await expect(page.getByText('Password is required')).toBeVisible()
})

test('確認成功通知', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com/auth/login')

  await page.getByRole('textbox', { name: 'Email address' }).fill('customer@practicesoftwaretesting.com')
  await page.getByRole('textbox', { name: 'Password' }).fill('welcome01')
  await page.getByRole('button', { name: 'Login' }).click()

  // 確認成功通知（使用正則避免完全比對動態文字）
  await expect(page.getByText(/successfully logged in/i)).toBeVisible()
})
```

### 最佳實踐

- `getByText` 最適合用於驗證訊息文字（成功提示、錯誤訊息、標籤等）
- 若要定位按鈕或連結，優先用 `getByRole`，因為 `getByRole` 會考量 ARIA 語意
- 動態文字（含使用者名稱等變數）使用 `RegExp` 匹配，避免硬編碼失敗
- 使用 `exact: true` 避免文字子集匹配到非預期元素

### 常見錯誤

**❌ 錯誤：用 getByText 定位按鈕，可能匹配到非按鈕的同名文字元素**

```typescript
// 頁面上可能有 <span>確認</span> 和 <button>確認</button>，難以預測匹配哪個
await page.getByText('確認').click()
```

**✅ 正確：按鈕用 getByRole 定位**

```typescript
await page.getByRole('button', { name: '確認' }).click()
```

**❌ 錯誤：對含有子元素的容器用 exact text 完全匹配**

```typescript
// 若容器內有 <strong>重要</strong> 訊息，純文字取出後可能是 "重要訊息" 而非 "重要 訊息"
await page.getByText('重要 訊息', { exact: true })
```

**✅ 正確：改用正則表達式**

```typescript
await page.getByText(/重要.*訊息/)
```

---

&nbsp;

## 🍀 0204 - getByLabel

### 核心觀念

`getByLabel` 透過 `<label>` 元素的文字內容來定位對應的表單欄位。它模擬真實使用者閱讀表單標籤來操作輸入框的行為，是定位表單輸入框最語意化的方式。

**支援的綁定方式：**

| 綁定方式 | HTML 範例 | 說明 |
|----------|-----------|------|
| `for` / `htmlFor` 屬性 | `<label for="email">Email</label><input id="email">` | 傳統 `for-id` 綁定 |
| 包覆式 | `<label>Email <input type="text"></label>` | `<label>` 直接包含 `<input>` |
| `aria-label` | `<input aria-label="Email 地址">` | 無 `<label>` 元素時用 ARIA 屬性 |
| `aria-labelledby` | `<input aria-labelledby="email-label">` | 指向另一個元素的 id |

### API 說明

```typescript
page.getByLabel(text, options?)
```

| 參數 | 型別 | 說明 |
|------|------|------|
| `text` | `string \| RegExp` | `<label>` 的文字內容或 `aria-label` 值 |
| `options.exact` | `boolean` | 是否完全匹配（預設 `false`） |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('使用 getByLabel 操作登入表單', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com/auth/login')

  // 透過 label 文字定位輸入框（最語意化的方式）
  await page.getByLabel('Email address').fill('customer@practicesoftwaretesting.com')
  await page.getByLabel('Password').fill('welcome01')

  await page.getByRole('button', { name: 'Login' }).click()
  await expect(page).toHaveURL(/account/)
})

test('操作帶有 aria-label 的搜尋框', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // 有些輸入框使用 aria-label 而非 <label> 元素
  await page.getByLabel('Search').fill('Hammer')
  await page.getByLabel('Search').press('Enter')

  await expect(page.getByRole('heading', { name: /search results/i })).toBeVisible()
})

test('操作結帳表單多個欄位', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com/checkout')

  // 依各欄位的 label 依序填入
  await page.getByLabel('First name').fill('王')
  await page.getByLabel('Last name').fill('小明')
  await page.getByLabel('Address').fill('台北市信義區信義路五段七號')
  await page.getByLabel('City').fill('台北市')
  await page.getByLabel('Postcode').fill('110')
})
```

### 最佳實踐

- 表單輸入框優先用 `getByLabel`，它最接近真實使用者的操作方式
- 確保前端開發時每個 `<input>` 都有對應的 `<label>`，這對 Accessibility 與可測試性雙重有益
- 若找不到 `<label>`，與前端開發者溝通加上 `aria-label` 屬性
- 使用正則表達式時加上 `i` flag，避免大小寫問題：`/email/i`

### 常見錯誤

**❌ 錯誤：用 placeholder 定位輸入框（脆弱，placeholder 文字常被修改）**

```typescript
await page.locator('input[placeholder="請輸入電子郵件"]').fill('test@example.com')
```

**✅ 正確：用 getByLabel，與語意關聯**

```typescript
await page.getByLabel('電子郵件').fill('test@example.com')
```

**❌ 錯誤：`for` 屬性和 `id` 不匹配導致 getByLabel 找不到元素**

```html
<!-- 錯誤的 HTML：for 與 id 不一致 -->
<label for="user-email">Email</label>
<input id="email" type="text">
```

**✅ 正確 HTML：`for` 與 `id` 完全一致**

```html
<label for="email">Email</label>
<input id="email" type="text">
```

---

&nbsp;

## 🍀 0205 - getByPlaceholder

### 核心觀念

`getByPlaceholder` 透過 `<input>` 或 `<textarea>` 的 `placeholder` 屬性值來定位元素。適用於沒有 `<label>` 的輸入框，是 `getByLabel` 找不到元素時的備選方案。

**注意**：`placeholder` 的主要目的是提示「輸入格式範例」，不應作為欄位標籤。若前端有提供 `<label>`，優先使用 `getByLabel`。

### API 說明

```typescript
page.getByPlaceholder(text, options?)
```

| 參數 | 型別 | 說明 |
|------|------|------|
| `text` | `string \| RegExp` | `placeholder` 屬性的值 |
| `options.exact` | `boolean` | 是否完全匹配（預設 `false`） |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('使用 getByPlaceholder 操作搜尋框', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // 定位 placeholder 為 'Search' 的輸入框
  await page.getByPlaceholder('Search').fill('Pliers')
  await page.getByPlaceholder('Search').press('Enter')

  // 驗證搜尋結果出現
  await expect(page.getByText('Pliers')).toBeVisible()
})

test('操作沒有 label 的留言框', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com/contact')

  // textarea 常見的情境：只有 placeholder 沒有 label
  await page.getByPlaceholder('Your message').fill('請問貴公司是否支援宅配到府？')

  // 搭配其他 locator 填完表單後送出
  await page.getByLabel('First name').fill('王小明')
  await page.getByLabel('Email').fill('wang@example.com')
  await page.getByRole('button', { name: 'Send' }).click()

  await expect(page.getByText(/message sent/i)).toBeVisible()
})

test('部分匹配 placeholder 文字', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com/auth/register')

  // 使用部分匹配（當 placeholder 很長時）
  await page.getByPlaceholder('Your first name').fill('小明')
  await page.getByPlaceholder('Your last name').fill('王')

  // 使用正則
  await page.getByPlaceholder(/email/i).fill('wang@example.com')
})
```

### 最佳實踐

- 只在沒有 `<label>` 時才使用 `getByPlaceholder`
- `placeholder` 文字通常說明輸入格式（如「例：test@email.com」），不要依賴這類提示性文字
- 考慮向前端開發者反映應補上 `<label>` 或 `aria-label`，從根本改善

### 常見錯誤

**❌ 錯誤：有 label 卻用 placeholder 定位**

```typescript
// 即使有 <label>Email</label>，仍用 placeholder 定位（優先序錯誤）
await page.getByPlaceholder('Enter your email').fill('test@example.com')
```

**✅ 正確：優先使用 getByLabel**

```typescript
await page.getByLabel('Email').fill('test@example.com')
```

**❌ 錯誤：完全匹配含範例格式的 placeholder**

```typescript
// placeholder 如 "例：test@email.com" 包含特殊字元，容易打錯
await page.getByPlaceholder('例：test@email.com', { exact: true })
```

**✅ 正確：使用正則或部分匹配**

```typescript
await page.getByPlaceholder(/email/i)
```

---

&nbsp;

## 🍀 0206 - getByTestId

### 核心觀念

`getByTestId` 透過元素上的自定義測試 ID 屬性（預設為 `data-testid`）來定位元素。這是一種「專門為測試設計的錨點」，不受 UI 設計變更影響，非常穩定。

**使用時機**：當語意化定位方式（role、label、text）都無法精確定位，或者頁面有動態生成的複雜結構時，使用 `data-testid` 作為最後手段。

### API 說明

```typescript
page.getByTestId(testId)
```

| 參數 | 型別 | 說明 |
|------|------|------|
| `testId` | `string \| RegExp` | `data-testid` 屬性的值 |

**自訂 testId 屬性名稱**（在 `playwright.config.ts` 中設定）：

```typescript
import { defineConfig } from '@playwright/test'

export default defineConfig({
  use: {
    testIdAttribute: 'data-cy',  // 改用 data-cy 作為 testId 屬性
  },
})
```

常見的 testId 屬性名稱：

| 屬性名 | 常見於 |
|--------|--------|
| `data-testid` | Playwright、Testing Library（預設） |
| `data-cy` | Cypress |
| `data-test` | 通用慣例 |
| `data-qa` | QA 團隊慣例 |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('使用 data-testid 定位商品卡片', async ({ page }) => {
  await page.goto('https://demo-shop.example.com/products')

  // HTML: <div data-testid="product-card-1">...</div>
  const productCard = page.getByTestId('product-card-1')
  await expect(productCard).toBeVisible()

  // 在 testid 容器內繼續定位子元素
  await productCard.getByRole('button', { name: '加入購物車' }).click()

  // HTML: <div data-testid="cart-count">3</div>
  await expect(page.getByTestId('cart-count')).toHaveText('1')
})

test('搭配正則匹配多個 testid', async ({ page }) => {
  await page.goto('https://demo-shop.example.com/products')

  // 匹配所有 data-testid 以 "product-card-" 開頭的元素
  const productCards = page.getByTestId(/^product-card-/)
  await expect(productCards).toHaveCount(10)
})

test('使用自訂 testIdAttribute', async ({ page }) => {
  // 需在 playwright.config.ts 設定 testIdAttribute: 'data-cy'
  await page.goto('https://demo-shop.example.com/login')

  // HTML: <input data-cy="email-input">
  await page.getByTestId('email-input').fill('user@example.com')
  await page.getByTestId('password-input').fill('secret')
  await page.getByTestId('login-button').click()
})
```

### 最佳實踐

- `data-testid` 應視為「測試合約」，前端工程師加上後不要輕易修改屬性值
- 在 `playwright.config.ts` 統一設定 `testIdAttribute`，避免分散管理
- 僅在語意化方式無法滿足需求時才使用 `data-testid`（它繞過了 Accessibility 語意）
- `data-testid` 在 production build 中可以透過 babel plugin 自動移除，不影響終端使用者

### 常見錯誤

**❌ 錯誤：濫用 data-testid 取代所有語意化定位**

```typescript
// 過度依賴 testid，喪失了 Accessibility 測試的價值
await page.getByTestId('submit-btn').click()
await page.getByTestId('email-field').fill('test@example.com')
```

**✅ 正確：優先語意化，data-testid 作為備選**

```typescript
// 語意明確的元素用 role/label
await page.getByLabel('電子郵件').fill('test@example.com')
await page.getByRole('button', { name: '送出' }).click()

// 無法語意化定位的複雜元件才用 testid
await page.getByTestId('date-range-picker').click()
```

**❌ 錯誤：testid 值使用序號，導致 DOM 順序變更時測試失效**

```html
<!-- 脆弱：若商品順序改變，testid 的語意就錯了 -->
<div data-testid="product-1">錘子</div>
<div data-testid="product-2">扳手</div>
```

**✅ 正確：testid 值使用穩定識別子（如商品 slug 或 id）**

```html
<div data-testid="product-hammer">錘子</div>
<div data-testid="product-wrench">扳手</div>
```

---

&nbsp;

## 🍀 0207 - locator chaining

### 核心觀念

Locator chaining（鏈式定位）是指透過組合多個 Locator，逐步縮小定位範圍，最終精確找到目標元素。這在處理重複性 UI（如列表、表格、卡片）時特別有用。

### API 說明

| 方法 | 說明 |
|------|------|
| `locator.locator(selector)` | 在已有的 Locator 範圍內再次定位 |
| `locator.filter(options)` | 篩選匹配特定條件的元素 |
| `locator.filter({ hasText })` | 篩選包含特定文字的元素 |
| `locator.filter({ has })` | 篩選包含特定子 Locator 的元素 |
| `locator.filter({ hasNot })` | 篩選**不**包含特定子 Locator 的元素 |
| `locator.nth(index)` | 取第 N 個（0-based index） |
| `locator.first()` | 取第一個，等同 `.nth(0)` |
| `locator.last()` | 取最後一個 |
| `locator.and(locator)` | 同時滿足兩個 Locator 條件（AND 合併） |
| `locator.or(locator)` | 滿足其中一個 Locator 條件（OR 合併） |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('購物車列表操作（locator chaining）', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // 先定位所有商品卡片
  const productCards = page.getByRole('listitem')

  // 用 filter 找到特定商品
  const hammerCard = productCards.filter({ hasText: 'Claw Hammer' })
  await expect(hammerCard).toHaveCount(1)

  // 在找到的商品卡片內，繼續用 getByRole 找加入購物車的按鈕
  await hammerCard.getByRole('button', { name: 'Add to cart' }).click()

  // 驗證購物車圖示更新
  await expect(page.getByTestId('cart-count')).toHaveText('1')
})

test('表格特定資料列操作', async ({ page }) => {
  await page.goto('https://demo-shop.example.com/admin/orders')

  // 找到包含特定訂單號的資料列
  const targetRow = page.getByRole('row').filter({ hasText: 'ORD-2024-001' })

  // 點擊該列的操作按鈕
  await targetRow.getByRole('button', { name: '查看詳情' }).click()

  await expect(page.getByRole('heading', { name: 'ORD-2024-001' })).toBeVisible()
})

test('篩選含有特定子元素的列表項目', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // 篩選有特定 badge 的商品（用 has 篩選含有特定子 Locator 的父元素）
  const saleItems = page.getByRole('listitem').filter({
    has: page.getByText('Sale'),
  })

  // 取得特價商品數量
  const count = await saleItems.count()
  expect(count).toBeGreaterThan(0)
})

test('用 nth 操作指定位置的元素', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // 取第一個商品（0-based）
  const firstProduct = page.getByRole('listitem').first()
  await firstProduct.getByRole('button', { name: 'Add to cart' }).click()

  // 取第三個商品（index = 2）
  const thirdProduct = page.getByRole('listitem').nth(2)
  await thirdProduct.getByRole('button', { name: 'Add to cart' }).click()

  await expect(page.getByTestId('cart-count')).toHaveText('2')
})

test('用 and 合併兩個定位條件', async ({ page }) => {
  await page.goto('https://demo-shop.example.com/products')

  // 找到同時滿足兩個條件的按鈕（role 為 button，且在 .featured-section 內）
  const featuredButton = page
    .getByRole('button', { name: '加入購物車' })
    .and(page.locator('.featured-section button'))

  await featuredButton.click()
})
```

### 最佳實踐

- 優先用 `.filter({ hasText })` 替代 `.nth()`，因為文字定位比位置索引更穩定
- 避免過深的鏈式組合（超過 3 層），考慮提取為 Page Object 方法
- 使用 `.filter({ has })` 時，內部的 Locator 無需能單獨匹配到元素，只要能描述子元素特徵即可
- 在迴圈操作多個元素時，先用 `.all()` 取得 Locator 陣列再逐一操作

### 常見錯誤

**❌ 錯誤：用 nth(0) 假設第一個就是目標（DOM 順序不穩定）**

```typescript
// 頁面重新排列後，第一個商品可能不是 Hammer
await page.getByRole('listitem').nth(0).getByRole('button', { name: 'Add to cart' }).click()
```

**✅ 正確：用 filter 明確選定目標**

```typescript
await page.getByRole('listitem')
  .filter({ hasText: 'Claw Hammer' })
  .getByRole('button', { name: 'Add to cart' })
  .click()
```

---

&nbsp;

## 🍀 0208 - strict mode

### 核心觀念

Playwright 的 **strict mode**（嚴格模式）是指：當一個 Locator 同時匹配到**多個元素**時，執行互動動作（如 `click()`、`fill()`）或單一元素斷言時，Playwright 會拋出錯誤。

```txt
嚴格模式的意義：
  強迫撰寫精確的定位器，避免「剛好操作到第一個元素」的僥倖心態。
  這讓測試更可靠，問題更早暴露。
```

| 方法類型 | Strict Mode 是否啟用 |
|----------|---------------------|
| `click()`, `fill()`, `check()` 等互動方法 | ✅ 啟用（多個元素時報錯） |
| `expect(locator).toBeVisible()` 等斷言 | ✅ 啟用 |
| `locator.count()` | ❌ 不啟用（回傳數量） |
| `locator.all()` | ❌ 不啟用（回傳陣列） |
| `locator.first()` / `.nth()` / `.last()` | ❌ 不啟用（明確指定索引） |

### API 說明

```typescript
// 得知匹配到幾個元素
const count = await locator.count()

// 取得所有匹配元素的 Locator 陣列（可逐一操作）
const items = await locator.all()

// 確保只有一個元素（回傳 ElementHandle，不建議用於互動）
// 上述兩個方法都是繞過 strict mode 的合法方式
```

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('strict mode 錯誤示範與解決', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // ❌ 這會拋出 strict mode 錯誤（頁面上有多個 listitem）
  // await page.getByRole('listitem').click()

  // ✅ 用 filter 縮小到唯一元素
  await page.getByRole('listitem')
    .filter({ hasText: 'Claw Hammer' })
    .click()
})

test('使用 count() 確認元素數量', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // count() 不受 strict mode 限制
  const itemCount = await page.getByRole('listitem').count()
  expect(itemCount).toBeGreaterThan(0)
  console.log(`找到 ${itemCount} 個商品`)
})

test('用 all() 批次操作多個元素', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // all() 回傳所有匹配元素的 Locator 陣列
  const products = await page.getByRole('listitem').all()

  for (const product of products) {
    const name = await product.getByRole('heading').textContent()
    console.log('商品名稱：', name)
  }
})

test('操作前先確認唯一性', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // 先 count() 確認
  const targetLocator = page.getByRole('listitem').filter({ hasText: 'Hammer' })
  const count = await targetLocator.count()

  if (count === 1) {
    await targetLocator.getByRole('button', { name: 'Add to cart' }).click()
  } else {
    // 多個匹配：進一步縮小範圍
    await targetLocator
      .filter({ hasText: 'Claw Hammer' })
      .getByRole('button', { name: 'Add to cart' })
      .click()
  }
})
```

### 最佳實踐

- 遇到 strict mode 錯誤時，不要急著用 `.first()` 繞過，先思考定位器是否夠精確
- 需要操作多個元素時，使用 `.all()` 取得陣列後逐一處理
- 保留 strict mode 的保護，它能讓你在 CI 提早發現頁面結構變化
- 若確實需要操作第一個元素，至少用 `.filter()` 確認語意，再搭配 `.first()`

### 常見錯誤

**❌ 錯誤：用 .first() 忽略 strict mode 警告，掩蓋問題**

```typescript
// 錯誤示範：用 .first() 強行通過，但不清楚到底點到哪個按鈕
await page.getByRole('button', { name: '編輯' }).first().click()
```

**✅ 正確：先縮小範圍至唯一元素**

```typescript
// 先定位到特定資料列，再找該列的編輯按鈕
await page.getByRole('row')
  .filter({ hasText: '訂單 #001' })
  .getByRole('button', { name: '編輯' })
  .click()
```

---

&nbsp;

## 🍀 0209 - Shadow DOM

### 核心觀念

Shadow DOM 是 Web Components 的一部分，它允許元素擁有獨立、封閉的 DOM 樹。傳統的 `querySelector` 無法穿透 Shadow DOM 邊界，但 **Playwright 的 Locator 預設自動穿透 Shadow DOM**，不需要任何特殊處理。

```txt
傳統 querySelector：
  document.querySelector('#shadow-host').shadowRoot.querySelector('button')
  → 需要手動展開每一層 Shadow Root

Playwright Locator：
  page.getByRole('button', { name: '送出' })
  → 自動穿透所有 Shadow Root，直接找到目標元素
```

**常見使用 Shadow DOM 的技術：**

| 技術 | 說明 |
|------|------|
| Web Components / Custom Elements | 原生瀏覽器自訂元件 |
| Material Web (MWC) | Google Material Design Web Components |
| Lightning Web Components (LWC) | Salesforce 前端框架 |
| Lit | Google 的 Web Components 輕量框架 |
| 瀏覽器內建元件 | `<input type="date">`、`<video controls>` 等 |

### API 說明

Playwright 會**自動**穿透 Shadow DOM，大多數情況下不需要額外操作。

若需要明確定位 Shadow Root 內的元素，可以使用：

```typescript
// 自動穿透（推薦，大部分情況適用）
page.getByRole('button', { name: 'Submit' })

// 明確使用 CSS 穿透語法（較少用）
page.locator('my-component >> button')

// 先定位 host 元素，再在其內部定位（自動穿透仍有效）
page.locator('my-form-component').getByRole('button', { name: 'Submit' })
```

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('操作 Material Web 元件（Shadow DOM）', async ({ page }) => {
  // 假設頁面使用 <mwc-button> 等 Web Components
  await page.goto('https://demo-webcomponents.example.com/login')

  // Playwright 自動穿透 Shadow DOM，與一般元素操作無異
  await page.getByLabel('Email').fill('user@example.com')
  await page.getByLabel('Password').fill('password123')
  await page.getByRole('button', { name: 'Sign In' }).click()

  await expect(page.getByText('Welcome back!')).toBeVisible()
})

test('操作巢狀 Shadow DOM', async ({ page }) => {
  await page.goto('https://demo-webcomponents.example.com/checkout')

  // 即使 Shadow DOM 多層巢狀，Playwright 仍能找到元素
  const cardNumber = page.getByLabel('Card Number')
  await cardNumber.fill('4111 1111 1111 1111')

  const expiry = page.getByLabel('Expiry Date')
  await expiry.fill('12/28')

  const cvv = page.getByLabel('CVV')
  await cvv.fill('123')

  await page.getByRole('button', { name: 'Pay Now' }).click()
  await expect(page.getByText(/payment successful/i)).toBeVisible()
})

test('驗證 Shadow DOM 內的文字', async ({ page }) => {
  await page.goto('https://demo-webcomponents.example.com/profile')

  // 斷言 Shadow DOM 內的元素屬性
  const avatar = page.locator('user-avatar')
  await expect(avatar.getByRole('img')).toHaveAttribute('alt', '使用者頭像')

  // 取得 Shadow DOM 內的文字
  const usernameDisplay = page.locator('user-profile').getByTestId('username')
  await expect(usernameDisplay).toHaveText('王小明')
})
```

### 最佳實踐

- 優先使用 Playwright 的自動穿透特性，不需要手動展開 `shadowRoot`
- 若要加強 Shadow DOM 穿透的可靠性，在定位時從 Shadow Host 元素開始縮小範圍
- 遇到 Shadow DOM 定位問題時，先用 Playwright Inspector（`--ui` 模式）觀察元素結構

### 常見錯誤

**❌ 錯誤：用 `evaluate` 手動展開 shadowRoot（繁瑣且不必要）**

```typescript
// 不必要的複雜做法
const button = await page.evaluate(() => {
  const host = document.querySelector('my-form')
  return host?.shadowRoot?.querySelector('button[type="submit"]')
})
await button?.click()
```

**✅ 正確：直接用 Playwright Locator 自動穿透**

```typescript
await page.locator('my-form').getByRole('button', { name: 'Submit' }).click()
```

---

&nbsp;

## 🍀 0210 - iframe

### 核心觀念

`iframe` 是嵌入在主頁面中的獨立頁面，擁有自己的 document。與 Shadow DOM 不同，**Playwright 不會自動穿透 iframe**，需要使用 `frameLocator()` 先切換到 iframe 的 context，才能操作其內部元素。

**常見使用 iframe 的情境：**

| 情境 | 說明 |
|------|------|
| 第三方金流（如 Stripe, PayPal） | 信用卡輸入框通常在獨立 iframe 中 |
| reCAPTCHA | Google 驗證元件 |
| 嵌入式聊天元件 | Intercom, Zendesk 等 |
| YouTube / Vimeo 嵌入 | 影音播放器 |
| 廣告框架 | 第三方廣告單元 |

### API 說明

```typescript
// 透過 selector 定位 iframe，返回 FrameLocator
const frame = page.frameLocator('iframe[name="payment"]')
const frame = page.frameLocator('iframe[src*="stripe"]')
const frame = page.frameLocator('#checkout-frame')

// 在 FrameLocator 內使用所有 Locator 方法
await frame.getByLabel('Card Number').fill('4111 1111 1111 1111')
await frame.getByRole('button', { name: 'Pay' }).click()

// 使用 nth() 定位頁面上多個 iframe 中的指定一個
const secondFrame = page.frameLocator('iframe').nth(1)
```

| 方法 | 說明 |
|------|------|
| `page.frameLocator(selector)` | 定位 iframe，回傳 `FrameLocator` |
| `frameLocator.getByRole()` | 在 iframe 內用 role 定位 |
| `frameLocator.getByLabel()` | 在 iframe 內用 label 定位 |
| `frameLocator.locator()` | 在 iframe 內用 CSS/XPath 定位 |
| `frameLocator.frameLocator()` | 定位 iframe 內的巢狀 iframe |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('操作 Stripe 金流 iframe', async ({ page }) => {
  await page.goto('https://demo-shop.example.com/checkout')

  // 填寫訂單資訊（主頁面）
  await page.getByLabel('Full Name').fill('王小明')
  await page.getByLabel('Email').fill('wang@example.com')

  // 切換到信用卡 iframe（Stripe 的 iframe 通常有 name 或特定 src）
  const stripeFrame = page.frameLocator('iframe[name="__privateStripeFrame"]')

  // 在 iframe 內操作信用卡欄位
  await stripeFrame.getByPlaceholder('Card number').fill('4242 4242 4242 4242')
  await stripeFrame.getByPlaceholder('MM / YY').fill('12 / 28')
  await stripeFrame.getByPlaceholder('CVC').fill('123')

  // 回到主頁面點擊送出
  await page.getByRole('button', { name: '確認付款' }).click()

  await expect(page.getByText(/付款成功/i)).toBeVisible()
})

test('操作 reCAPTCHA iframe', async ({ page }) => {
  await page.goto('https://demo-shop.example.com/contact')

  // 填寫表單（主頁面）
  await page.getByLabel('Name').fill('王小明')
  await page.getByLabel('Message').fill('測試訊息')

  // 找到 reCAPTCHA iframe 並勾選「我不是機器人」
  const recaptchaFrame = page.frameLocator('iframe[title="reCAPTCHA"]')
  await recaptchaFrame.getByRole('checkbox', { name: /not a robot/i }).click()

  // 等待 reCAPTCHA 驗證通過
  await expect(recaptchaFrame.getByRole('checkbox')).toBeChecked()

  await page.getByRole('button', { name: 'Send' }).click()
})

test('操作巢狀 iframe', async ({ page }) => {
  await page.goto('https://demo-shop.example.com/embed')

  // 第一層 iframe
  const outerFrame = page.frameLocator('#checkout-container')

  // 第二層 iframe（在第一層 iframe 內）
  const innerFrame = outerFrame.frameLocator('iframe[name="card-input"]')

  await innerFrame.getByLabel('Card Number').fill('4111 1111 1111 1111')
})
```

### 最佳實踐

- 使用 `iframe[name]` 或 `iframe[title]` 定位 iframe，比 `iframe:nth-child(2)` 更穩定
- 第三方 iframe（如 Stripe）的內部結構可能隨版本改變，盡量使用語意化定位
- 若 iframe 是動態載入的，善用 Playwright 的 auto-wait，不需要手動 `waitForSelector`

### 常見錯誤

**❌ 錯誤：試圖直接在主頁面定位 iframe 內的元素**

```typescript
// iframe 內的元素無法從主頁面直接定位
await page.getByLabel('Card Number').fill('4242 4242 4242 4242')
// → 找不到元素，因為 Card Number 在 iframe 內
```

**✅ 正確：先用 frameLocator 切換到 iframe context**

```typescript
const frame = page.frameLocator('iframe[name="card-frame"]')
await frame.getByLabel('Card Number').fill('4242 4242 4242 4242')
```

**❌ 錯誤：用 page.frame() 取得 Frame 物件後操作（舊 API，較繁瑣）**

```typescript
// 舊式做法：需先等待 frame 載入，再轉成 FrameLocator
const frame = page.frame({ name: 'payment' })
await frame?.fill('#card-number', '4242 4242 4242 4242')
```

**✅ 正確：直接使用 frameLocator（新 API，支援 auto-wait）**

```typescript
await page.frameLocator('iframe[name="payment"]')
  .getByLabel('Card Number')
  .fill('4242 4242 4242 4242')
```

---

&nbsp;

## 🍀 0211 - 最佳 selector 策略

### 核心觀念

Playwright 提供多種定位方式，但並非所有方式都同等穩定。選擇正確的 selector 策略是寫出可維護測試的關鍵。

**核心原則：**

```txt
好的 selector 應該：
① 反映使用者如何感知並操作元素（語意化）
② 對 UI 視覺重構具有抗性（不依賴 CSS class / DOM 結構）
③ 在 DOM 中唯一（避免 strict mode 錯誤）
④ 當功能改變時，selector 才需要改變（而非 UI 美化時就壞掉）
```

### Selector 優先序表

| 優先序 | 方法 | 適用情境 | 穩定性 |
|--------|------|----------|--------|
| ⭐⭐⭐⭐⭐ | `getByRole` | 按鈕、連結、輸入框、標題、列表等有 ARIA role 的元素 | 最高 |
| ⭐⭐⭐⭐⭐ | `getByLabel` | 表單輸入框（有對應 label） | 最高 |
| ⭐⭐⭐⭐ | `getByPlaceholder` | 表單輸入框（無 label，有 placeholder） | 高 |
| ⭐⭐⭐⭐ | `getByText` | 靜態文字、錯誤訊息、標籤文字 | 高 |
| ⭐⭐⭐⭐ | `getByAltText` | `<img alt>` 圖片 | 高 |
| ⭐⭐⭐ | `getByTestId` | 無法語意化定位的複雜元件 | 中（依賴前端維護 data-testid） |
| ⭐⭐ | `locator('css=...')` | 簡單、穩定的 CSS selector，如 `[name="q"]` | 中低 |
| ⭐ | `locator('xpath=...')` | 無其他方式時的最後手段 | 低 |
| ❌ | `.classList`、`.nth-child`、動態 CSS hash | 絕對避免 | 極低 |

**應避免的 Selector 特徵：**

- 動態生成的 CSS class（如 `class="btn_a1b2c3"`）
- 依賴 DOM 位置（如 `div:nth-child(3) > span`）
- 過長的 XPath（如 `//div[@class='container']//ul/li[2]/a`）
- 包含頁面佈局相關的 class（如 `.col-md-6`, `.flex-container`）

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('最佳 selector 策略完整示範', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com/auth/login')

  // ✅ 優先 1：getByLabel（表單輸入框）
  await page.getByLabel('Email address').fill('customer@practicesoftwaretesting.com')
  await page.getByLabel('Password').fill('welcome01')

  // ✅ 優先 1：getByRole（按鈕）
  await page.getByRole('button', { name: 'Login' }).click()

  // ✅ 優先 4：getByText（驗證提示訊息）
  await expect(page.getByText(/successfully/i)).toBeVisible()
})

test('複雜情境下的 selector 選擇', async ({ page }) => {
  await page.goto('https://practicesoftwaretesting.com')

  // ✅ getByRole（導覽連結）
  await page.getByRole('navigation').getByRole('link', { name: 'Home' }).click()

  // ✅ locator chaining + getByText（商品卡片內定位）
  const productCard = page.getByRole('listitem').filter({ hasText: 'Thor Hammer' })
  await productCard.getByRole('button', { name: 'Add to cart' }).click()

  // ❌ 避免（CSS class 可能因重構改變）：
  // await page.locator('.product-card-title').first().click()

  // ❌ 避免（XPath 複雜且脆弱）：
  // await page.locator('xpath=//div[@class="card"]/div/h5/a').first().click()
})

test('data-testid 作為備選方案', async ({ page }) => {
  await page.goto('https://demo-shop.example.com/dashboard')

  // 對於無法用語意化方式定位的複雜日期選擇器元件，使用 data-testid
  await page.getByTestId('date-range-picker').click()
  await page.getByTestId('date-range-from').fill('2024-01-01')
  await page.getByTestId('date-range-to').fill('2024-12-31')
  await page.getByRole('button', { name: '套用' }).click()

  // 回到語意化定位驗證結果
  await expect(page.getByText('2024 年度報表')).toBeVisible()
})
```

### 最佳實踐

- 建立團隊統一的 selector 優先序規範，並在 code review 時強制執行
- 定期執行測試並觀察哪些 selector 最容易在 UI 重構後失效，作為改善依據
- 考慮引入 ESLint plugin（如 `eslint-plugin-playwright`）提示不佳的 selector 寫法
- 為每個頁面建立 Page Object，將 Locator 集中管理，修改一處即可

### 常見錯誤

**❌ 錯誤：用動態 CSS class 定位**

```typescript
// CSS module 或 utility class 可能因重構或版本升級而改變
await page.locator('.btn_x7k2p').click()
await page.locator('.MuiButton-containedPrimary').click()
```

**✅ 正確：用 role 或 accessible name**

```typescript
await page.getByRole('button', { name: '結帳' }).click()
```

**❌ 錯誤：依賴 DOM 層級結構的 XPath**

```typescript
await page.locator('xpath=//div[2]/ul/li[3]/a').click()
```

**✅ 正確：用語意化定位**

```typescript
await page.getByRole('navigation').getByRole('link', { name: '我的帳戶' }).click()
```

---

&nbsp;
