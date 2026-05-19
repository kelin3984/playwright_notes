# 💫 Chapter 4 — Browser 與 Context 機制

> 理解瀏覽器 automation 本質，掌握 Browser / Context / Page 三層架構，正確管理 Cookie、Storage 與多分頁情境。

&nbsp;

## 目錄

- [💫 Chapter 4 — Browser 與 Context 機制](#-chapter-4--browser-與-context-機制)
  - [目錄](#目錄)
  - [🍀 0401 - Browser](#-0401---browser)
    - [Browser 物件說明](#browser-物件說明)
    - [三種瀏覽器引擎](#三種瀏覽器引擎)
    - [手動啟動 Browser](#手動啟動-browser)
  - [🍀 0402 - BrowserContext](#-0402---browsercontext)
    - [Context 核心概念](#context-核心概念)
    - [Context vs Page](#context-vs-page)
    - [建立 Context](#建立-context)
    - [Context 初始化選項](#context-初始化選項)
  - [🍀 0403 - Cookie](#-0403---cookie)
    - [操作 Cookie 的方式](#操作-cookie-的方式)
    - [新增 Cookie](#新增-cookie)
    - [讀取 Cookie](#讀取-cookie)
    - [清除 Cookie](#清除-cookie)
    - [Cookie 欄位說明](#cookie-欄位說明)
  - [🍀 0404 - LocalStorage](#-0404---localstorage)
    - [操作 LocalStorage](#操作-localstorage)
    - [透過 storageState 預設 LocalStorage](#透過-storagestate-預設-localstorage)
  - [🍀 0405 - SessionStorage](#-0405---sessionstorage)
    - [SessionStorage 特性](#sessionstorage-特性)
    - [操作 SessionStorage](#操作-sessionstorage)
  - [🍀 0406 - popup](#-0406---popup)
    - [什麼是 popup？](#什麼是-popup)
    - [捕捉 popup 視窗](#捕捉-popup-視窗)
    - [常見情境：點擊連結開啟新視窗](#常見情境點擊連結開啟新視窗)
  - [🍀 0407 - 多分頁](#-0407---多分頁)
    - [多分頁基本操作](#多分頁基本操作)
    - [切換分頁](#切換分頁)
    - [同步操作多分頁](#同步操作多分頁)

---

&nbsp;

## 🍀 0401 - Browser

### Browser 物件說明

`Browser` 是 Playwright 中最頂層的物件，代表一個**瀏覽器程序（browser process）**。

```txt
Browser
  └── BrowserContext（相當於無痕視窗）
        └── Page（分頁）
              └── Locator / Frame / ...
```

在 Test Runner 模式下，Playwright 會自動管理 Browser 的生命週期；在 Library 模式下，需要手動 `launch()` 與 `close()`。

### 三種瀏覽器引擎

| 引擎 | 說明 | 對應真實瀏覽器 |
|------|------|----------------|
| `chromium` | Blink 核心 | Chrome、Edge |
| `firefox` | Gecko 核心 | Firefox |
| `webkit` | WebKit 核心 | Safari |

### 手動啟動 Browser

```typescript
import { chromium, firefox, webkit } from '@playwright/test'

// 手動啟動（Library 模式）
const browser = await chromium.launch({ headless: false })
const context = await browser.newContext()
const page = await context.newPage()

await page.goto('https://example.com')
await browser.close()
```

> **Test Runner 模式下不需要手動啟動**，直接使用 fixture 注入的 `browser` 物件即可。

```typescript
import { test } from '@playwright/test'

test('使用 fixture browser', async ({ browser }) => {
  const context = await browser.newContext()
  const page = await context.newPage()
  await page.goto('https://example.com')
  await context.close()
})
```

**最佳實踐**

- 優先使用 Test Runner 的 `page` fixture，Playwright 會自動處理生命週期
- 只有在需要自訂 Context 設定（如模擬裝置、設定語系）時才手動建立 Context
- `browser.close()` 會同時關閉所有 Context 與 Page

**常見錯誤**

| ❌ 錯誤 | ✅ 正確 |
|--------|--------|
| 在 test 結束後忘記 `browser.close()` | 搭配 `try/finally` 或使用 Test Runner fixture |
| 混用 Library 模式與 Test Runner 模式 | 確認使用情境，選一種模式一致使用 |

---

&nbsp;

## 🍀 0402 - BrowserContext

### Context 核心概念

`BrowserContext` 是 Playwright 中**隔離測試環境的核心單位**，相當於瀏覽器的「無痕視窗」。

```txt
Context ≈ 無痕視窗
```

每個 Context 擁有獨立的：
- Cookie
- LocalStorage / SessionStorage
- 快取
- 登入狀態

因此，**不同 Context 之間的狀態完全隔離**，這是 Playwright 實現 test isolation 的關鍵機制。

### Context vs Page

| 比較項目 | BrowserContext | Page |
|----------|---------------|------|
| 比喻 | 無痕視窗 | 分頁（Tab） |
| 狀態隔離 | ✅ 完全隔離 | ❌ 同 Context 共用狀態 |
| Cookie / Storage | 每個 Context 獨立 | 繼承所在 Context |
| 建立方式 | `browser.newContext()` | `context.newPage()` |

### 建立 Context

```typescript
import { test, expect } from '@playwright/test'

test('建立獨立 Context', async ({ browser }) => {
  // 建立兩個完全隔離的 Context
  const adminContext = await browser.newContext()
  const guestContext = await browser.newContext()

  const adminPage = await adminContext.newPage()
  const guestPage = await guestContext.newPage()

  // 各自登入不同帳號
  await adminPage.goto('https://shop.example.com/login')
  await adminPage.getByLabel('Email').fill('admin@example.com')
  await adminPage.getByLabel('密碼').fill('admin-secret')
  await adminPage.getByRole('button', { name: '登入' }).click()

  await guestPage.goto('https://shop.example.com/login')
  await guestPage.getByLabel('Email').fill('guest@example.com')
  await guestPage.getByLabel('密碼').fill('guest-secret')
  await guestPage.getByRole('button', { name: '登入' }).click()

  // 兩個 Context 的 Cookie 完全隔離，互不干擾
  await adminContext.close()
  await guestContext.close()
})
```

### Context 初始化選項

```typescript
const context = await browser.newContext({
  // 模擬裝置 viewport
  viewport: { width: 1280, height: 720 },

  // 設定語系與時區
  locale: 'zh-TW',
  timezoneId: 'Asia/Taipei',

  // 模擬地理位置
  geolocation: { longitude: 121.5654, latitude: 25.033 },
  permissions: ['geolocation'],

  // 預載入登入狀態（storageState）
  storageState: 'auth.json',

  // 忽略 HTTPS 憑證錯誤
  ignoreHTTPSErrors: true,
})
```

**最佳實踐**

- 每個測試應使用獨立的 Context，避免測試間狀態污染
- 在 `playwright.config.ts` 的 `use` 設定全域 Context 選項，避免重複設定
- 多角色測試（admin / user / guest）使用多個 Context 分別模擬

**常見錯誤**

| ❌ 錯誤 | ✅ 正確 |
|--------|--------|
| 多個測試共用同一個 Context | 每個 test 使用獨立 Context |
| 用 `page.goto()` 手動清除登入狀態 | 使用新 Context 自動獲得乾淨狀態 |

---

&nbsp;

## 🍀 0403 - Cookie

### 操作 Cookie 的方式

Playwright 透過 `BrowserContext` 提供直接讀寫 Cookie 的能力，無需透過 UI 操作，適合：
- 預先設定登入 Cookie，跳過登入流程
- 驗證登入後 Cookie 是否正確設置
- 清除特定 Cookie 後驗證系統行為

### 新增 Cookie

```typescript
import { test, expect } from '@playwright/test'

test('預設登入 Cookie，直接進入後台', async ({ context, page }) => {
  // 直接注入 Cookie，繞過登入 UI
  await context.addCookies([
    {
      name: 'session_token',
      value: 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.test',
      domain: 'shop.example.com',
      path: '/',
      httpOnly: true,
      secure: true,
      sameSite: 'Lax',
    },
  ])

  // 因為 Cookie 已設定，直接進入後台不會被導向登入頁
  await page.goto('https://shop.example.com/admin')
  await expect(page).toHaveURL('https://shop.example.com/admin/dashboard')
  await expect(page.getByRole('heading', { name: '管理後台' })).toBeVisible()
})
```

### 讀取 Cookie

```typescript
test('驗證登入後 Cookie 是否正確設置', async ({ page, context }) => {
  await page.goto('https://shop.example.com/login')
  await page.getByLabel('Email').fill('user@example.com')
  await page.getByLabel('密碼').fill('password123')
  await page.getByRole('button', { name: '登入' }).click()

  // 讀取所有 Cookie
  const cookies = await context.cookies()

  // 找到 session_token Cookie
  const sessionCookie = cookies.find(c => c.name === 'session_token')
  expect(sessionCookie).toBeDefined()
  expect(sessionCookie?.httpOnly).toBe(true)
  expect(sessionCookie?.secure).toBe(true)

  // 讀取指定 URL 的 Cookie
  const siteCookies = await context.cookies('https://shop.example.com')
  console.log('Cookie 數量：', siteCookies.length)
})
```

### 清除 Cookie

```typescript
test('清除 Cookie 後應跳回登入頁', async ({ page, context }) => {
  // 先登入
  await page.goto('https://shop.example.com/login')
  await page.getByLabel('Email').fill('user@example.com')
  await page.getByLabel('密碼').fill('password123')
  await page.getByRole('button', { name: '登入' }).click()
  await expect(page).toHaveURL(/dashboard/)

  // 清除所有 Cookie（模擬登出或 Cookie 過期）
  await context.clearCookies()

  // 重新訪問需要登入的頁面，應被導向登入頁
  await page.goto('https://shop.example.com/profile')
  await expect(page).toHaveURL(/login/)
})
```

### Cookie 欄位說明

| 欄位 | 型別 | 說明 |
|------|------|------|
| `name` | `string` | Cookie 名稱 |
| `value` | `string` | Cookie 值 |
| `domain` | `string` | 適用網域（不含 `https://`） |
| `path` | `string` | 適用路徑，通常為 `/` |
| `expires` | `number` | Unix timestamp，不設定則為 session cookie |
| `httpOnly` | `boolean` | 是否禁止 JS 存取（安全性） |
| `secure` | `boolean` | 是否只在 HTTPS 傳送 |
| `sameSite` | `'Strict' \| 'Lax' \| 'None'` | CSRF 防護設定 |

**最佳實踐**

- 登入狀態優先使用 `storageState`（包含 Cookie + LocalStorage），一次儲存、重複使用
- 只需驗證 Cookie 時才直接操作 `context.cookies()`
- 設定 Cookie 時務必填寫 `domain` 與 `path`，否則可能不生效

**常見錯誤**

| ❌ 錯誤 | ✅ 正確 |
|--------|--------|
| `domain` 寫成 `https://shop.example.com` | `domain` 應為 `shop.example.com`（不含協定） |
| 假設 Cookie 對所有子網域生效 | 明確指定 `.example.com` 以覆蓋子網域 |

---

&nbsp;

## 🍀 0404 - LocalStorage

### 操作 LocalStorage

LocalStorage 屬於 `Page` 層級（而非 Context），需透過 `page.evaluate()` 執行 JavaScript 來操作。

```typescript
import { test, expect } from '@playwright/test'

test('預設 LocalStorage 使用者偏好設定', async ({ page }) => {
  // 先導航到目標網站，LocalStorage 需在頁面載入後才能設定
  await page.goto('https://shop.example.com')

  // 設定 LocalStorage
  await page.evaluate(() => {
    localStorage.setItem('theme', 'dark')
    localStorage.setItem('language', 'zh-TW')
    localStorage.setItem('cart', JSON.stringify([
      { id: 1, name: '商品A', quantity: 2 },
      { id: 2, name: '商品B', quantity: 1 },
    ]))
  })

  // 重新載入頁面，驗證設定是否被讀取
  await page.reload()
  await expect(page.locator('html')).toHaveAttribute('data-theme', 'dark')
})

test('讀取並驗證 LocalStorage 內容', async ({ page }) => {
  await page.goto('https://shop.example.com')

  // 加入購物車
  await page.getByRole('button', { name: '加入購物車' }).first().click()

  // 讀取 LocalStorage
  const cart = await page.evaluate(() => {
    return JSON.parse(localStorage.getItem('cart') ?? '[]')
  })

  expect(cart.length).toBeGreaterThan(0)
  expect(cart[0]).toHaveProperty('id')
})

test('清除 LocalStorage', async ({ page }) => {
  await page.goto('https://shop.example.com')

  await page.evaluate(() => {
    localStorage.clear()
  })

  const itemCount = await page.evaluate(() => localStorage.length)
  expect(itemCount).toBe(0)
})
```

### 透過 storageState 預設 LocalStorage

```typescript
// 建立包含 LocalStorage 的 storageState 檔案
// 先手動執行一次登入流程，儲存狀態
import { chromium } from '@playwright/test'

async function saveStorageState() {
  const browser = await chromium.launch()
  const context = await browser.newContext()
  const page = await context.newPage()

  await page.goto('https://shop.example.com/login')
  await page.getByLabel('Email').fill('user@example.com')
  await page.getByLabel('密碼').fill('password123')
  await page.getByRole('button', { name: '登入' }).click()

  // 儲存 Cookie + LocalStorage 狀態
  await context.storageState({ path: 'auth.json' })
  await browser.close()
}
```

```typescript
// 在測試中直接使用預存狀態
test.use({ storageState: 'auth.json' })

test('使用預存登入狀態', async ({ page }) => {
  // 無需登入流程，直接訪問需要驗證的頁面
  await page.goto('https://shop.example.com/profile')
  await expect(page.getByText('我的帳戶')).toBeVisible()
})
```

**最佳實踐**

- 優先使用 `storageState` 而非逐一設定 LocalStorage，可同時包含 Cookie
- 操作 LocalStorage 前必須先 `page.goto()` 到對應網域
- 使用 `JSON.stringify` / `JSON.parse` 處理複雜資料結構

**常見錯誤**

| ❌ 錯誤 | ✅ 正確 |
|--------|--------|
| 在 `page.goto()` 前操作 LocalStorage | 必須先導航到目標網域才能操作 |
| 直接存字串物件 `localStorage.setItem('data', {})` | 使用 `JSON.stringify({})` 序列化 |

---

&nbsp;

## 🍀 0405 - SessionStorage

### SessionStorage 特性

SessionStorage 與 LocalStorage API 相同，但有一個重要差異：

| 特性 | LocalStorage | SessionStorage |
|------|-------------|----------------|
| 生命週期 | 永久保留（手動清除前） | 分頁關閉後自動清除 |
| 跨分頁共用 | ✅ 同網域可共用 | ❌ 每個分頁獨立 |
| 容量 | ~5MB | ~5MB |
| 使用情境 | 使用者偏好、購物車 | 一次性表單資料、臨時 token |

> **注意**：在 Playwright 中，SessionStorage 的「分頁」等同於 `Page` 物件，關閉 `page` 後 SessionStorage 即消失。

### 操作 SessionStorage

```typescript
import { test, expect } from '@playwright/test'

test('設定 SessionStorage 模擬多步驟表單暫存', async ({ page }) => {
  await page.goto('https://shop.example.com/checkout')

  // 預設多步驟結帳的第一步資料
  await page.evaluate(() => {
    sessionStorage.setItem('checkout_step', '2')
    sessionStorage.setItem('shipping_info', JSON.stringify({
      name: '測試使用者',
      address: '台北市信義區信義路五段7號',
      phone: '0912345678',
    }))
  })

  await page.reload()

  // 驗證表單是否從第二步開始
  await expect(page.getByTestId('checkout-step')).toHaveText('步驟 2/3')
})

test('讀取 SessionStorage', async ({ page }) => {
  await page.goto('https://shop.example.com/product/1')

  // 點擊加入比較清單
  await page.getByRole('button', { name: '加入比較' }).click()

  const compareList = await page.evaluate(() => {
    return JSON.parse(sessionStorage.getItem('compare_list') ?? '[]')
  })

  expect(compareList).toContain(1)
})

test('驗證 SessionStorage 在新分頁中不共用', async ({ context, page }) => {
  await page.goto('https://shop.example.com')

  // 在第一個分頁設定 SessionStorage
  await page.evaluate(() => {
    sessionStorage.setItem('tab_id', 'tab-001')
  })

  // 開啟新分頁
  const newPage = await context.newPage()
  await newPage.goto('https://shop.example.com')

  // 新分頁讀不到第一個分頁的 SessionStorage
  const tabId = await newPage.evaluate(() => sessionStorage.getItem('tab_id'))
  expect(tabId).toBeNull()

  await newPage.close()
})
```

**最佳實踐**

- 測試多步驟流程（wizard）時，使用 SessionStorage 預設中間步驟資料，減少重複操作
- 驗證 SessionStorage 隔離性時，需使用 `context.newPage()` 開啟新分頁

**常見錯誤**

| ❌ 錯誤 | ✅ 正確 |
|--------|--------|
| 以為 SessionStorage 跨分頁共用 | SessionStorage 每個 `Page` 物件獨立 |
| 頁面關閉後還試圖讀取 SessionStorage | SessionStorage 隨 Page 關閉而消失 |

---

&nbsp;

## 🍀 0406 - popup

### 什麼是 popup？

popup 指的是透過以下方式開啟的新視窗（new window）：

```javascript
// 頁面上的 JS
window.open('https://example.com', '_blank', 'width=800,height=600')
```

或是帶有 `target="_blank"` 的連結點擊。

> **popup 與一般多分頁的差異**：popup 是帶有特定尺寸的子視窗（window），而多分頁是在同一視窗內的 tab。Playwright 都用 `Page` 物件表示，但觸發機制不同。

### 捕捉 popup 視窗

Playwright 必須在點擊觸發 popup **之前**，先監聽 `context` 的 `popup` 事件：

```typescript
import { test, expect } from '@playwright/test'

test('捕捉 popup 並在其中操作', async ({ page, context }) => {
  await page.goto('https://shop.example.com/product/1')

  // ✅ 正確做法：使用 Promise.all 同時等待 popup 事件與點擊動作
  const [popup] = await Promise.all([
    context.waitForEvent('page'),  // 等待新 popup/page 出現
    page.getByRole('button', { name: '查看放大圖' }).click(),
  ])

  // 等待 popup 頁面載入
  await popup.waitForLoadState('domcontentloaded')

  // 驗證 popup 內容
  await expect(popup).toHaveURL(/product-image/)
  await expect(popup.getByRole('img')).toBeVisible()

  // 在 popup 中操作
  await popup.getByRole('button', { name: '關閉' }).click()

  // 驗證 popup 已關閉
  await expect(popup.isClosed()).resolves.toBe(true)
})
```

### 常見情境：點擊連結開啟新視窗

```typescript
test('點擊「在新視窗開啟」連結', async ({ page, context }) => {
  await page.goto('https://doc.example.com')

  // 捕捉新視窗並驗證
  const [newWindow] = await Promise.all([
    context.waitForEvent('page'),
    page.getByRole('link', { name: '查看完整說明（新視窗）' }).click(),
  ])

  await newWindow.waitForLoadState('networkidle')

  // 驗證新視窗的 URL 與標題
  await expect(newWindow).toHaveURL(/docs\/full/)
  await expect(newWindow).toHaveTitle(/說明文件/)

  await newWindow.close()

  // 確認原視窗仍可操作
  await expect(page.getByRole('main')).toBeVisible()
})
```

**最佳實踐**

- 永遠用 `Promise.all()` 同時啟動「等待事件」與「觸發動作」，避免 popup 在監聽前就出現
- 使用 `popup.waitForLoadState()` 確保 popup 完全載入後再操作
- popup 關閉後驗證原頁面狀態是否正確

**常見錯誤**

| ❌ 錯誤 | ✅ 正確 |
|--------|--------|
| 先 `click()` 再 `waitForEvent('page')` | `Promise.all()` 同時執行兩者 |
| 忘記 `waitForLoadState()` 就直接操作 popup | 先等待載入再操作 |

---

&nbsp;

## 🍀 0407 - 多分頁

### 多分頁基本操作

在同一個 `BrowserContext` 中，可以同時開啟多個 `Page`（分頁），這些分頁**共用 Cookie 與 Storage**。

```typescript
import { test, expect } from '@playwright/test'

test('在同一 Context 中操作多個分頁', async ({ context }) => {
  // 開啟兩個分頁
  const page1 = await context.newPage()
  const page2 = await context.newPage()

  // 兩個分頁導航到不同頁面
  await page1.goto('https://shop.example.com/cart')
  await page2.goto('https://shop.example.com/wishlist')

  // 各自驗證
  await expect(page1.getByRole('heading', { name: '購物車' })).toBeVisible()
  await expect(page2.getByRole('heading', { name: '收藏清單' })).toBeVisible()

  await page1.close()
  await page2.close()
})
```

### 切換分頁

```typescript
test('列出所有分頁並切換', async ({ context, page }) => {
  // 目前 context 只有一個分頁
  await page.goto('https://shop.example.com')

  // 透過 JS 由頁面本身開啟新分頁（模擬使用者點擊）
  const [newPage] = await Promise.all([
    context.waitForEvent('page'),
    page.getByRole('link', { name: '活動專區' }).click(),
  ])

  await newPage.waitForLoadState('domcontentloaded')

  // 取得所有分頁
  const pages = context.pages()
  console.log('目前分頁數：', pages.length) // 2

  // 切換回第一個分頁
  await pages[0].bringToFront()
  await expect(pages[0]).toHaveURL('https://shop.example.com')

  // 切換到第二個分頁
  await pages[1].bringToFront()
  await expect(pages[1]).toHaveURL(/promo/)
})
```

### 同步操作多分頁

```typescript
test('同步驗證多分頁狀態', async ({ context }) => {
  // 模擬使用者在多個分頁間操作購物流程
  const cartPage = await context.newPage()
  const productPage = await context.newPage()

  // 在商品頁加入購物車
  await productPage.goto('https://shop.example.com/product/42')
  await productPage.getByRole('button', { name: '加入購物車' }).click()
  await expect(productPage.getByTestId('cart-count')).toHaveText('1')

  // 切換到購物車頁，因為同一 Context，Cookie 共用，購物車應同步
  await cartPage.goto('https://shop.example.com/cart')
  await expect(cartPage.getByTestId('cart-item-count')).toHaveText('1')
  await expect(cartPage.getByText('商品 #42')).toBeVisible()

  await cartPage.close()
  await productPage.close()
})

test('不同 Context 的分頁不共用狀態', async ({ browser }) => {
  // 兩個獨立 Context
  const contextA = await browser.newContext()
  const contextB = await browser.newContext()

  const pageA = await contextA.newPage()
  const pageB = await contextB.newPage()

  // Context A 登入
  await pageA.goto('https://shop.example.com/login')
  await pageA.getByLabel('Email').fill('user@example.com')
  await pageA.getByLabel('密碼').fill('password123')
  await pageA.getByRole('button', { name: '登入' }).click()

  // Context B 未登入，應無法存取個人頁面
  await pageB.goto('https://shop.example.com/profile')
  await expect(pageB).toHaveURL(/login/)

  await contextA.close()
  await contextB.close()
})
```

**最佳實踐**

- 需要共用登入狀態（如「在多個分頁購物」）→ 使用同一個 Context 的多個 Page
- 需要隔離狀態（如「多角色同時測試」）→ 使用多個 Context
- 使用 `context.pages()` 取得所有分頁，搭配 `bringToFront()` 切換焦點

**常見錯誤**

| ❌ 錯誤 | ✅ 正確 |
|--------|--------|
| 誤以為不同 Page 的 Cookie 自動隔離 | 同 Context 下的 Page 共用 Cookie |
| 用索引存取分頁（`pages[0]`）但順序可能改變 | 用有意義的變數名稱指向特定 Page |
| 忘記關閉多餘的 Page / Context 造成資源洩漏 | 測試結束時確實 `close()` 所有物件 |

---

&nbsp;
