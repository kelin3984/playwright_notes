# 💫 Chapter 3 — Assertion 與驗證

> 學會真正「驗證」頁面狀態，讓測試有意義、有信心。

&nbsp;

## 目錄

- [💫 Chapter 3 — Assertion 與驗證](#-chapter-3--assertion-與驗證)
  - [目錄](#目錄)
  - [🍀 0301 - expect](#-0301---expect)
    - [兩種 Assertion 類型](#兩種-assertion-類型)
    - [常用 expect API 一覽](#常用-expect-api-一覽)
    - [基礎範例](#基礎範例)
    - [最佳實踐](#最佳實踐)
    - [常見錯誤](#常見錯誤)
  - [🍀 0302 - toBeVisible](#-0302---tobevisible)
    - [可見性驗證觀念](#可見性驗證觀念)
    - [可見性 API 比較](#可見性-api-比較)
    - [實際範例](#實際範例)
    - [最佳實踐](#最佳實踐-1)
    - [常見錯誤](#常見錯誤-1)
  - [🍀 0303 - toContainText](#-0303---tocontaintext)
    - [文字匹配觀念](#文字匹配觀念)
    - [toContainText vs toHaveText](#tocontaintext-vs-tohavetext)
    - [實際範例](#實際範例-1)
    - [最佳實踐](#最佳實踐-2)
    - [常見錯誤](#常見錯誤-2)
  - [🍀 0304 - toHaveValue](#-0304---tohavevalue)
    - [表單值驗證觀念](#表單值驗證觀念)
    - [各元素類型對應 API](#各元素類型對應-api)
    - [實際範例](#實際範例-2)
    - [最佳實踐](#最佳實踐-3)
    - [常見錯誤](#常見錯誤-3)
  - [🍀 0305 - toHaveURL](#-0305---tohaveurl)
    - [URL 驗證觀念](#url-驗證觀念)
    - [URL 匹配方式比較](#url-匹配方式比較)
    - [實際範例](#實際範例-3)
    - [最佳實踐](#最佳實踐-4)
    - [常見錯誤](#常見錯誤-4)
  - [🍀 0306 - auto retry](#-0306---auto-retry)
    - [自動重試機制觀念](#自動重試機制觀念)
    - [有重試 vs 無重試的 API 對比](#有重試-vs-無重試的-api-對比)
    - [timeout 設定](#timeout-設定)
    - [實際範例](#實際範例-4)
    - [最佳實踐](#最佳實踐-5)
    - [常見錯誤](#常見錯誤-5)
  - [🍀 0307 - 軟斷言 vs 硬斷言](#-0307---軟斷言-vs-硬斷言)
    - [斷言模式觀念](#斷言模式觀念)
    - [硬斷言 vs 軟斷言比較](#硬斷言-vs-軟斷言比較)
    - [實際範例](#實際範例-5)
    - [最佳實踐](#最佳實踐-6)
    - [常見錯誤](#常見錯誤-6)

---

&nbsp;

## 🍀 0301 - expect

### 兩種 Assertion 類型

Playwright 的 `expect` 分為兩大類型：

| 類型 | 說明 | 範例 |
|------|------|------|
| **Locator Assertion** | 對頁面元素進行非同步驗證，**支援自動重試** | `await expect(locator).toBeVisible()` |
| **Value Assertion** | 對一般 JavaScript 值進行同步驗證，**不會重試** | `expect(value).toBe(123)` |

> **核心觀念**：在 Playwright 的 E2E 測試中，**絕大多數情況應使用 Locator Assertion**，因為頁面狀態是非同步變化的，自動重試機制才能讓測試穩定不 flaky。

### 常用 expect API 一覽

#### Locator Assertions（需 `await`）

| API | 說明 |
|-----|------|
| `toBeVisible()` | 元素在頁面上可見 |
| `toBeHidden()` | 元素不可見或不存在 |
| `toBeEnabled()` | 元素可互動（非 disabled） |
| `toBeDisabled()` | 元素被禁用 |
| `toBeChecked()` | checkbox / radio 被選取 |
| `toContainText(str)` | 元素文字包含指定字串 |
| `toHaveText(str)` | 元素文字完全符合（trim 後） |
| `toHaveValue(str)` | input / select 的值符合 |
| `toHaveAttribute(name, val)` | 元素屬性符合 |
| `toHaveClass(cls)` | 元素包含指定 CSS class |
| `toHaveCount(n)` | locator 匹配到 N 個元素 |
| `toHaveURL(url)` | 當前頁面 URL 符合 |
| `toHaveTitle(title)` | 當前頁面標題符合 |

#### Value Assertions（不需 `await`）

| API | 說明 |
|-----|------|
| `toBe(val)` | 嚴格相等（`===`） |
| `toEqual(obj)` | 深層相等（物件、陣列） |
| `toBeTruthy()` | 值為 truthy |
| `toBeFalsy()` | 值為 falsy |
| `toContain(val)` | 陣列 / 字串包含指定值 |
| `toHaveLength(n)` | 陣列 / 字串長度符合 |
| `toBeGreaterThan(n)` | 數值大於 N |
| `toBeLessThan(n)` | 數值小於 N |

### 基礎範例

```typescript
import { test, expect } from '@playwright/test'

test('電商首頁驗證', async ({ page }) => {
  await page.goto('https://shop.example.com')

  // ✅ Locator Assertion（有自動重試，推薦）
  await expect(page.getByRole('heading', { name: '熱銷商品' })).toBeVisible()
  await expect(page.getByTestId('cart-count')).toHaveText('0')
  await expect(page.getByRole('link', { name: '登入' })).toBeEnabled()

  // ✅ Value Assertion（對一般 JS 值使用，無重試）
  const title = await page.title()
  expect(title).toContain('購物網站')

  const itemCount = await page.getByTestId('product-card').count()
  expect(itemCount).toBeGreaterThan(0)
})
```

### 最佳實踐

- 優先使用 **Locator Assertion**（帶 `await`），不要先取得值再用 value assertion 驗證 DOM 狀態
- 使用語意化的 locator（`getByRole`、`getByLabel`）搭配 assertion，讓測試意圖清晰
- 每個測試的 assertion 數量不宜過多，聚焦在「這個情境最關鍵的驗證」
- 失敗訊息不夠明確時，使用 `.toHaveText()` 的第二個參數傳入自訂錯誤訊息：`await expect(el).toHaveText('成功', { message: '應顯示成功訊息' })`

### 常見錯誤

```typescript
// ❌ 錯誤：用 value assertion 驗證 DOM，沒有自動重試
const text = await page.getByTestId('status').textContent()
expect(text).toBe('已完成') // 若 API 回應慢，這行可能在更新前就執行

// ✅ 正確：用 locator assertion，會等待元素符合條件
await expect(page.getByTestId('status')).toHaveText('已完成')
```

```typescript
// ❌ 錯誤：忘記 await，assertion 沒有真正執行
expect(page.getByRole('button')).toBeVisible() // 少了 await，永遠不會失敗

// ✅ 正確
await expect(page.getByRole('button')).toBeVisible()
```

---

&nbsp;

## 🍀 0302 - toBeVisible

### 可見性驗證觀念

`toBeVisible()` 驗證元素是否**在頁面上對使用者可見**，判斷標準包括：

- 元素存在於 DOM
- `display` 不為 `none`
- `visibility` 不為 `hidden`
- `opacity` 不為 `0`
- 元素本身或祖先元素沒有隱藏

> **注意**：元素在視窗範圍外（scroll 下方）仍然算 **visible**。若要驗證元素完全不被遮擋，需配合其他方法。

### 可見性 API 比較

| API | 元素存在 DOM | 元素可見 | 說明 |
|-----|-------------|---------|------|
| `toBeVisible()` | ✅ | ✅ | 存在且可見 |
| `toBeHidden()` | 可有可無 | ❌ | 不可見，或根本不在 DOM |
| `toBeAttached()` | ✅ | 任意 | 只確認在 DOM 中，不管可見性 |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('登入表單可見性驗證', async ({ page }) => {
  await page.goto('https://shop.example.com/login')

  // 驗證表單元素可見
  await expect(page.getByLabel('電子郵件')).toBeVisible()
  await expect(page.getByLabel('密碼')).toBeVisible()
  await expect(page.getByRole('button', { name: '登入' })).toBeVisible()

  // 驗證錯誤訊息初始隱藏
  await expect(page.getByTestId('error-message')).toBeHidden()

  // 輸入錯誤密碼後驗證錯誤訊息出現
  await page.getByLabel('電子郵件').fill('user@example.com')
  await page.getByLabel('密碼').fill('wrong-password')
  await page.getByRole('button', { name: '登入' }).click()

  await expect(page.getByTestId('error-message')).toBeVisible()
  await expect(page.getByRole('button', { name: '登入' })).toBeVisible()
})

test('購物車側欄開關', async ({ page }) => {
  await page.goto('https://shop.example.com')

  // 初始狀態：購物車側欄隱藏
  await expect(page.getByTestId('cart-sidebar')).toBeHidden()

  // 點擊購物車圖示後：側欄出現
  await page.getByTestId('cart-icon').click()
  await expect(page.getByTestId('cart-sidebar')).toBeVisible()

  // 關閉後：側欄消失
  await page.getByRole('button', { name: '關閉購物車' }).click()
  await expect(page.getByTestId('cart-sidebar')).toBeHidden()
})
```

### 最佳實踐

- 用 `toBeVisible()` 驗證關鍵 UI 元素（錯誤訊息、成功提示、Modal）是否正確顯示或隱藏
- 驗證「元素不應顯示」時，優先用 `toBeHidden()` 而非 `not.toBeVisible()`（語意更明確）
- 若要同時驗證可見且包含特定文字，直接用 `toContainText()`，它隱含了可見性條件

### 常見錯誤

```typescript
// ❌ 錯誤：用 isVisible() 取值再判斷，沒有自動重試
const visible = await page.getByTestId('modal').isVisible()
expect(visible).toBe(true) // 動畫進行中可能還不可見就失敗

// ✅ 正確：直接用 locator assertion
await expect(page.getByTestId('modal')).toBeVisible()
```

```typescript
// ❌ 錯誤：元素在 DOM 但隱藏，誤以為不在 DOM
await expect(page.getByTestId('tooltip')).not.toBeAttached() // 可能失敗，因為元素存在但只是隱藏

// ✅ 正確：確認不可見
await expect(page.getByTestId('tooltip')).toBeHidden()
```

---

&nbsp;

## 🍀 0303 - toContainText

### 文字匹配觀念

`toContainText()` 驗證元素的文字內容**包含**指定字串，支援：

- 純字串（部分匹配）
- 正則表達式（`RegExp`）
- 陣列（驗證清單中每個項目）

> Playwright 在比對時會**標準化空白**（去除多餘空格、換行），不需要精確控制空白格式。

### toContainText vs toHaveText

| API | 匹配方式 | 說明 |
|-----|---------|------|
| `toContainText('abc')` | 部分匹配 | 文字包含 `abc` 即通過 |
| `toHaveText('abc')` | 完全匹配（trim） | 文字 trim 後必須完全等於 `abc` |
| `toHaveText(/abc/)` | 正則匹配 | 文字符合正則表達式 |
| `toContainText(['a', 'b'])` | 陣列部分匹配 | 清單元素依序包含這些文字 |
| `toHaveText(['a', 'b'])` | 陣列完全匹配 | 清單元素數量和文字完全吻合 |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('商品詳情頁文字驗證', async ({ page }) => {
  await page.goto('https://shop.example.com/products/1')

  // 部分文字匹配
  await expect(page.getByTestId('product-title')).toContainText('無線藍牙耳機')

  // 使用正則表達式（驗證價格格式）
  await expect(page.getByTestId('product-price')).toContainText(/NT\$ \d+/)

  // 不區分大小寫（透過 regex flag）
  await expect(page.getByTestId('product-badge')).toContainText(/推薦/i)
})

test('訂單清單驗證', async ({ page }) => {
  await page.goto('https://shop.example.com/orders')

  const orderItems = page.getByTestId('order-item')

  // 驗證清單包含這些文字（部分匹配，順序需一致）
  await expect(orderItems).toContainText([
    '訂單 #1001',
    '訂單 #1002',
    '訂單 #1003',
  ])
})

test('成功通知訊息', async ({ page }) => {
  await page.goto('https://shop.example.com/checkout')

  // 填寫並送出結帳表單
  await page.getByLabel('收件人').fill('王小明')
  await page.getByLabel('地址').fill('台北市信義區信義路五段7號')
  await page.getByRole('button', { name: '確認訂單' }).click()

  // 驗證成功訊息包含訂單編號格式
  await expect(page.getByTestId('success-message')).toContainText(/訂單編號：#\d+/)
  await expect(page.getByTestId('success-message')).toContainText('感謝您的購買')
})
```

### 最佳實踐

- 優先使用 `toContainText` 而非 `toHaveText`，避免因動態內容（如時間戳記）導致完全匹配失敗
- 驗證動態產生的數字、ID 時，使用正則表達式 `/\d+/` 而非固定字串
- 驗證清單元素時，使用陣列形式的 `toContainText(['...', '...'])`，一次驗證多個項目

### 常見錯誤

```typescript
// ❌ 錯誤：用 toHaveText 驗證包含動態內容的文字，容易因格式差異失敗
await expect(page.getByTestId('welcome')).toHaveText('歡迎回來，王小明！您有 3 則通知')

// ✅ 正確：只驗證關鍵部分
await expect(page.getByTestId('welcome')).toContainText('歡迎回來，王小明')
```

```typescript
// ❌ 錯誤：驗證清單時只驗證第一個元素
await expect(page.getByTestId('order-item').first()).toContainText('訂單 #1001')
// 無法確保 #1002, #1003 也存在

// ✅ 正確：對整個 locator 使用陣列驗證
await expect(page.getByTestId('order-item')).toContainText(['#1001', '#1002', '#1003'])
```

---

&nbsp;

## 🍀 0304 - toHaveValue

### 表單值驗證觀念

`toHaveValue()` 驗證表單元素的**當前值**，適用於：

- `<input type="text/email/password/number">` → 輸入的文字值
- `<textarea>` → 文字內容
- `<select>` → 被選取的 option 的 value 屬性值

> **注意**：`toHaveValue()` 驗證的是元素的 **value 屬性**，不是顯示文字。`<select>` 元素驗證的是 `<option value="...">` 的 value，而非 option 的顯示文字。

### 各元素類型對應 API

| 元素類型 | 驗證值的 API | 說明 |
|---------|------------|------|
| `input`, `textarea`, `select` | `toHaveValue(str)` | 驗證 `.value` 屬性 |
| `input[type=checkbox]` | `toBeChecked()` | 驗證是否勾選 |
| `input[type=radio]` | `toBeChecked()` | 驗證是否被選 |
| 多選 `select` | `toHaveValues(['a', 'b'])` | 驗證多個選中值 |
| 任意元素的顯示文字 | `toHaveText()` | 驗證 `textContent` |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('個人資料表單預填值驗證', async ({ page }) => {
  await page.goto('https://shop.example.com/profile')

  // 驗證 input 預填值
  await expect(page.getByLabel('姓名')).toHaveValue('王小明')
  await expect(page.getByLabel('電子郵件')).toHaveValue('wang@example.com')

  // 驗證 textarea
  await expect(page.getByLabel('備註')).toHaveValue('請勿在門口放置')

  // 驗證 select（依 value 屬性，非顯示文字）
  await expect(page.getByLabel('縣市')).toHaveValue('taipei')

  // 驗證 checkbox
  await expect(page.getByLabel('訂閱電子報')).toBeChecked()
  await expect(page.getByLabel('接受行銷訊息')).not.toBeChecked()
})

test('篩選條件送出後驗證狀態保留', async ({ page }) => {
  await page.goto('https://shop.example.com/products')

  // 設定篩選條件
  await page.getByLabel('價格範圍').selectOption('1000-3000')
  await page.getByLabel('排序').selectOption('price-asc')
  await page.getByRole('button', { name: '套用篩選' }).click()

  // 驗證重新渲染後篩選值仍保留
  await expect(page.getByLabel('價格範圍')).toHaveValue('1000-3000')
  await expect(page.getByLabel('排序')).toHaveValue('price-asc')
})

test('多選下拉選單驗證', async ({ page }) => {
  await page.goto('https://shop.example.com/preferences')

  await page.getByLabel('偏好類別').selectOption(['electronics', 'clothing'])

  // 使用 toHaveValues 驗證多選
  await expect(page.getByLabel('偏好類別')).toHaveValues(['electronics', 'clothing'])
})
```

### 最佳實踐

- 填寫表單後立即用 `toHaveValue()` 驗證，確認 locator 正確且值有被寫入
- 驗證 `<select>` 時，先確認 option 的 `value` 屬性是什麼（非顯示文字）
- checkbox 和 radio 使用 `toBeChecked()` / `not.toBeChecked()`，不用 `toHaveValue()`

### 常見錯誤

```typescript
// ❌ 錯誤：用 toContainText 驗證 input 的值
await expect(page.getByLabel('姓名')).toContainText('王小明')
// input 沒有 textContent，這個方式無效

// ✅ 正確
await expect(page.getByLabel('姓名')).toHaveValue('王小明')
```

```typescript
// ❌ 錯誤：對 select 用顯示文字驗證
await expect(page.getByLabel('縣市')).toHaveValue('台北市')
// 如果 option value 是 "taipei"，這行會失敗

// ✅ 正確：使用 value 屬性的值
await expect(page.getByLabel('縣市')).toHaveValue('taipei')
```

---

&nbsp;

## 🍀 0305 - toHaveURL

### URL 驗證觀念

`toHaveURL()` 驗證當前頁面的 URL，是導航流程測試的關鍵 assertion。它支援：

- 完整 URL 字串（完全匹配）
- 部分字串（使用 `string` 包含匹配）
- 正則表達式（靈活匹配動態 URL）

> **注意**：`toHaveURL()` 是 **Page Assertion**，直接對 `page` 物件使用，非 locator。

### URL 匹配方式比較

| 匹配方式 | 範例 | 說明 |
|---------|------|------|
| 完整字串 | `toHaveURL('https://example.com/login')` | URL 必須完全相等 |
| 部分字串 | `toHaveURL(/dashboard/)` | URL 包含 `dashboard`（需用 regex） |
| 正則表達式 | `toHaveURL(/\/orders\/\d+/)` | URL 符合 regex pattern |
| 相對路徑（搭配 baseURL） | `toHaveURL('/dashboard')` | 需在 config 設定 `baseURL` |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('登入成功後跳轉至首頁', async ({ page }) => {
  await page.goto('https://shop.example.com/login')

  await page.getByLabel('電子郵件').fill('user@example.com')
  await page.getByLabel('密碼').fill('correct-password')
  await page.getByRole('button', { name: '登入' }).click()

  // 驗證跳轉至會員首頁
  await expect(page).toHaveURL('https://shop.example.com/member/dashboard')
})

test('商品詳情頁 URL 格式驗證', async ({ page }) => {
  await page.goto('https://shop.example.com')

  await page.getByTestId('product-card').first().click()

  // URL 包含 /products/ 加上數字 ID（動態）
  await expect(page).toHaveURL(/\/products\/\d+/)
})

test('結帳完成後 URL 含訂單編號', async ({ page }) => {
  await page.goto('https://shop.example.com/checkout')

  // 完成結帳流程...
  await page.getByRole('button', { name: '確認訂單' }).click()

  // 驗證跳轉至訂單完成頁，URL 含訂單 ID
  await expect(page).toHaveURL(/\/orders\/\d+\/confirmation/)
})

test('搜尋結果頁 URL 含 query string', async ({ page }) => {
  await page.goto('https://shop.example.com')

  await page.getByPlaceholder('搜尋商品').fill('藍牙耳機')
  await page.keyboard.press('Enter')

  // 驗證搜尋關鍵字出現在 URL
  await expect(page).toHaveURL(/[?&]q=%E8%97%8D%E7%89%99%E8%80%B3%E6%A9%9F/)
})
```

### 最佳實踐

- 在登入、送出表單、刪除等操作後，**一定要驗證 URL 跳轉**，確認流程正確完成
- 使用正則表達式處理包含動態 ID 的 URL，避免 hardcode 特定的 resource ID
- 在 `playwright.config.ts` 設定 `baseURL`，讓測試可用相對路徑：`await expect(page).toHaveURL('/dashboard')`

### 常見錯誤

```typescript
// ❌ 錯誤：用 page.url() 取值比較，沒有自動重試
const url = page.url()
expect(url).toContain('/dashboard') // 導航可能還沒完成就失敗

// ✅ 正確：使用 assertion，有自動重試等待導航完成
await expect(page).toHaveURL(/dashboard/)
```

```typescript
// ❌ 錯誤：字串匹配用 toHaveURL('...') 做部分比對（字串版是完整比對）
await expect(page).toHaveURL('/dashboard') // 如果實際 URL 是 https://example.com/dashboard 會失敗

// ✅ 正確：要部分匹配就用正則
await expect(page).toHaveURL(/\/dashboard/)
```

---

&nbsp;

## 🍀 0306 - auto retry

### 自動重試機制觀念

Playwright 的 **Locator Assertion** 內建自動重試（auto retry）機制：

```txt
assertion 執行
    → 條件不符合？
        → 等待一段時間後重試
            → 條件符合 → 通過 ✅
            → 超過 timeout → 失敗 ❌
```

這個機制讓測試能夠處理：
- 非同步 API 回應後的 UI 更新
- CSS 動畫、過場效果完成後的狀態
- SPA 路由切換後的元件渲染

> **重要**：自動重試只在 **Locator Assertion**（`await expect(locator).xxx()`）有效。**Value Assertion** 和直接呼叫 `locator.textContent()` 等方法都**沒有**自動重試。

### 有重試 vs 無重試的 API 對比

| 方式 | 是否自動重試 | 範例 |
|------|------------|------|
| Locator Assertion | ✅ 有 | `await expect(locator).toHaveText('ok')` |
| `locator.textContent()` | ❌ 無 | `await locator.textContent()` |
| `locator.isVisible()` | ❌ 無 | `await locator.isVisible()` |
| `page.url()` | ❌ 無 | `page.url()` |
| `page.title()` | ❌ 無 | `await page.title()` |

### timeout 設定

可在三個層級設定 assertion timeout：

#### 全域設定（playwright.config.ts）

```typescript
import { defineConfig } from '@playwright/test'

export default defineConfig({
  expect: {
    timeout: 10000, // assertion 等待最多 10 秒（預設 5000ms）
  },
})
```

#### 單次 assertion 覆寫

```typescript
// 這個 assertion 最多等待 15 秒
await expect(page.getByTestId('result')).toHaveText('完成', { timeout: 15000 })
```

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('非同步操作後的 UI 狀態驗證', async ({ page }) => {
  await page.goto('https://shop.example.com/cart')

  // 點擊刪除後，後端 API 回應需要時間
  await page.getByTestId('remove-item-btn').first().click()

  // ✅ 自動重試：等待商品數量更新（可能需要 1-3 秒）
  await expect(page.getByTestId('cart-count')).toHaveText('2')

  // ✅ 自動重試：等待成功通知出現
  await expect(page.getByTestId('toast-message')).toContainText('已移除商品')

  // ✅ 自動重試：等待通知自動消失
  await expect(page.getByTestId('toast-message')).toBeHidden()
})

test('搜尋結果載入等待', async ({ page }) => {
  await page.goto('https://shop.example.com')

  await page.getByPlaceholder('搜尋商品').fill('耳機')
  await page.keyboard.press('Enter')

  // 等待 loading spinner 消失（SPA 路由轉換 + API 載入）
  await expect(page.getByTestId('loading-spinner')).toBeHidden()

  // 等待搜尋結果出現
  await expect(page.getByTestId('product-card')).toHaveCount(10)

  // 若搜尋需要特別長的時間（例如全文搜尋），延長 timeout
  await expect(page.getByTestId('search-summary')).toContainText('約 10 筆結果', {
    timeout: 10000,
  })
})
```

### 最佳實踐

- 不要用 `page.waitForTimeout(3000)` 等固定時間，改用 assertion 的自動重試（更快、更可靠）
- 若某個操作需要更長的等待，針對那個 assertion 設定 `{ timeout: 15000 }`，而非全域調大
- 全域 `expect.timeout` 建議設定 8000–10000ms，對大多數應用足夠

### 常見錯誤

```typescript
// ❌ 錯誤：固定等待時間，不穩定（太短失敗，太長浪費）
await page.waitForTimeout(3000)
await expect(page.getByTestId('result')).toHaveText('成功')

// ✅ 正確：直接用 assertion 等待，自動重試到成功或 timeout
await expect(page.getByTestId('result')).toHaveText('成功')
```

```typescript
// ❌ 錯誤：用 isVisible() 自己做輪詢（反模式）
let visible = false
while (!visible) {
  visible = await page.getByTestId('modal').isVisible()
}

// ✅ 正確：交給 Playwright assertion 處理
await expect(page.getByTestId('modal')).toBeVisible()
```

---

&nbsp;

## 🍀 0307 - 軟斷言 vs 硬斷言

### 斷言模式觀念

Playwright 提供兩種斷言模式：

| 模式 | API | 失敗行為 |
|------|-----|---------|
| **硬斷言**（Hard Assertion） | `expect(...)` | 立即停止測試，後續程式碼不執行 |
| **軟斷言**（Soft Assertion） | `expect.soft(...)` | 記錄失敗但繼續執行，測試結束後統一報告所有失敗 |

```txt
硬斷言流程：
step 1 ✅ → step 2 ✅ → step 3 ❌（立即停止）→ step 4 未執行

軟斷言流程：
step 1 ✅ → step 2 ✅ → step 3 ❌（記錄，繼續）→ step 4 ✅ → 測試結束，回報 step 3 失敗
```

### 硬斷言 vs 軟斷言比較

| 面向 | 硬斷言 | 軟斷言 |
|------|--------|--------|
| 失敗後行為 | 立即終止 | 繼續執行 |
| 適用場景 | 關鍵流程前提條件、流程性測試 | 頁面完整性驗證、多欄位表單驗證 |
| 優點 | 快速失敗，節省執行時間 | 一次收集所有問題 |
| 缺點 | 第一個失敗後看不到後續問題 | 可能遮蓋因前一步失敗造成的連鎖問題 |
| 使用時機 | **預設使用** | 需要同時驗證多個獨立欄位或 UI 狀態 |

### 實際範例

```typescript
import { test, expect } from '@playwright/test'

test('商品詳情頁完整資訊驗證（軟斷言）', async ({ page }) => {
  await page.goto('https://shop.example.com/products/1')

  // 使用軟斷言一次驗證多個獨立的 UI 欄位
  // 即使某個欄位失敗，其他欄位也會被驗證
  await expect.soft(page.getByTestId('product-title')).toBeVisible()
  await expect.soft(page.getByTestId('product-price')).toContainText(/NT\$/)
  await expect.soft(page.getByTestId('product-description')).toBeVisible()
  await expect.soft(page.getByTestId('product-image')).toBeVisible()
  await expect.soft(page.getByTestId('add-to-cart-btn')).toBeEnabled()
  await expect.soft(page.getByTestId('stock-status')).toContainText('有庫存')

  // 使用硬斷言驗證關鍵前提（加入購物車按鈕存在後才繼續測試互動）
  await expect(page.getByTestId('add-to-cart-btn')).toBeVisible()
  await page.getByTestId('add-to-cart-btn').click()

  await expect.soft(page.getByTestId('cart-count')).toHaveText('1')
  await expect.soft(page.getByTestId('toast-message')).toContainText('已加入購物車')

  // 測試結束時，Playwright 會自動統計所有 soft assertion 的失敗
})
```

```typescript
import { test, expect } from '@playwright/test'

test('登入流程（硬斷言，流程性測試）', async ({ page }) => {
  await page.goto('https://shop.example.com/login')

  await page.getByLabel('電子郵件').fill('user@example.com')
  await page.getByLabel('密碼').fill('password123')
  await page.getByRole('button', { name: '登入' }).click()

  // 硬斷言：若 URL 不對，後面的操作沒有意義，應立即停止
  await expect(page).toHaveURL('/member/dashboard')

  // 確認進入正確頁面後才繼續
  await expect(page.getByText('歡迎回來')).toBeVisible()
})
```

```typescript
import { test, expect } from '@playwright/test'

test('表單送出後多欄位錯誤驗證（軟斷言）', async ({ page }) => {
  await page.goto('https://shop.example.com/register')

  // 送出空白表單，觸發所有欄位驗證錯誤
  await page.getByRole('button', { name: '註冊' }).click()

  // 使用軟斷言一次確認所有錯誤訊息都出現
  await expect.soft(page.getByTestId('email-error')).toContainText('請輸入電子郵件')
  await expect.soft(page.getByTestId('password-error')).toContainText('請輸入密碼')
  await expect.soft(page.getByTestId('name-error')).toContainText('請輸入姓名')
  await expect.soft(page.getByTestId('phone-error')).toContainText('請輸入手機號碼')
})
```

### 最佳實踐

- **預設使用硬斷言**，只在需要「一次驗證多個獨立 UI 狀態」時才用軟斷言
- 軟斷言適合：頁面資訊完整性驗證（商品詳情、表單錯誤清單）
- 硬斷言適合：流程性測試（登入 → 跳轉 → 操作），前一步是後一步的前提
- 不要在同一個測試中混用大量軟硬斷言，會讓失敗原因難以追蹤

### 常見錯誤

```typescript
// ❌ 錯誤：把所有 assertion 都改成軟斷言，以為這樣「更好」
await expect.soft(page).toHaveURL('/dashboard') // 若 URL 錯誤還繼續執行
await expect.soft(page.getByTestId('profile-name')).toHaveText('王小明') // 可能根本不在這頁

// ✅ 正確：關鍵前提用硬斷言確保正確性
await expect(page).toHaveURL('/dashboard')
await expect.soft(page.getByTestId('profile-name')).toHaveText('王小明')
```

```typescript
// ❌ 錯誤：不知道軟斷言需要測試「跑完」才會報告失敗
// 若測試中途因硬斷言失敗而停止，軟斷言的失敗訊息不會出現
await expect.soft(page.getByTestId('field-a')).toHaveText('a')
await expect(page).toHaveURL('/wrong-url') // 硬斷言失敗，測試停止
// 上面軟斷言的結果可能不會顯示在報告中

// ✅ 正確：理解軟斷言需要測試完整執行才能收集所有結果
// 在軟斷言區塊結束後不要放會中斷流程的硬斷言
```

---

&nbsp;
