# 💫 Chapter 10 — Page Object Model（POM）

> 學會用可維護的頁面抽象管理 locator、操作流程與元件組合，並理解 POM 不等於只能用 class。

&nbsp;

## 目錄

- [💫 Chapter 10 — Page Object Model（POM）](#-chapter-10--page-object-modelpom)
  - [目錄](#目錄)
  - [🍀 1001 - POM 概念](#-1001---pom-概念)
    - [POM 在解決什麼問題](#pom-在解決什麼問題)
    - [POM 的核心責任](#pom-的核心責任)
    - [POM 實戰範例：從散落 selector 到頁面抽象](#pom-實戰範例從散落-selector-到頁面抽象)
    - [POM 最佳實踐](#pom-最佳實踐)
    - [POM 常見錯誤](#pom-常見錯誤)
  - [🍀 1002 - page abstraction with class and functional](#-1002---page-abstraction-with-class-and-functional)
    - [兩種表達方式的共同原則](#兩種表達方式的共同原則)
    - [class 與 functional 的差異](#class-與-functional-的差異)
    - [對照範例骨架：Login Page](#對照範例骨架login-page)
      - [class 版本](#class-版本)
      - [functional 版本](#functional-版本)
      - [在測試中的使用方式](#在測試中的使用方式)
    - [page abstraction 最佳實踐](#page-abstraction-最佳實踐)
    - [page abstraction 常見錯誤](#page-abstraction-常見錯誤)
  - [🍀 1003 - reusable methods](#-1003---reusable-methods)
    - [什麼叫可重用方法](#什麼叫可重用方法)
    - [好方法與壞方法的差異](#好方法與壞方法的差異)
    - [reusable methods 實戰範例：搜尋與加入購物車](#reusable-methods-實戰範例搜尋與加入購物車)
      - [class 版本骨架](#class-版本骨架)
      - [functional 版本骨架](#functional-版本骨架)
      - [測試使用方式](#測試使用方式)
    - [reusable methods 最佳實踐](#reusable-methods-最佳實踐)
    - [reusable methods 常見錯誤](#reusable-methods-常見錯誤)
  - [🍀 1004 - component object](#-1004---component-object)
    - [Page Object 與 Component Object 的分工](#page-object-與-component-object-的分工)
    - [component object 常見拆分對象](#component-object-常見拆分對象)
    - [component object 對照骨架：ProductCard 組合進 ProductListPage](#component-object-對照骨架productcard-組合進-productlistpage)
      - [class 版本骨架](#class-版本骨架-1)
      - [functional 版本骨架](#functional-版本骨架-1)
    - [component object 最佳實踐](#component-object-最佳實踐)
    - [component object 常見錯誤](#component-object-常見錯誤)
  - [🍀 1005 - boundaries and anti-pattern](#-1005---boundaries-and-anti-pattern)
    - [POM 與其他層的邊界](#pom-與其他層的邊界)
    - [哪些 assertion 可以放進 POM](#哪些-assertion-可以放進-pom)
    - [anti-pattern 清單](#anti-pattern-清單)
    - [boundaries and anti-pattern 最佳實踐](#boundaries-and-anti-pattern-最佳實踐)
    - [boundaries and anti-pattern 常見錯誤](#boundaries-and-anti-pattern-常見錯誤)
  - [⚡更多細節](#更多細節)
    - [🍀 1002.01 - 何時選 class，何時選 functional](#-100201---何時選-class何時選-functional)
      - [如何做風格選型](#如何做風格選型)
    - [🍀 1004.01 - 何時該抽 component object](#-100401---何時該抽-component-object)
      - [判斷是否值得抽象的檢查點](#判斷是否值得抽象的檢查點)
    - [🍀 1005.01 - POM / fixture / helper / data / spec 邊界判斷](#-100501---pom--fixture--helper--data--spec-邊界判斷)
      - [快速判斷表](#快速判斷表)

---

&nbsp;

## 🍀 1001 - POM 概念

POM 的核心不是「把測試改寫成 class」，而是把頁面操作抽成穩定、可維護的介面。當 locator、點擊流程、表單操作都散落在測試檔裡時，畫面一改，很多測試會一起壞。POM 要解決的就是這種維護成本。

### POM 在解決什麼問題

| 問題 | 沒有 POM 時的現象 | 有 POM 後的改善 |
|------|------------------|------------------|
| selector 四處散落 | 多個 spec 重複寫同一組 locator | selector 集中管理 |
| 頁面流程重複 | 每支測試都重寫登入、搜尋、送出 | 封裝成可重用方法 |
| DOM 改版影響大 | 改一個按鈕 class，十幾支測試壞掉 | 多半只需改 page file |
| 測試可讀性差 | 測試充滿 low-level click / fill | 更接近使用者意圖 |

### POM 的核心責任

```txt
POM = 封裝頁面結構 + 封裝 locator + 提供頁面操作 API
```

理解：

- spec 負責描述測試案例、組合流程、驗證結果。
- POM 負責提供頁面或元件的操作介面。
- POM 不等於只能用 class；functional 也能實作 POM。
- POM 不是把所有 helper、fixture、data builder 都塞進同一個檔案。

### POM 實戰範例：從散落 selector 到頁面抽象

先看沒有抽象時的寫法：

```ts
import { test, expect } from '@playwright/test'

test('使用者可以登入後看到 dashboard', async ({ page }) => {
  await page.goto('https://example.com/login')
  await page.getByLabel('Email').fill('alice@example.com')
  await page.getByLabel('Password').fill('P@ssw0rd123')
  await page.getByRole('button', { name: '登入' }).click()

  await expect(page).toHaveURL(/dashboard/)
  await expect(page.getByRole('heading', { name: '控制台' })).toBeVisible()
})
```

當同樣流程在很多測試重複出現時，就值得抽成 page abstraction：

```ts
import { test, expect } from '@playwright/test'
import { LoginPage } from '../pages/login-page'

test('使用者可以登入後看到 dashboard', async ({ page }) => {
  const loginPage = new LoginPage(page)

  await loginPage.goto()
  await loginPage.login('alice@example.com', 'P@ssw0rd123')

  await expect(page).toHaveURL(/dashboard/)
  await expect(page.getByRole('heading', { name: '控制台' })).toBeVisible()
})
```

### POM 最佳實踐

- 抽象的目的是減少重複與集中變更點，不是為了「看起來很進階」。
- 先抽最常重複的頁面與流程，不必一開始就把全站都做成 POM。
- Page file 應優先暴露使用者意圖，例如 `login()`、`submitSearch()`。
- 讓 spec 保留業務驗證，測試可讀性會更好。
- 如果流程只出現一次且很短，不一定要硬抽成 POM。

### POM 常見錯誤

| 錯誤寫法 | 問題 | 正確方向 |
|---------|------|----------|
| 把 POM 理解成 class 教學 | 會讓讀者以為 functional 不算 POM | 先教模式，再教語法表達 |
| 全站所有操作都塞進一個 page file | 形成 God object | 按頁面或元件邊界拆分 |
| 只抽 locator，不抽有語意的方法 | 可讀性提升有限 | 暴露 domain action |
| 每個案例都先做 POM | 可能過度設計 | 從高重複流程開始抽象 |

---

&nbsp;

## 🍀 1002 - page abstraction with class and functional

同一個 POM 模式，可以用 class，也可以用 functional factory 來表達。兩者都能做到封裝 locator 與操作，只是組織方式不同。

### 兩種表達方式的共同原則

不管你用哪一種風格，都應該滿足這些原則：

- 綁定 `page` 或特定 root locator。
- 封裝 locators，而不是讓 spec 到處自己找元素。
- 提供有語意的操作方法，而不是只暴露一堆原始 locator。
- 讓測試流程更接近使用者意圖，而不是更接近 DOM 細節。

### class 與 functional 的差異

| 風格 | 特色 | 適合情境 |
|------|------|----------|
| class style | 結構明確、與官方範例接近、搭配 fixture 直覺 | 大型專案、多人協作、偏 OOP 組織 |
| functional style | 輕量、貼近現代前端常見 factory 寫法 | 偏函數式團隊、喜歡閉包與組合 |

### 對照範例骨架：Login Page

以下示範同一個登入頁如何用兩種方式表達。

#### class 版本

```ts
import type { Locator, Page } from '@playwright/test'

export class LoginPage {
  readonly page: Page
  readonly emailInput: Locator
  readonly passwordInput: Locator
  readonly submitButton: Locator

  constructor(page: Page) {
    this.page = page
    this.emailInput = page.getByLabel('Email')
    this.passwordInput = page.getByLabel('Password')
    this.submitButton = page.getByRole('button', { name: '登入' })
  }

  async goto() {
    await this.page.goto('https://example.com/login')
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email)
    await this.passwordInput.fill(password)
    await this.submitButton.click()
  }

  async waitUntilReady() {
    await this.submitButton.waitFor()
  }
}
```

#### functional 版本

```ts
import type { Page } from '@playwright/test'

export function createLoginPage(page: Page) {
  const emailInput = page.getByLabel('Email')
  const passwordInput = page.getByLabel('Password')
  const submitButton = page.getByRole('button', { name: '登入' })

  return {
    goto: async () => {
      await page.goto('https://example.com/login')
    },

    login: async (email: string, password: string) => {
      await emailInput.fill(email)
      await passwordInput.fill(password)
      await submitButton.click()
    },

    waitUntilReady: async () => {
      await submitButton.waitFor()
    },
  }
}
```

#### 在測試中的使用方式

```ts
import { test, expect } from '@playwright/test'
import { LoginPage } from '../pages/login-page'
import { createLoginPage } from '../pages/login-page-functional'

test('class 與 functional 都能表達同一個登入頁抽象', async ({ page }) => {
  const loginPage = new LoginPage(page)
  await loginPage.goto()
  await loginPage.login('alice@example.com', 'P@ssw0rd123')
  await expect(page).toHaveURL(/dashboard/)
})

test('functional 版本同樣可維護', async ({ page }) => {
  const loginPage = createLoginPage(page)
  await loginPage.goto()
  await loginPage.login('alice@example.com', 'P@ssw0rd123')
  await expect(page).toHaveURL(/dashboard/)
})
```

### page abstraction 最佳實踐

- 先定義共同規則，再選語法風格。
- class 與 functional 不必混在同一檔案；可以按團隊慣例擇一。
- 若團隊大量使用 fixture 注入，class 常更直覺。
- 若團隊普遍採 factory function 與 composition，functional 會更順手。
- 兩者都應優先暴露 `login()`、`search()` 這種語意方法。

### page abstraction 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 把 class 與 functional 當成兩套不同模式 | 其實只是表達方式不同 | 先統一抽象標準 |
| functional 只回傳一堆 locator | 最後還是測試自己操作 DOM | 回傳有語意的方法 |
| class 裡全部公開 raw locator | page file 變成 selector 倉庫 | 封裝為方法或有限公開 |
| 同專案隨意混風格且無規範 | 會增加維護成本 | 先定義團隊慣例 |

---

&nbsp;

## 🍀 1003 - reusable methods

好的 POM 不只是在「搬運 locator」，還要提供真正可重用的方法。這些方法應貼近使用者意圖，而不是每一步都只是薄薄包一層。

### 什麼叫可重用方法

可重用方法通常有兩個特徵：

1. 它對應一個明確的頁面意圖。
2. 它會在多個測試中重複出現。

例如：

```txt
login(email, password)
addItemToCart(productName)
applyCoupon(code)
submitSearch(keyword)
```

### 好方法與壞方法的差異

| 類型 | 範例 | 問題或優點 |
|------|------|-----------|
| 壞方法 | `clickLoginButton()` | 太底層，沒有表達完整意圖 |
| 壞方法 | `fillEmail()`、`fillPassword()` | 容易讓 spec 重新拼裝流程 |
| 好方法 | `login(email, password)` | 接近使用者行為與業務意圖 |
| 好方法 | `searchProduct(keyword)` | 可重用且可讀性高 |

### reusable methods 實戰範例：搜尋與加入購物車

#### class 版本骨架

```ts
import type { Locator, Page } from '@playwright/test'

export class ProductsPage {
  readonly page: Page
  readonly searchInput: Locator
  readonly searchButton: Locator

  constructor(page: Page) {
    this.page = page
    this.searchInput = page.getByPlaceholder('搜尋商品')
    this.searchButton = page.getByRole('button', { name: '搜尋' })
  }

  async goto() {
    await this.page.goto('https://example.com/products')
  }

  async searchProduct(keyword: string) {
    await this.searchInput.fill(keyword)
    await this.searchButton.click()
  }

  async addItemToCart(productName: string) {
    const card = this.page.getByRole('article', { name: new RegExp(productName, 'i') })
    await card.getByRole('button', { name: '加入購物車' }).click()
  }
}
```

#### functional 版本骨架

```ts
import type { Page } from '@playwright/test'

export function createProductsPage(page: Page) {
  const searchInput = page.getByPlaceholder('搜尋商品')
  const searchButton = page.getByRole('button', { name: '搜尋' })

  return {
    goto: async () => {
      await page.goto('https://example.com/products')
    },

    searchProduct: async (keyword: string) => {
      await searchInput.fill(keyword)
      await searchButton.click()
    },

    addItemToCart: async (productName: string) => {
      const card = page.getByRole('article', { name: new RegExp(productName, 'i') })
      await card.getByRole('button', { name: '加入購物車' }).click()
    },
  }
}
```

#### 測試使用方式

```ts
import { test, expect } from '@playwright/test'
import { createProductsPage } from '../pages/products-page-functional'

test('搜尋後可將指定商品加入購物車', async ({ page }) => {
  const productsPage = createProductsPage(page)

  await productsPage.goto()
  await productsPage.searchProduct('機械式鍵盤')
  await productsPage.addItemToCart('機械式鍵盤')

  await expect(page.getByRole('status')).toContainText('已加入購物車')
})
```

### reusable methods 最佳實踐

- 優先抽象重複流程，不是所有操作都要包。
- 方法名稱應反映業務意圖，不只反映 DOM 動作。
- 對同一頁會反覆出現的操作，抽成方法能顯著降低重複碼。
- 若方法變得太長，代表可能混入不屬於該頁的責任。
- 可接受少量頁面 readiness 檢查，但主要斷言仍應留在 spec。

### reusable methods 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 每個點擊都包成單一方法 | 增加抽象層卻沒提升意義 | 封裝完整意圖 |
| `login()` 內塞滿 business assertions | POM 與 spec 邊界模糊 | 主要驗證留在 spec |
| 一個方法跨多個頁面亂跳 | 責任邊界不清 | 按頁面或流程分工 |

---

&nbsp;

## 🍀 1004 - component object

當頁面中有重複出現、且有獨立互動責任的區塊時，應考慮抽成 component object。這樣比把所有東西都塞進單一 page object 更穩定，也更容易重用。

### Page Object 與 Component Object 的分工

| 類型 | 代表範圍 | 例子 |
|------|----------|------|
| Page Object | 整頁流程 | LoginPage、CheckoutPage、OrdersPage |
| Component Object | 可重複使用的局部區塊 | Header、Modal、Sidebar、ProductCard |

### component object 常見拆分對象

```txt
Header
Sidebar
Modal
ProductCard
DatePicker
Toast
```

如果某個區塊在不同頁面都出現、而且有明確互動責任，就很適合獨立。

### component object 對照骨架：ProductCard 組合進 ProductListPage

#### class 版本骨架

```ts
import type { Locator, Page } from '@playwright/test'

export class ProductCard {
  readonly root: Locator

  constructor(root: Locator) {
    this.root = root
  }

  async addToCart() {
    await this.root.getByRole('button', { name: '加入購物車' }).click()
  }

  async openDetail() {
    await this.root.getByRole('link', { name: /查看詳情/i }).click()
  }
}

export class ProductListPage {
  readonly page: Page

  constructor(page: Page) {
    this.page = page
  }

  cardByName(productName: string) {
    const root = this.page.getByRole('article', { name: new RegExp(productName, 'i') })
    return new ProductCard(root)
  }
}
```

#### functional 版本骨架

```ts
import type { Locator, Page } from '@playwright/test'

export function createProductCard(root: Locator) {
  return {
    addToCart: async () => {
      await root.getByRole('button', { name: '加入購物車' }).click()
    },

    openDetail: async () => {
      await root.getByRole('link', { name: /查看詳情/i }).click()
    },
  }
}

export function createProductListPage(page: Page) {
  return {
    cardByName: (productName: string) => {
      const root = page.getByRole('article', { name: new RegExp(productName, 'i') })
      return createProductCard(root)
    },
  }
}
```

### component object 最佳實踐

- 優先抽可重用、可辨識、責任獨立的 UI 區塊。
- 組合通常比繼承更穩定。
- component object 應盡量只知道自己的 root 與局部操作。
- Page object 負責整頁流程，component object 負責局部互動。
- 避免為了「切很多檔」而過度拆分。

### component object 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 所有元素都放進單一 page object | 物件很快膨脹 | 對重複區塊抽 component object |
| component object 直接控制全頁 | 邊界失焦 | 只管理自己的 root |
| 為一次性區塊硬拆 component | 增加複雜度 | 先看是否真的重用 |

---

&nbsp;

## 🍀 1005 - boundaries and anti-pattern

Chapter 10 最容易出錯的地方，不是語法，而是責任邊界。若把 `POM`、`fixture`、`helper`、`data`、`spec` 混在一起，專案一大就會難維護。

### POM 與其他層的邊界

| 層 | 主要責任 | 不應負責什麼 |
|----|----------|---------------|
| POM | 封裝頁面結構與操作 | seed data、全域依賴注入、複雜商業驗證 |
| fixture | 提供測試前置、注入 page object、登入狀態 | 寫頁面 DOM 操作細節 |
| helper | 放純工具函式 | 綁定特定頁面 DOM |
| data | 建立測試資料、builder、faker、payload | 控制 UI 互動流程 |
| spec | 描述案例、驗證結果、組合流程 | 持續重複 selector 細節 |

### 哪些 assertion 可以放進 POM

這是最常被問到的問題。原則不是完全不能放，而是要分層。

| 類型 | 建議放哪裡 | 說明 |
|------|------------|------|
| 頁面 readiness | 可少量放在 POM | 例如等待主要表單可互動 |
| 元件是否已展開 | 視情況可放在 component object | 局部互動完成條件 |
| 業務結果驗證 | 優先放在 spec | 例如下單成功、折扣計算、權限結果 |
| 跨頁流程驗證 | 放在 spec | 更能反映測試意圖 |

範例：

```ts
async waitUntilReady() {
  await this.submitButton.waitFor()
}
```

上面這種 readiness 檢查通常可接受；但如果 `login()` 裡面直接寫十幾個 dashboard 驗證，就容易越界。

### anti-pattern 清單

| 反模式 | 問題 |
|--------|------|
| God object | 一個 page file 管整站功能，難維護也難定位 |
| selector 洩漏 | spec 自己重寫 locator，POM 失去集中管理價值 |
| assertion 雜燴 | Page file 混入大量業務驗證，責任不清 |
| abstraction 過度 | 每個 click 都包一層，但沒有更好理解 |
| functional 寫成 helper | 沒有綁定頁面邊界，最後只剩工具函式集合 |

### boundaries and anti-pattern 最佳實踐

- 先定義團隊的責任分工，再開始大量抽 POM。
- Page file 只應處理與該頁或該元件直接相關的互動。
- `fixture` 若要注入 page object，應把「建立」與「操作」責任分開。
- `helper` 優先保持純函式，不綁定特定頁面。
- 若某個 page file 越寫越大，先檢查是否應拆 component object。

### boundaries and anti-pattern 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 在 POM 內建立 faker 資料 | 混入資料責任 | 移到 data builder |
| 在 helper 中直接寫 `page.getByRole()` | helper 變相成 page object | 明確切回 POM |
| 所有驗證都藏進 page method | 測試意圖不清楚 | 主要結果留在 spec |
| 為方便把多頁流程都塞進單一 page | 長期難維護 | 依頁面或元件邊界拆分 |

---

&nbsp;

## ⚡更多細節

### 🍀 1002.01 - 何時選 class，何時選 functional

#### 如何做風格選型

可以依團隊慣例與專案結構做決策：

| 判斷面向 | 偏向 class | 偏向 functional |
|----------|------------|------------------|
| 團隊習慣 | 已大量使用 class / fixture 注入 | 已大量使用 factory / composition |
| 官方範例對齊 | 想更貼近 Playwright 文件 | 想更貼近前端函數式風格 |
| 組織方式 | 希望欄位與方法集中在同一型別 | 希望用閉包封裝局部狀態 |
| 心智成本 | 新成員熟悉 OOP | 團隊熟悉 functional API |

重點不是哪種「比較高級」，而是哪種更適合你目前的專案。

&nbsp;

### 🍀 1004.01 - 何時該抽 component object

#### 判斷是否值得抽象的檢查點

當你考慮抽 component object 時，可以先問自己：

1. 這個區塊是否在多個頁面重複出現？
2. 它是否有獨立互動責任，例如展開、收合、加入購物車、切換日期？
3. 它是否值得被當成一個獨立語意單位來閱讀？
4. 抽出後是否真的能降低重複碼與維護成本？

如果以上大多數答案都是「是」，那通常值得抽。

```ts
// 不一定要抽：只出現一次、沒有互動責任的靜態區塊
const legalText = page.getByText('使用條款')

// 比較值得抽：多處重複、具互動責任的商品卡片
const productCard = page.getByRole('article', { name: /機械式鍵盤/i })
```

&nbsp;

### 🍀 1005.01 - POM / fixture / helper / data / spec 邊界判斷

#### 快速判斷表

| 情境 | 應放哪裡 | 原因 |
|------|----------|------|
| `login(email, password)` | POM | 這是頁面操作 |
| 預先建立測試帳號資料 | data | 這是測試資料準備 |
| 在每個測試前自動注入已登入狀態 | fixture | 這是生命週期與依賴注入 |
| `formatCurrency()` | helper | 純工具函式，不綁定頁面 |
| 驗證登入後看到 dashboard heading | spec | 這是測試案例結果驗證 |

如果你猶豫某段邏輯應放哪裡，可以先問：

```txt
這段邏輯是在描述「頁面怎麼操作」，
還是在描述「測試想驗證什麼結果」？
```

前者通常偏向 POM，後者通常應留在 spec。

---

&nbsp;
