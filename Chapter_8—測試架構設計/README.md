# 💫 Chapter 8 — 測試架構設計

> 學會把 Playwright 測試專案設計成可維護、可平行擴張、可除錯的長期結構。

&nbsp;

## 目錄

- [💫 Chapter 8 — 測試架構設計](#-chapter-8--測試架構設計)
  - [目錄](#目錄)
  - [這章在解決什麼問題](#這章在解決什麼問題)
  - [這章與其他章節的邊界](#這章與其他章節的邊界)
  - [學習地圖](#學習地圖)
  - [🍀 0801 - 測試分層與責任邊界](#-0801---測試分層與責任邊界)
    - [為什麼先談分層](#為什麼先談分層)
    - [各層責任對照表](#各層責任對照表)
    - [實戰範例：從單檔測試走向分層結構](#實戰範例從單檔測試走向分層結構)
    - [測試分層與責任邊界最佳實踐](#測試分層與責任邊界最佳實踐)
    - [測試分層與責任邊界常見錯誤](#測試分層與責任邊界常見錯誤)
  - [🍀 0802 - 專案結構與模組切分](#-0802---專案結構與模組切分)
    - [常見資料夾切法](#常見資料夾切法)
    - [推薦專案骨架](#推薦專案骨架)
    - [實戰範例：按 domain 組合 spec、page、fixture 與 data](#實戰範例按-domain-組合-specpagefixture-與-data)
    - [專案結構與模組切分最佳實踐](#專案結構與模組切分最佳實踐)
    - [專案結構與模組切分常見錯誤](#專案結構與模組切分常見錯誤)
  - [🍀 0803 - test isolation](#-0803---test-isolation)
    - [test isolation 的核心觀念](#test-isolation-的核心觀念)
    - [哪些狀態最容易互相污染](#哪些狀態最容易互相污染)
    - [實戰範例：隔離登入狀態與購物車資料](#實戰範例隔離登入狀態與購物車資料)
    - [test isolation 最佳實踐](#test-isolation-最佳實踐)
    - [test isolation 常見錯誤](#test-isolation-常見錯誤)
  - [🍀 0804 - test lifecycle](#-0804---test-lifecycle)
    - [生命週期全貌](#生命週期全貌)
    - [不同階段適合放什麼](#不同階段適合放什麼)
    - [實戰範例：把 setup 放到正確層級](#實戰範例把-setup-放到正確層級)
    - [test lifecycle 最佳實踐](#test-lifecycle-最佳實踐)
    - [test lifecycle 常見錯誤](#test-lifecycle-常見錯誤)
  - [🍀 0805 - fixtures](#-0805---fixtures)
    - [fixture 是什麼](#fixture-是什麼)
    - [test-scoped 與 worker-scoped 的差異](#test-scoped-與-worker-scoped-的差異)
    - [實戰範例：自訂登入態與 API client fixture](#實戰範例自訂登入態與-api-client-fixture)
    - [fixtures 最佳實踐](#fixtures-最佳實踐)
    - [fixtures 常見錯誤](#fixtures-常見錯誤)
  - [🍀 0806 - hooks](#-0806---hooks)
    - [hooks 的定位](#hooks-的定位)
    - [何時該用 hooks，何時改用 fixture](#何時該用-hooks何時改用-fixture)
    - [實戰範例：用輕量 hooks 保持測試可讀性](#實戰範例用輕量-hooks-保持測試可讀性)
    - [hooks 最佳實踐](#hooks-最佳實踐)
    - [hooks 常見錯誤](#hooks-常見錯誤)
  - [🍀 0807 - parallel testing](#-0807---parallel-testing)
    - [平行執行真正的難點](#平行執行真正的難點)
    - [平行安全檢查表](#平行安全檢查表)
    - [實戰範例：依 worker 分配獨立測試資料](#實戰範例依-worker-分配獨立測試資料)
    - [parallel testing 最佳實踐](#parallel-testing-最佳實踐)
    - [parallel testing 常見錯誤](#parallel-testing-常見錯誤)
  - [🍀 0808 - flaky test prevention](#-0808---flaky-test-prevention)
    - [flaky test 的常見來源](#flaky-test-的常見來源)
    - [架構層面的預防策略](#架構層面的預防策略)
    - [實戰範例：從脆弱等待改成穩定策略](#實戰範例從脆弱等待改成穩定策略)
    - [flaky test prevention 最佳實踐](#flaky-test-prevention-最佳實踐)
    - [flaky test prevention 常見錯誤](#flaky-test-prevention-常見錯誤)
  - [⚡更多細節](#更多細節)
    - [🍀 0803.01 - isolation 設計檢查點](#-080301---isolation-設計檢查點)
      - [每次新增測試前先問的問題](#每次新增測試前先問的問題)
    - [🍀 0805.01 - fixture 與 hook 的快速判斷法](#-080501---fixture-與-hook-的快速判斷法)
      - [三個快速判斷問題](#三個快速判斷問題)
    - [🍀 0807.01 - 什麼時候該暫時改成 serial](#-080701---什麼時候該暫時改成-serial)
      - [serial 應該是最後手段](#serial-應該是最後手段)

---

&nbsp;

## 這章在解決什麼問題

很多人前幾章學完 `locator`、`assertion`、`network`、`auth` 之後，已經能寫出可跑的測試，但專案一大就會開始出現另一類問題：

| 你遇到的狀況 | 真正的根因 |
|--------------|------------|
| selector 到處散落、改版時一片爆炸 | 沒有明確分層與封裝 |
| `beforeEach` 越寫越肥 | setup 被放錯層級 |
| 平行執行後偶爾互相干擾 | isolation 不完整 |
| 有些測試一定要照順序跑 | 共用狀態或測試資料設計有問題 |
| 同一段登入、建資料、清資料邏輯一直重複 | 沒有 fixture 與 data layer 邊界 |
| 測試常常偶發失敗 | 架構沒有防範 flaky 的設計 |

所以這章不是在教你多一個 API，而是在教你：**如何把測試專案設計成可以長期維護的系統。**

&nbsp;

## 這章與其他章節的邊界

Chapter 8 很容易跟相鄰章節混在一起，所以先把責任講清楚：

| 章節 | 主要處理什麼 | Chapter 8 與它的關係 |
|------|--------------|-----------------------|
| Chapter 1 | 初始化、基本結構、config 入門 | Chapter 8 承接入門骨架，進一步談大型專案怎麼分層 |
| Chapter 6 | `waitForResponse()`、`route`、mock、fake backend | Chapter 8 只談 network 支援層應該放在哪裡 |
| Chapter 7 | auth、`storageState`、多角色 | Chapter 8 只談 auth 依賴如何被 fixture 與資料夾結構承接 |
| Chapter 9 | mock data、seed、cleanup、faker | Chapter 8 先定義 data layer 的位置與責任 |
| Chapter 10 | `POM`、component object、邊界 | Chapter 8 只定義 pages 層，不深入討論 POM 寫法 |
| Chapter 11 | debug、trace、video、network inspection | Chapter 8 只談如何從架構上減少除錯痛點 |
| Chapter 14 | CI/CD、artifact、report | Chapter 8 只談平行安全與專案內部結構 |
| Chapter 15 | test pyramid、risk-based、smoke | Chapter 8 談結構；Chapter 15 談策略 |

如果你把這章理解成「測試專案內部設計」，閱讀位置就會很清楚。

&nbsp;

## 學習地圖

這章最好的讀法不是背 API，而是按下面順序建立心智模型：

```txt
先知道每一層負責什麼
→ 再知道資料夾如何落地
→ 再理解每個 test 為什麼必須隔離
→ 再理解 setup / teardown 應放在哪個生命週期
→ 再學 fixture 與 hook 的分工
→ 最後才看 parallel 與 flaky prevention
```

你也可以把本章分成三段：

| 階段 | 主題 | 重點 |
|------|------|------|
| 架構骨架 | `0801`、`0802` | 定義責任邊界與專案形狀 |
| 執行模型 | `0803`、`0804`、`0805`、`0806` | 理解隔離、生命週期、fixture、hook |
| 擴張與穩定性 | `0807`、`0808` | 平行執行與抗 flaky 設計 |

---

&nbsp;

## 🍀 0801 - 測試分層與責任邊界

### 為什麼先談分層

專案一開始只有幾支測試時，很多東西都可以先寫在同一個 `spec` 裡。但當你開始有登入、購物車、搜尋、權限、mock data、API 支援層之後，如果沒有先講分層，後面的 `fixture`、`POM`、`data` 都只會變成不同檔名而已。

### 各層責任對照表

| 層 | 主要責任 | 不該放什麼 |
|----|----------|------------|
| `spec` | 描述案例、組合流程、寫 assertion | 大量 selector 細節、共用 setup 實作 |
| `page` / `component` | 封裝頁面操作與 locator | 複雜測試資料建構、全站共用工具 |
| `fixture` | 注入依賴、包 setup / teardown | 與單一頁面強綁定的 low-level 操作 |
| `data` | seed、builder、假資料工廠 | DOM 操作與頁面互動 |
| `helper` | 純工具函式、格式轉換、等待輔助 | 直接操作 `page` 的業務流程 |
| `auth` / `state` | 登入狀態、角色設定、`storageState` | 與單一案例綁死的驗證邏輯 |
| `api` | 測試支援用 API client、資料建立與清理 | 頁面上的 click / fill |

可以先記一句：

```txt
spec 說「要測什麼」
fixture 說「怎麼準備依賴」
page 說「怎麼操作頁面」
data 說「測什麼資料」
```

### 實戰範例：從單檔測試走向分層結構

先看一個容易失控的單檔測試：

```typescript
import { test, expect } from '@playwright/test'

test('管理員可以建立商品', async ({ page, request }) => {
  const email = `admin-${Date.now()}@example.com`

  await request.post('/api/testing/create-user', {
    data: {
      email,
      role: 'admin',
      password: 'P@ssw0rd123',
    },
  })

  await page.goto('/login')
  await page.getByLabel('Email').fill(email)
  await page.getByLabel('Password').fill('P@ssw0rd123')
  await page.getByRole('button', { name: '登入' }).click()

  await page.getByRole('link', { name: '商品管理' }).click()
  await page.getByLabel('商品名稱').fill('機械鍵盤')
  await page.getByLabel('價格').fill('3990')
  await page.getByRole('button', { name: '建立商品' }).click()

  await expect(page.getByText('建立成功')).toBeVisible()
})
```

這段能跑，但問題很多：

- 建立使用者資料的邏輯在 `spec`
- 登入流程在 `spec`
- selector 散落在 `spec`
- 未來如果登入 UI 改版，很多測試都要改

改成分層後，測試會更像下面這樣：

```typescript
import { expect } from '@playwright/test'
import { test } from '../fixtures/app-fixtures'
import { buildAdminUser } from '../data/users'

test('管理員可以建立商品', async ({ loginPage, productAdminPage, adminApi }) => {
  const admin = buildAdminUser()
  await adminApi.ensureUser(admin)

  await loginPage.goto()
  await loginPage.login(admin.email, admin.password)

  await productAdminPage.goto()
  await productAdminPage.createProduct({
    name: '機械鍵盤',
    price: 3990,
  })

  await expect(productAdminPage.successMessage).toBeVisible()
})
```

這時 `spec` 只留下案例意圖，維護成本會低很多。

### 測試分層與責任邊界最佳實踐

- 先定義責任，再決定資料夾名稱。
- 若一段邏輯在三支以上測試重複出現，就評估抽層。
- 讓 `spec` 保持可讀，盡量接近使用者流程。
- 讓資料準備與頁面操作分開，未來 debug 會輕鬆很多。
- POM、fixture、data builder 都是手段，目標是降低變更成本。

### 測試分層與責任邊界常見錯誤

| 錯誤寫法 | 問題 | 更好的做法 |
|----------|------|------------|
| 所有東西都先塞進 `spec` | 短期快，長期難維護 | 逐步抽出 page、fixture、data |
| 任何共用邏輯都丟進 `helper` | helper 會變成垃圾桶 | 先判斷是 page、fixture 還是 data 責任 |
| 在 `page` 內塞滿業務 assertion | 邊界模糊、難重用 | 驗證結果優先留在 `spec` |
| fixture 直接知道太多 DOM 細節 | 造成耦合 | fixture 注入依賴，頁面細節交給 page |

---

&nbsp;

## 🍀 0802 - 專案結構與模組切分

### 常見資料夾切法

目錄沒有唯一正解，但切法至少要能對應責任。常見做法如下：

| 資料夾 | 常見內容 | 說明 |
|--------|----------|------|
| `tests/` | `*.spec.ts` | 測試案例本體 |
| `pages/` | page object、component object | 頁面抽象層 |
| `fixtures/` | `base-test.ts`、自訂 fixture | 依賴注入層 |
| `data/` | builder、seed、mock payload | 測試資料層 |
| `api/` | 測試專用 API client | 建資料、清資料、支援流程 |
| `states/` 或 `auth/` | `storageState`、角色登入狀態 | 承接 auth/session |
| `helpers/` | 純工具函式 | 不依賴特定頁面 |

### 推薦專案骨架

```txt
.
├── tests/
│   ├── auth/
│   │   └── login.spec.ts
│   ├── products/
│   │   └── create-product.spec.ts
│   └── cart/
│       └── checkout.spec.ts
├── pages/
│   ├── login-page.ts
│   ├── product-admin-page.ts
│   └── components/
│       └── header.ts
├── fixtures/
│   ├── base-test.ts
│   └── app-fixtures.ts
├── data/
│   ├── users.ts
│   └── products.ts
├── api/
│   └── admin-api.ts
├── auth/
│   └── admin-storage-state.json
└── playwright.config.ts
```

這個骨架不是強制標準，但它有一個好處：**讀檔名就知道責任。**

### 實戰範例：按 domain 組合 spec、page、fixture 與 data

```typescript
// fixtures/app-fixtures.ts
import { test as base } from '@playwright/test'
import { LoginPage } from '../pages/login-page'
import { ProductAdminPage } from '../pages/product-admin-page'
import { AdminApi } from '../api/admin-api'

export const test = base.extend<{
  loginPage: LoginPage
  productAdminPage: ProductAdminPage
  adminApi: AdminApi
}>({
  loginPage: async ({ page }, use) => {
    await use(new LoginPage(page))
  },

  productAdminPage: async ({ page }, use) => {
    await use(new ProductAdminPage(page))
  },

  adminApi: async ({ request }, use) => {
    await use(new AdminApi(request))
  },
})
```

```typescript
// tests/products/create-product.spec.ts
import { expect } from '@playwright/test'
import { test } from '../../fixtures/app-fixtures'
import { buildAdminUser } from '../../data/users'

test('管理員可以建立商品', async ({ loginPage, productAdminPage, adminApi }) => {
  const admin = buildAdminUser()
  await adminApi.ensureUser(admin)

  await loginPage.goto()
  await loginPage.login(admin.email, admin.password)
  await productAdminPage.goto()

  await productAdminPage.createProduct({
    name: '降噪耳機',
    price: 5290,
  })

  await expect(productAdminPage.successMessage).toContainText('建立成功')
})
```

### 專案結構與模組切分最佳實踐

- 先按責任分，再按團隊習慣選擇依頁面或依 domain 排目錄。
- 如果專案有多角色，建議讓 `auth/` 或 `states/` 獨立存在。
- 不要讓 `tests/` 內同時混雜大量 page、helper、data 定義。
- 檔名應反映用途，例如 `admin-api.ts`、`login-page.ts`、`app-fixtures.ts`。
- 當一個資料夾開始裝太多無關內容，就表示責任切分還不夠清楚。

### 專案結構與模組切分常見錯誤

| 錯誤寫法 | 問題 | 更好的做法 |
|----------|------|------------|
| 只有 `tests/`，所有東西都放裡面 | 專案放大後很難找檔案 | 抽出 `pages/`、`fixtures/`、`data/` 等層 |
| `pages/` 內放 seed data | 命名與責任不一致 | 測試資料獨立到 `data/` |
| `helpers/` 內直接操作 `page` | helper 與 page 邊界模糊 | 頁面互動回到 page object |
| 檔名太抽象，如 `common.ts` | 很難知道用途 | 用語意化檔名 |

---

&nbsp;

## 🍀 0803 - test isolation

### test isolation 的核心觀念

`test isolation` 的核心不是「每支測試不要互相看對方」，而是：

```txt
任何一支測試都應能獨立執行
不依賴前一支測試先跑過
就算平行執行，也不會共用可變狀態
```

Playwright 預設已經幫你隔離 `BrowserContext`，但真實專案裡還有很多狀態會互相污染。

### 哪些狀態最容易互相污染

| 汙染來源 | 例子 | 風險 |
|----------|------|------|
| 帳號與權限 | 多支測試共用同一個 admin | 平行執行時互相改資料 |
| 測試資料 | 共用同一筆訂單或商品 | 刪除、更新、結帳互撞 |
| `storageState` | 直接複用過期登入態 | 測試偶發失敗 |
| network mock | `route` 沒清乾淨 | 後續測試吃到前一支假回應 |
| fake backend state | 記憶體狀態沒 reset | 測試順序一變就壞 |

### 實戰範例：隔離登入狀態與購物車資料

```typescript
import { test, expect } from '@playwright/test'

function buildUser(index: number) {
  return {
    email: `buyer-${Date.now()}-${index}@example.com`,
    password: 'P@ssw0rd123',
  }
}

test('每支測試使用自己的帳號與購物車資料', async ({ page, request }, testInfo) => {
  const user = buildUser(testInfo.parallelIndex)

  await request.post('/api/testing/create-user', {
    data: {
      email: user.email,
      password: user.password,
      role: 'buyer',
    },
  })

  await request.post('/api/testing/reset-cart', {
    data: {
      email: user.email,
    },
  })

  await page.goto('/login')
  await page.getByLabel('Email').fill(user.email)
  await page.getByLabel('Password').fill(user.password)
  await page.getByRole('button', { name: '登入' }).click()

  await page.goto('/products')
  await page.getByRole('button', { name: '加入購物車' }).first().click()
  await page.getByRole('link', { name: '購物車' }).click()

  await expect(page.getByText('共 1 項商品')).toBeVisible()
})
```

這個例子重點不在 API，而在於：**每支測試使用自己的資料，不跟別人搶。**

### test isolation 最佳實踐

- 讓每個 test 能單獨跑、重跑、平行跑。
- 優先建立可重設的測試資料，而不是共用一組長壽資料。
- 每次註冊 `route` 或 fake backend 時，都要思考清理策略。
- `storageState` 適合加速登入，但仍要注意角色與有效期。
- 若測試一定依賴前置資料，優先用 API 建立，而不是依賴另一支測試先跑。

### test isolation 常見錯誤

| 錯誤寫法 | 問題 | 更好的做法 |
|----------|------|------------|
| 測試 A 建商品、測試 B 接著編輯 | 形成順序依賴 | 每支測試自行準備資料 |
| 所有人共用同一組 admin 帳號 | 平行執行互撞 | 使用角色池或動態建立帳號 |
| 只相信 Playwright 自帶隔離 | 忽略後端資料與 mock 狀態 | 一起考慮外部狀態 |
| `route` 註冊後不清理 | 影響後續案例 | 讓 mock 範圍明確且可重置 |

---

&nbsp;

## 🍀 0804 - test lifecycle

### 生命週期全貌

在 Playwright Test 中，setup 與 teardown 不是只有 `beforeEach` 和 `afterEach`。完整心智模型更像這樣：

```txt
global setup
→ worker 啟動
→ beforeAll
→ beforeEach
→ test 本體
→ afterEach
→ afterAll
→ worker 結束
→ global teardown
```

如果你不知道現在的準備工作該放在哪一層，通常就會把所有事都塞進 `beforeEach`，最後拖慢速度，也讓問題更難追。

### 不同階段適合放什麼

| 階段 | 適合放的事 | 不適合放的事 |
|------|------------|--------------|
| global setup | 全專案共用的初始準備 | 與單一測試高度耦合的資料 |
| worker-scoped fixture | 同一 worker 可共用的昂貴依賴 | 會在不同測試間互相污染的狀態 |
| `beforeAll` | 同一 `describe` 區塊共用的前置 | 需要每支測試重置的資料 |
| `beforeEach` | 輕量頁面初始化、導航 | 大量資料建置與複雜業務流程 |
| test body | 真正的案例流程與 assertion | 隱藏性的 setup |
| `afterEach` | 清理、附件、額外觀測資訊 | 補救前面沒做好隔離的主要手段 |

### 實戰範例：把 setup 放到正確層級

```typescript
import { test, expect } from '@playwright/test'

test.describe('商品搜尋', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/products')
  })

  test('可以用關鍵字搜尋', async ({ page }) => {
    await page.getByPlaceholder('搜尋商品').fill('鍵盤')
    await page.getByRole('button', { name: '搜尋' }).click()

    await expect(page.getByText('機械鍵盤')).toBeVisible()
  })

  test('可以切換價格排序', async ({ page }) => {
    await page.getByRole('combobox', { name: '排序方式' }).selectOption('price-desc')

    await expect(page.locator('[data-testid="product-price"]').first()).toContainText('5990')
  })
})
```

這裡 `beforeEach` 只做輕量導航。若你把建立測試資料、登入、mock 註冊、清理資料都混在這裡，生命週期會很快失控。

### test lifecycle 最佳實踐

- 先判斷資料的存活範圍，再決定放在哪個生命週期。
- 輕量、可重複、與案例高度相關的初始化，適合 `beforeEach`。
- 昂貴但穩定的依賴，優先考慮 worker-scoped fixture。
- 若 setup 影響測試可讀性，先思考是否該抽成 fixture。
- 每多加一層生命週期，都要思考 teardown 與污染風險。

### test lifecycle 常見錯誤

| 錯誤寫法 | 問題 | 更好的做法 |
|----------|------|------------|
| 所有事都丟 `beforeEach` | 測試慢且難 debug | 依作用域拆到 fixture / API 準備層 |
| `beforeAll` 建了會被修改的共用資料 | 測試間互相影響 | 改成每支測試自建資料 |
| 在 test body 偷偷做大量 setup | 案例意圖不清楚 | setup 與案例流程適度分離 |
| 清理時機不明確 | 易殘留狀態 | 設計對應 teardown |

---

&nbsp;

## 🍀 0805 - fixtures

### fixture 是什麼

在 Playwright Test 裡，fixture 可以理解成「可注入的依賴」。它不只是資料容器，還能包 setup、teardown 與重用邏輯。

常見用途：

- 注入 page object
- 注入 API client
- 注入預先登入好的使用者狀態
- 注入測試資料建立器

### test-scoped 與 worker-scoped 的差異

| 類型 | 生命週期 | 適合情境 | 風險 |
|------|----------|----------|------|
| test-scoped | 每個 test 一份 | page object、短生命週期資料 | 成本較高但最安全 |
| worker-scoped | 同一 worker 共享 | 昂貴且可安全共用的依賴 | 若依賴可變狀態，易污染 |

### 實戰範例：自訂登入態與 API client fixture

```typescript
import { test as base, request, type APIRequestContext } from '@playwright/test'
import { LoginPage } from '../pages/login-page'

type AdminApi = {
  client: APIRequestContext
  ensureAdmin(email: string, password: string): Promise<void>
}

export const test = base.extend<{
  loginPage: LoginPage
  adminApi: AdminApi
}>({
  loginPage: async ({ page }, use) => {
    await use(new LoginPage(page))
  },

  adminApi: async ({ playwright, baseURL }, use) => {
    const client = await request.newContext({
      baseURL,
      extraHTTPHeaders: {
        'x-testing-token': 'local-dev-token',
      },
    })

    const api: AdminApi = {
      client,
      ensureAdmin: async (email: string, password: string) => {
        await client.post('/api/testing/create-user', {
          data: {
            email,
            password,
            role: 'admin',
          },
        })
      },
    }

    await use(api)
    await client.dispose()
  },
})
```

```typescript
import { expect } from '@playwright/test'
import { test } from '../fixtures/app-fixtures'

test('透過 fixture 注入頁面與 API 依賴', async ({ loginPage, adminApi }) => {
  const email = `admin-${Date.now()}@example.com`
  const password = 'P@ssw0rd123'

  await adminApi.ensureAdmin(email, password)
  await loginPage.goto()
  await loginPage.login(email, password)

  await expect(loginPage.page).toHaveURL(/dashboard/)
})
```

### fixtures 最佳實踐

- fixture 名稱應直接反映用途，例如 `loginPage`、`adminApi`、`authenticatedUser`。
- 預設優先選 test-scoped，只有確認安全時才升級成 worker-scoped。
- fixture 內可包 setup / teardown，但不要塞太多頁面細節。
- fixture 適合管理依賴，不適合承擔所有測試案例邏輯。
- 當同一組依賴在很多測試重複注入時，fixture 的價值最高。

### fixtures 常見錯誤

| 錯誤寫法 | 問題 | 更好的做法 |
|----------|------|------------|
| fixture 內直接跑完整業務流程 | spec 變得看不出案例意圖 | fixture 只提供依賴與必要 setup |
| 一開始全部做成 worker-scoped | 雖快但風險高 | 先 test-scoped，再評估成本 |
| fixture 名稱太抽象，如 `service` | 難理解內容 | 用明確語意命名 |
| fixture 依賴 page DOM 細節過深 | 和 page object 重疊 | DOM 操作回到 page 層 |

---

&nbsp;

## 🍀 0806 - hooks

### hooks 的定位

`hooks` 是用來輔助測試編排的，不是拿來取代架構分層的。你可以把它理解成：

```txt
hooks = 生命週期中的協調點
fixture = 真正的依賴注入與 setup 封裝工具
```

### 何時該用 hooks，何時改用 fixture

| 情境 | 優先選擇 | 原因 |
|------|----------|------|
| 每支測試都要先打開同一頁 | `beforeEach` | 輕量且直觀 |
| 需要注入 page object 或 API client | fixture | 是依賴，不只是前置步驟 |
| 需要做昂貴但可共用的準備 | worker-scoped fixture 或 `beforeAll` | 看是否要被注入使用 |
| 需要在失敗時附加額外資訊 | `afterEach` | 屬於生命週期後處理 |
| setup 太複雜、牽涉多個依賴 | fixture | 比 hook 更容易維護 |

### 實戰範例：用輕量 hooks 保持測試可讀性

```typescript
import { test, expect } from '@playwright/test'

test.describe('會員中心', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/account')
  })

  test.afterEach(async ({ page }, testInfo) => {
    if (testInfo.status !== testInfo.expectedStatus) {
      await testInfo.attach('account-page-url', {
        body: Buffer.from(page.url()),
        contentType: 'text/plain',
      })
    }
  })

  test('可以看到基本資料', async ({ page }) => {
    await expect(page.getByRole('heading', { name: '會員中心' })).toBeVisible()
  })
})
```

這個例子裡，`hooks` 只做兩件事：

- 進頁面
- 失敗時補充觀測資訊

它沒有偷藏登入流程、沒有偷建資料，所以測試仍然好讀。

### hooks 最佳實踐

- 只把輕量且通用的前後處理放在 hook。
- 若 hook 內容越來越像依賴建立器，就該考慮改成 fixture。
- 讓 hook 對測試讀者來說是可預期的，不要藏太多魔法。
- `afterEach` 適合補觀測資訊，不適合當主要清理策略。
- 一個 `describe` 內的 hook 應盡量聚焦在同一類場景。

### hooks 常見錯誤

| 錯誤寫法 | 問題 | 更好的做法 |
|----------|------|------------|
| `beforeEach` 內完成整個登入與建資料流程 | 測試意圖被隱藏 | 將依賴抽成 fixture，必要步驟留在 spec |
| hook 裡註冊太多 mock | 清理困難 | 把 mock 策略集中管理 |
| `afterEach` 負責收拾所有污染 | 只是補救，非根治 | 前面先做好 isolation |
| hook 層層巢狀過多 | 難追蹤執行順序 | 控制作用域與複雜度 |

---

&nbsp;

## 🍀 0807 - parallel testing

### 平行執行真正的難點

很多人以為平行測試就是把 `workers` 打開，但真正會出問題的往往是資料與環境，而不是 Playwright 本身。

### 平行安全檢查表

| 檢查點 | 問題範例 | 建議 |
|--------|----------|------|
| 帳號是否共用 | 多支測試同時修改個人資料 | 使用角色池或動態帳號 |
| 資料是否唯一 | 都在改同一筆商品 | 用唯一 ID 或動態 seed |
| 環境是否可重設 | fake backend 會累積狀態 | 每支測試或每個 worker 重置 |
| 是否依賴順序 | A 跑完才有 B 要的資料 | 每支測試自行準備資料 |
| 是否有共享檔案 | 同時寫同一個下載檔名 | 加入測試 ID 或 worker ID |

### 實戰範例：依 worker 分配獨立測試資料

```typescript
import { test, expect } from '@playwright/test'

test('每個 worker 使用自己的測試使用者', async ({ page, request }, testInfo) => {
  const workerId = testInfo.parallelIndex
  const email = `parallel-user-${workerId}-${Date.now()}@example.com`
  const password = 'P@ssw0rd123'

  await request.post('/api/testing/create-user', {
    data: {
      email,
      password,
      role: 'buyer',
    },
  })

  await page.goto('/login')
  await page.getByLabel('Email').fill(email)
  await page.getByLabel('Password').fill(password)
  await page.getByRole('button', { name: '登入' }).click()

  await expect(page).toHaveURL(/dashboard/)
})
```

這裡不是因為 `parallelIndex` 很神奇，而是因為它幫你把「每個 worker 要有自己的資源」具體化。

### parallel testing 最佳實踐

- 先確保 isolation，再談加速。
- 讓資料命名、檔案命名、帳號命名具唯一性。
- 盡量讓測試不依賴共享可變資源。
- 只有在不得已時才用 serial，且要註明原因。
- 平行測試的失敗，多半是架構問題，不是單純運氣差。

### parallel testing 常見錯誤

| 錯誤寫法 | 問題 | 更好的做法 |
|----------|------|------------|
| 一開專案就全面平行 | 問題會被放大 | 先做 isolation 檢查 |
| 用一組固定帳號跑全部測試 | 很容易互相改資料 | 改成多帳號池或動態建立 |
| 失敗就直接改成 serial | 掩蓋設計問題 | 先排查資料競爭與污染 |
| 共用下載、上傳或報表檔名 | 檔案互蓋 | 加測試唯一識別 |

---

&nbsp;

## 🍀 0808 - flaky test prevention

### flaky test 的常見來源

`flaky test` 不是單一問題，而是很多小問題累積後的症狀。

| 類型 | 常見現象 | 背後原因 |
|------|----------|----------|
| 等待不穩定 | 偶爾找不到元素 | 用錯等待策略、盲等 timeout |
| selector 脆弱 | UI 小改版就壞 | selector 沒集中、語意不穩定 |
| 資料互撞 | 平行時偶發失敗 | isolation 不完整 |
| 外部依賴不穩 | 第三方 API 偶發超時 | 對外部服務依賴太深 |
| 狀態殘留 | 單跑會過、全跑會壞 | route、storage、fake backend 沒清理 |

### 架構層面的預防策略

最有效的抗 flaky 方法，通常不是再多加一個 `waitForTimeout()`，而是從架構面先把風險降下來：

- selector 集中到 page / component 層
- 等待以 `expect()`、`waitForResponse()`、語意化 ready state 為主
- 測試資料可重建、可重置、可唯一識別
- 外部依賴能 mock 的就適度 mock
- hook、fixture、route、fake backend 都要有清楚作用域

### 實戰範例：從脆弱等待改成穩定策略

先看脆弱版本：

```typescript
import { test, expect } from '@playwright/test'

test('搜尋後顯示結果', async ({ page }) => {
  await page.goto('/products')
  await page.getByPlaceholder('搜尋商品').fill('鍵盤')
  await page.getByRole('button', { name: '搜尋' }).click()

  await page.waitForTimeout(2000)
  await expect(page.getByText('機械鍵盤')).toBeVisible()
})
```

更穩定的版本會改成：

```typescript
import { test, expect } from '@playwright/test'

test('搜尋後顯示結果', async ({ page }) => {
  await page.goto('/products')

  const responsePromise = page.waitForResponse((response) => {
    return response.url().includes('/api/products/search') && response.status() === 200
  })

  await page.getByPlaceholder('搜尋商品').fill('鍵盤')
  await page.getByRole('button', { name: '搜尋' }).click()

  await responsePromise
  await expect(page.getByText('機械鍵盤')).toBeVisible()
})
```

差別在於：後者等待的是「真正支撐 UI 更新的事件」。

### flaky test prevention 最佳實踐

- 把 flaky 當成設計警訊，而不是單純執行噪音。
- 能用語意 selector，就不要依賴脆弱 CSS 路徑。
- 能等真實事件，就不要盲等秒數。
- 把資料與狀態污染風險納入 code review。
- 若某類場景反覆 flaky，回頭檢查架構，不要只局部補洞。

### flaky test prevention 常見錯誤

| 錯誤寫法 | 問題 | 更好的做法 |
|----------|------|------------|
| 一失敗就加 `waitForTimeout()` | 治標不治本 | 找出真正等待條件 |
| 任何 locator 都直接寫在 spec | 變更點四散 | 集中到 page / component |
| 對外部真服務完全不設防 | 環境不穩定時大量假失敗 | 適度 mock 或隔離外部依賴 |
| 看到 flaky 就先 `retries++` | 只是在掩蓋症狀 | 先查資料、等待與共享狀態 |

---

&nbsp;

## ⚡更多細節

### 🍀 0803.01 - isolation 設計檢查點

#### 每次新增測試前先問的問題

新增一支測試時，可以先快速檢查這五件事：

1. 這支測試有沒有依賴別支測試先建立的資料？
2. 這支測試是否共用會被修改的帳號、訂單、商品？
3. 這支測試有沒有註冊 mock / route，且作用域是否清楚？
4. 這支測試是否能單獨重跑而不需要先執行整包？
5. 這支測試若改成平行執行，會不會和其他測試搶資源？

只要其中兩題以上回答「會」，通常就表示 isolation 還沒設計好。

&nbsp;

### 🍀 0805.01 - fixture 與 hook 的快速判斷法

#### 三個快速判斷問題

當你在想「這段要放 fixture 還是 hook」時，可以先問：

| 問題 | 若答案是是 | 優先選擇 |
|------|------------|----------|
| 這是要被測試函式直接使用的依賴嗎？ | 例如 page object、API client | fixture |
| 這只是生命週期中的輕量前後處理嗎？ | 例如導航、附加失敗資訊 | hook |
| 這段邏輯是否需要明確 setup / teardown 封裝？ | 例如建立 request context | fixture |

如果你還是不確定，通常先從 fixture 想會比較清楚，因為依賴注入的意圖比隱藏在 hook 裡更透明。

&nbsp;

### 🍀 0807.01 - 什麼時候該暫時改成 serial

#### serial 應該是最後手段

有些情境短期內真的很難平行安全，例如：

- 被測系統只有單一共享帳號，且無法快速補角色池
- 測試環境只有一份無法重設的排程資料
- 某些外部整合環境有嚴格頻率限制

這時可以暫時用 serial 保住穩定性，但應搭配以下做法：

- 註明為什麼不能平行
- 記錄未來要怎麼拆掉 serial 依賴
- 把 serial 範圍縮到最小，不要整包測試都跟著受影響

`serial` 可以救急，但不應成為長期架構。

---

&nbsp;
