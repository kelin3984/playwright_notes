# 💫 Chapter 6 — Network 與 API 控制

> 學會用 Playwright 觀察、等待、攔截與改寫網路請求，讓測試更穩定也更可控。

&nbsp;

## 目錄

- [💫 Chapter 6 — Network 與 API 控制](#-chapter-6--network-與-api-控制)
  - [目錄](#目錄)
  - [🍀 0601 - waitForResponse](#-0601---waitforresponse)
    - [waitForResponse 的核心觀念](#waitforresponse-的核心觀念)
    - [waitForResponse 常用 API](#waitforresponse-常用-api)
    - [waitForResponse 實戰範例：登入後等待使用者資料](#waitforresponse-實戰範例登入後等待使用者資料)
    - [waitForResponse 最佳實踐](#waitforresponse-最佳實踐)
    - [waitForResponse 常見錯誤](#waitforresponse-常見錯誤)
  - [🍀 0602 - route interception](#-0602---route-interception)
    - [route interception 的核心觀念](#route-interception-的核心觀念)
    - [route interception 常用 API](#route-interception-常用-api)
    - [route interception 實戰範例：阻擋第三方追蹤腳本](#route-interception-實戰範例阻擋第三方追蹤腳本)
    - [route interception 最佳實踐](#route-interception-最佳實踐)
    - [route interception 常見錯誤](#route-interception-常見錯誤)
  - [🍀 0603 - mock API](#-0603---mock-api)
    - [mock API 的核心觀念](#mock-api-的核心觀念)
    - [mock API 常用 API](#mock-api-常用-api)
    - [mock API 實戰範例：商品列表空狀態](#mock-api-實戰範例商品列表空狀態)
    - [mock API 最佳實踐](#mock-api-最佳實踐)
    - [mock API 常見錯誤](#mock-api-常見錯誤)
  - [🍀 0604 - modify response](#-0604---modify-response)
    - [modify response 的核心觀念](#modify-response-的核心觀念)
    - [modify response 常用 API](#modify-response-常用-api)
    - [modify response 實戰範例：在不改後端的情況下注入促銷資料](#modify-response-實戰範例在不改後端的情況下注入促銷資料)
    - [modify response 最佳實踐](#modify-response-最佳實踐)
    - [modify response 常見錯誤](#modify-response-常見錯誤)
  - [🍀 0605 - fake backend](#-0605---fake-backend)
    - [fake backend 的核心觀念](#fake-backend-的核心觀念)
    - [fake backend 常用 API](#fake-backend-常用-api)
    - [fake backend 實戰範例：購物車 CRUD 流程](#fake-backend-實戰範例購物車-crud-流程)
    - [fake backend 最佳實踐](#fake-backend-最佳實踐)
    - [fake backend 常見錯誤](#fake-backend-常見錯誤)
  - [🍀 0606 - network idle](#-0606---network-idle)
    - [network idle 的核心觀念](#network-idle-的核心觀念)
    - [network idle 常用 API](#network-idle-常用-api)
    - [network idle 實戰範例：搜尋結果頁面穩定等待](#network-idle-實戰範例搜尋結果頁面穩定等待)
    - [network idle 最佳實踐](#network-idle-最佳實踐)
    - [network idle 常見錯誤](#network-idle-常見錯誤)
  - [🍀 0607 - GraphQL testing](#-0607---graphql-testing)
    - [GraphQL testing 的核心觀念](#graphql-testing-的核心觀念)
    - [GraphQL testing 常用 API](#graphql-testing-常用-api)
    - [GraphQL testing 實戰範例：攔截產品查詢與 mutation](#graphql-testing-實戰範例攔截產品查詢與-mutation)
    - [GraphQL testing 最佳實踐](#graphql-testing-最佳實踐)
    - [GraphQL testing 常見錯誤](#graphql-testing-常見錯誤)
  - [🍀 0608 - WebSocket](#-0608---websocket)
    - [WebSocket 的核心觀念](#websocket-的核心觀念)
    - [WebSocket 常用 API](#websocket-常用-api)
    - [WebSocket 實戰範例：客服聊天室即時訊息](#websocket-實戰範例客服聊天室即時訊息)
    - [WebSocket 最佳實踐](#websocket-最佳實踐)
    - [WebSocket 常見錯誤](#websocket-常見錯誤)
  - [⚡更多細節](#更多細節)
    - [🍀 0602.01 - route matching 與清理策略](#-060201---route-matching-與-清理策略)
      - [為什麼攔截規則常讓測試互相污染](#為什麼攔截規則常讓測試互相污染)
    - [🍀 0604.01 - modify response 的一致性檢查](#-060401---modify-response-的-一致性檢查)
      - [哪些欄位改了最容易讓前端進入假成功狀態](#哪些欄位改了最容易讓前端進入假成功狀態)
    - [🍀 0605.01 - stateful fake backend 的設計方式](#-060501---stateful-fake-backend-的設計方式)
      - [如何讓 fake backend 既逼真又可重置](#如何讓-fake-backend-既逼真又可重置)
    - [🍀 0606.01 - 何時不該依賴 networkidle](#-060601---何時不該依賴-networkidle)
      - [比 `networkidle` 更穩定的替代策略](#比-networkidle-更穩定的替代策略)
    - [🍀 0607.01 - GraphQL 測試的 operationName 策略](#-060701---graphql-測試的-operationname-策略)
      - [同一支 `/graphql` API 為什麼不能只看 URL](#同一支-graphql-api-為什麼不能只看-url)
    - [🍀 0608.01 - WebSocket 的測試邊界與穩定性](#-060801---websocket-的測試邊界與穩定性)
      - [哪些即時功能適合驗證 UI，哪些應降到整合測試](#哪些即時功能適合驗證-ui哪些應降到整合測試)

---

&nbsp;

## 🍀 0601 - waitForResponse

`waitForResponse()` 的本質不是「等一下網路」，而是**等待一筆符合條件的回應真正完成**。當頁面操作會觸發 API，而且 UI 更新又依賴該 API 結果時，這種等待通常比 `waitForTimeout()` 或盲等 `networkidle` 更精準。

### waitForResponse 的核心觀念

| 重點 | 說明 |
|------|------|
| 等的是 response，不是 request | 只有回應真正回來、狀態碼與內容可讀時，等待才算完成 |
| 適合搭配使用者操作 | 最常見寫法是 `Promise.all()`：一邊點擊、一邊等待 API |
| 條件要夠明確 | 建議至少比對 URL、method、status，必要時再看 body |
| 用來解決 race condition | 避免「先點擊，後等待」導致事件已經發生而錯過 |

### waitForResponse 常用 API

| API | 用途 | 常見參數 / 寫法 |
|-----|------|------------------|
| `page.waitForResponse()` | 等待符合條件的回應 | `urlOrPredicate`, `timeout` |
| `response.url()` | 取得回應 URL | 常與字串比對搭配 |
| `response.status()` | 取得 HTTP status | `200`、`201`、`404` |
| `response.request().method()` | 取得對應 request method | `GET`、`POST` |
| `response.json()` | 解析 JSON body | 適合進一步驗證 payload |

```typescript
const response = await page.waitForResponse((response) => {
  return response.url().includes('/api/profile') && response.status() === 200
})

const data = await response.json()
expect(data.name).toBe('Alice')
```

### waitForResponse 實戰範例：登入後等待使用者資料

```typescript
import { test, expect } from '@playwright/test'

test('登入成功後顯示使用者名稱', async ({ page }) => {
  await page.goto('https://example.com/login')

  await page.getByLabel('Email').fill('alice@example.com')
  await page.getByLabel('Password').fill('P@ssw0rd123')

  const [profileResponse] = await Promise.all([
    page.waitForResponse((response) => {
      return (
        response.url().includes('/api/me') &&
        response.request().method() === 'GET' &&
        response.status() === 200
      )
    }),
    page.getByRole('button', { name: '登入' }).click(),
  ])

  const profile = await profileResponse.json()
  expect(profile.email).toBe('alice@example.com')

  await expect(page.getByRole('banner')).toContainText('Alice Chen')
  await expect(page).toHaveURL(/dashboard/)
})
```

### waitForResponse 最佳實踐

- 優先用 `Promise.all()` 同步管理「觸發動作」與「等待回應」。
- Predicate 至少帶入 `url + method + status`，避免誤抓其他同名 API。
- 等到回應後，最好再接 UI assertion，確認前端真的有消化該資料。
- 如果同頁面會發很多平行請求，將條件縮到最小，不要只寫 `includes('/api')`。
- 對 SPA 頁面，`waitForResponse()` 通常比 `waitForLoadState('networkidle')` 更精準。

### waitForResponse 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 先 `click()` 再 `waitForResponse()` | 可能錯過已經發出的回應 | 用 `Promise.all()` 同時開始 |
| 只判斷 URL | 同 URL 可能有多種 method 或不同狀態 | 加上 `method` 與 `status` |
| 拿到 response 就結束 | API 成功不代表 UI 已更新 | 後面再補 `expect()` 驗證畫面 |
| 用 `waitForTimeout(1000)` 取代 | 時間固定、不穩定、拖慢測試 | 直接等待真正的 API 完成 |

---

&nbsp;

## 🍀 0602 - route interception

`route interception` 是 Playwright 控制網路流量的核心能力。它能在 request 發出去之前攔截，決定要放行、阻擋、改寫或直接回傳假資料，因此不只適合測試，也很適合隔離第三方服務與製造邊界情境。

### route interception 的核心觀念

| 操作 | 代表意義 | 常見用途 |
|------|----------|----------|
| `continue()` | 放行原始 request | 僅在送出前改 header、method、postData |
| `abort()` | 直接中止 request | 模擬 CDN 掛掉、圖片下載失敗、第三方腳本失效 |
| `fulfill()` | 不打到真實後端，直接回應 | mock API、前端狀態測試、離線情境 |
| `fetch()` | 先向真後端取回 response，再自行改寫 | modify response、注入測試資料 |

### route interception 常用 API

| API | 用途 | 補充 |
|-----|------|------|
| `page.route(url, handler)` | 只攔截當前頁面 context 內的 request | 單頁測試常用 |
| `browserContext.route(url, handler)` | 對整個 context 生效 | 多頁面 / popup 流程常用 |
| `route.request()` | 取得被攔截的 request | 可讀 URL、headers、body |
| `route.continue()` | 放行 request | 可附帶改寫參數 |
| `route.abort()` | 取消 request | 可模擬失敗 |
| `route.fulfill()` | 直接回傳 response | 最常用於 mock |

```typescript
await page.route('**/analytics/**', async (route) => {
  await route.abort()
})
```

### route interception 實戰範例：阻擋第三方追蹤腳本

```typescript
import { test, expect } from '@playwright/test'

test('結帳流程不依賴第三方 analytics 也能完成', async ({ page }) => {
  await page.route('**/analytics/**', async (route) => {
    await route.abort()
  })

  await page.goto('https://example.com/checkout')

  await page.getByLabel('收件人').fill('王小明')
  await page.getByLabel('地址').fill('台北市信義區市府路 1 號')
  await page.getByRole('button', { name: '送出訂單' }).click()

  await expect(page.getByRole('heading', { name: '訂單成立' })).toBeVisible()
  await expect(page.getByText('感謝您的購買')).toBeVisible()
})
```

### route interception 最佳實踐

- 先問自己要驗證的是「放行」、「阻擋」還是「改寫」，不要一開始就全部用 `fulfill()`。
- 只攔截必要 URL，避免使用過大的 glob 影響其他 request。
- 若攔截只為特定測試存在，應在該測試範圍內定義，減少交叉污染。
- 第三方服務（analytics、chat widget、AB test）很適合用 `abort()` 隔離。
- 對多頁面流程，優先在 `context` 層攔截，而不是每個 page 都各自設定一次。

### route interception 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| `page.route('**/*', ...)` | 攔太大，容易誤傷 CSS、圖片、主 API | 只攔特定 domain 或 endpoint |
| 攔截寫在 `page.goto()` 之後 | 首波 request 可能已經出去了 | 先設 route，再進入頁面 |
| 一律用 `fulfill()` | 失去真後端驗證能力 | 視需求選擇 `continue` / `abort` / `fulfill` |
| 同一測試檔殘留 route | 可能影響後續案例 | 在適當時機 `unroute()` 或縮小作用域 |

---

&nbsp;

## 🍀 0603 - mock API

`mock API` 是在**不依賴真實後端資料**的前提下，建立可重現的前端情境。它特別適合測試空狀態、錯誤狀態、極端資料、尚未完成的 API，或想讓測試在 CI 中更穩定時使用。

### mock API 的核心觀念

| 情境 | 為什麼適合 mock |
|------|------------------|
| 空資料 / 無結果頁 | 真實環境不一定容易準備 |
| 500 / 401 / 429 等錯誤 | 真後端不容易安全地長期提供 |
| 邊界資料 | 很多系統不容易做 seed |
| 開發中功能 | 前端先行時可先穩定驗 UI |

### mock API 常用 API

| API | 用途 | 補充 |
|-----|------|------|
| `route.fulfill()` | 直接回假回應 | 最基本 mock 手法 |
| `status` | 設定 HTTP 狀態碼 | `200`、`500`、`401` |
| `contentType` | 設定回應型別 | `application/json` |
| `json` | 直接回傳 JSON 物件 | 適合 mock REST / GraphQL |
| `headers` | 自訂 response header | cache、CORS、trace id 等 |

### mock API 實戰範例：商品列表空狀態

```typescript
import { test, expect } from '@playwright/test'

test('商品列表為空時顯示 empty state', async ({ page }) => {
  await page.route('**/api/products?category=keyboard', async (route) => {
    await route.fulfill({
      status: 200,
      contentType: 'application/json',
      json: {
        items: [],
        total: 0,
      },
    })
  })

  await page.goto('https://example.com/products?category=keyboard')

  await expect(page.getByRole('heading', { name: '找不到符合條件的商品' })).toBeVisible()
  await expect(page.getByRole('button', { name: '清除篩選條件' })).toBeVisible()
})
```

### mock API 最佳實踐

- 把 mock 資料寫成接近真實 API 形狀，不要只回傳前端剛好用到的一兩個欄位。
- 每個測試只 mock 與情境直接相關的 request，避免整頁全部變成假世界。
- 對錯誤情境除了驗證錯誤訊息，也要驗證 retry、disable、fallback UI。
- 若多個測試都需要同一 mock，可抽成 helper，但仍要保留情境名稱。
- Mock 的目的應該是控制不確定性，不是掩蓋真正應該打整合測試的流程。

### mock API 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 回傳結構過度簡化 | 前端可能因欄位缺失走到不真實分支 | 盡量貼近正式 API schema |
| 所有測試都用 mock | 失去對真整合行為的信心 | 保留部分 smoke / critical flow 打真 API |
| 只驗證文字訊息 | 可能漏掉空狀態按鈕、retry UI | 補驗互動元件與整體頁面狀態 |
| 測試間共用可變 mock 物件 | 容易互相污染 | 每個測試建立新的資料副本 |

---

&nbsp;

## 🍀 0604 - modify response

`modify response` 的價值在於：**真實 request 照常送出，但回應進來後由測試再做局部改寫**。這在你想保留後端大部分真實行為、只想把某些欄位調成特殊狀態時非常實用。

### modify response 的核心觀念

| 特性 | 說明 |
|------|------|
| 保留真後端流程 | request 仍真的送出，身份驗證、header、server logic 都照跑 |
| 只改局部資料 | 適合把某個欄位改成缺貨、VIP、feature flag 開啟 |
| 比純 mock 更貼近真實 | 其餘欄位仍由後端提供，降低假資料偏差 |
| 需注意資料一致性 | 改動後不能讓前端進入不合理狀態 |

### modify response 常用 API

| API | 用途 | 補充 |
|-----|------|------|
| `route.fetch()` | 先取得原始 response | 適合保留後端原始內容 |
| `response.json()` | 讀取原始 JSON | 修改前先解析 |
| `route.fulfill({ response, json })` | 以原始 response 為基底覆蓋內容 | 可保留 header / status |
| `route.fulfill({ body })` | 手動回傳字串 body | 非 JSON 時常用 |

### modify response 實戰範例：在不改後端的情況下注入促銷資料

```typescript
import { test, expect } from '@playwright/test'

test('VIP 使用者會看到額外折扣標籤', async ({ page }) => {
  await page.route('**/api/products/sku-1001', async (route) => {
    const originalResponse = await route.fetch()
    const product = await originalResponse.json()

    await route.fulfill({
      response: originalResponse,
      json: {
        ...product,
        badges: [...product.badges, 'VIP 95 折'],
        pricing: {
          ...product.pricing,
          vipDiscountPercent: 5,
        },
      },
    })
  })

  await page.goto('https://example.com/products/sku-1001')

  await expect(page.getByText('VIP 95 折')).toBeVisible()
  await expect(page.getByText('會員價')).toBeVisible()
})
```

### modify response 最佳實踐

- 優先改少量欄位，讓大部分資料維持真實來源。
- 若原始 response 很大，先只抽出你需要改的區塊，不要整包重寫。
- 修改後一定要驗 UI 最終狀態，而不是只相信 `route.fulfill()` 已經成功。
- 對價格、庫存、權限、feature flag 這類條件式 UI，`modify response` 特別實用。
- 若 response 不是 JSON，需另外考慮 `body`、`headers`、encoding 等一致性。

### modify response 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 直接改成一份完全不同的 schema | 會讓測試脫離真後端契約 | 保留原 schema，只改局部欄位 |
| 只改 `body` 不管 `content-type` | 前端可能解析失敗 | 確保回應型別與內容一致 |
| 改了價格沒改折扣標記 | UI 可能顯示衝突狀態 | 一併維護相關欄位一致性 |
| 把 modify response 當 mock API 用 | 測試意圖不清楚 | 若不需要真後端，直接用 mock API |

---

&nbsp;

## 🍀 0605 - fake backend

`fake backend` 比單次 `mock API` 更進一步：它不是只回一筆假回應，而是**在測試期間暫時扮演一個有狀態的後端**。這很適合前端開發中、後端尚未完成，或你想在本地建立完整 CRUD 流程但又不想連真 DB 時使用。

### fake backend 的核心觀念

| 面向 | mock API | fake backend |
|------|----------|--------------|
| 狀態 | 多半是固定回應 | 可隨測試操作改變 |
| 請求類型 | 常是單一 endpoint | 可覆蓋 list / detail / create / update / delete |
| 適用場景 | UI 狀態驗證 | 小型流程驗證、前端先行開發 |
| 維護成本 | 低 | 較高 |

### fake backend 常用 API

| API / 手法 | 用途 | 說明 |
|------------|------|------|
| `page.route()` | 攔截整組 API | 假後端的入口 |
| 記憶體陣列 / Map | 保存假資料狀態 | 例如購物車、待辦清單 |
| `request.postDataJSON()` | 讀取 request body | 建立或更新資料時使用 |
| `route.fulfill()` | 回應更新後資料 | 模擬 REST API |

### fake backend 實戰範例：購物車 CRUD 流程

```typescript
import { test, expect } from '@playwright/test'

test('使用 fake backend 驗證購物車新增與刪除流程', async ({ page }) => {
  const cart = [
    { id: 'item-1', name: '無線滑鼠', qty: 1, price: 990 },
  ]

  await page.route('**/api/cart', async (route) => {
    const request = route.request()

    if (request.method() === 'GET') {
      await route.fulfill({
        status: 200,
        contentType: 'application/json',
        json: {
          items: cart,
          total: cart.reduce((sum, item) => sum + item.qty * item.price, 0),
        },
      })
      return
    }

    if (request.method() === 'POST') {
      const body = request.postDataJSON() as { id: string; name: string; qty: number; price: number }
      cart.push(body)

      await route.fulfill({
        status: 201,
        contentType: 'application/json',
        json: { success: true, items: cart },
      })
      return
    }

    await route.fulfill({ status: 405, json: { message: 'Method Not Allowed' } })
  })

  await page.route('**/api/cart/*', async (route) => {
    const request = route.request()
    const itemId = request.url().split('/').pop()

    if (request.method() === 'DELETE' && itemId) {
      const targetIndex = cart.findIndex((item) => item.id === itemId)

      if (targetIndex >= 0) {
        cart.splice(targetIndex, 1)
      }

      await route.fulfill({
        status: 200,
        contentType: 'application/json',
        json: { success: true, items: cart },
      })
      return
    }

    await route.fulfill({ status: 404, json: { message: 'Not Found' } })
  })

  await page.goto('https://example.com/cart')

  await expect(page.getByText('無線滑鼠')).toBeVisible()

  await page.getByRole('button', { name: '加入藍牙耳機' }).click()
  await expect(page.getByText('藍牙耳機')).toBeVisible()

  await page.getByRole('button', { name: '刪除 無線滑鼠' }).click()
  await expect(page.getByText('無線滑鼠')).toHaveCount(0)
})
```

### fake backend 最佳實踐

- Fake backend 最好只覆蓋一小組有業務關聯的 API，不要無限擴大。
- 每個測試都重新初始化假資料，避免 state 洩漏到下一個案例。
- 讓回應格式貼近真後端，例如 `status code`、`message`、`pagination`。
- 若流程很大，考慮將 fake backend 抽為 helper，但要保留 reset 機制。
- Fake backend 適合前端開發與局部流程測試，不適合取代端到端整合測試。

### fake backend 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 多個測試共用同一份全域狀態 | 測試順序一變就出問題 | 每個測試重新建立資料 |
| 只處理 happy path | 錯誤狀態與邊界情境仍沒被驗證 | 補上 404 / 409 / 500 等分支 |
| 假後端回應太簡化 | 前端流程與正式環境差距大 | 保留接近真實契約的欄位 |
| 把所有後端都 fake 掉 | 失去真整合信心 | 只針對需要控制的區域使用 |

---

&nbsp;

## 🍀 0606 - network idle

`networkidle` 是一種載入狀態，而不是萬用等待解法。它的概念通常是「目前網路活動降到很低，頁面暫時安靜下來」，但在現代 SPA、輪詢、analytics、WebSocket、SSE 場景中，這個條件很容易過早或永遠達不到。

### network idle 的核心觀念

| 觀念 | 說明 |
|------|------|
| 它不是業務事件 | `networkidle` 只描述網路活動，不能保證 UI 已經渲染成你要的狀態 |
| 適合頁面初始載入後的粗粒度等待 | 尤其是 demo、爬蟲、簡單網站 |
| 不適合高互動 SPA 的主要同步策略 | 因為背景請求很多且時序複雜 |
| 最穩定的等待仍是 UI / API 精準條件 | 如 `expect(locator)`、`waitForResponse()`、`waitForURL()` |

### network idle 常用 API

| API | 用途 | 補充 |
|-----|------|------|
| `page.waitForLoadState('networkidle')` | 等頁面網路相對安靜 | 常用於初次進站後 |
| `page.waitForURL()` | 等待導航完成 | 比 networkidle 更聚焦 |
| `page.waitForResponse()` | 等待特定 API 完成 | 適合 SPA 動態更新 |
| `expect(locator).toBeVisible()` | 等 UI ready | 最貼近使用者結果 |

### network idle 實戰範例：搜尋結果頁面穩定等待

```typescript
import { test, expect } from '@playwright/test'

test('搜尋後顯示正確結果，不依賴 networkidle 當唯一條件', async ({ page }) => {
  await page.goto('https://example.com/search')

  await page.getByPlaceholder('搜尋商品').fill('機械鍵盤')

  const [searchResponse] = await Promise.all([
    page.waitForResponse((response) => {
      return (
        response.url().includes('/api/search') &&
        response.request().method() === 'GET' &&
        response.status() === 200
      )
    }),
    page.getByRole('button', { name: '搜尋' }).click(),
  ])

  await searchResponse.json()

  await expect(page.getByRole('heading', { name: /機械鍵盤/ })).toBeVisible()
  await expect(page.getByRole('list', { name: '搜尋結果' })).toContainText('80% 鍵盤')
})
```

### network idle 最佳實踐

- 把 `networkidle` 視為輔助訊號，不要當成唯一成功條件。
- 高互動 SPA 優先等待特定 API、URL、UI 元素，而不是整頁安靜。
- 背景輪詢、埋點、聊天、串流功能存在時，要特別小心 `networkidle`。
- 如果只是要等某個 skeleton 消失，用 UI assertion 會更準。
- 在爬蟲或 RPA 腳本中可用，但在 E2E 測試中應節制使用。

### network idle 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 每一步都 `waitForLoadState('networkidle')` | 測試變慢，且不一定真的穩 | 只在必要處使用精準等待 |
| 用它保證資料渲染完成 | 背景請求安靜不代表畫面完成 | 接 UI assertion 或 API 等待 |
| 遇到 flaky 就加 networkidle | 常只是掩蓋 race condition | 找出真正該等待的事件 |
| 對 WebSocket / polling 頁面使用 | 可能永遠不 idle | 改等關鍵 UI 狀態 |

---

&nbsp;

## 🍀 0607 - GraphQL testing

GraphQL 測試的難點在於：很多操作都打到同一支 `/graphql`，真正區分請求的不是 URL，而是 `operationName` 與 query / variables。這表示你不能用 REST 的思維只靠 endpoint 來等待或 mock。

### GraphQL testing 的核心觀念

| 重點 | 說明 |
|------|------|
| 同一路徑承載多個操作 | `GetProducts`、`GetCart`、`UpdateCart` 可能都打 `/graphql` |
| 應優先辨識 `operationName` | 這是最實用的分流依據 |
| 要同時驗證資料與 UI | GraphQL 常有 partial data + errors 的情境 |
| Mock 時要保留真回應形狀 | 通常是 `{ data, errors }` 結構 |

### GraphQL testing 常用 API

| API | 用途 | 補充 |
|-----|------|------|
| `request.postDataJSON()` | 讀取 GraphQL body | 可取出 `operationName`、`variables` |
| `page.route('**/graphql', handler)` | 攔截 GraphQL endpoint | 常搭配 operationName 判斷 |
| `page.waitForResponse()` | 等特定 GraphQL 回應 | 常透過 request body 進一步識別 |
| `route.fulfill({ json })` | 回傳 GraphQL mock | 建議維持 `{ data, errors }` 結構 |

### GraphQL testing 實戰範例：攔截產品查詢與 mutation

```typescript
import { test, expect } from '@playwright/test'

test('GraphQL 商品列表與加入購物車流程', async ({ page }) => {
  await page.route('**/graphql', async (route) => {
    const request = route.request()
    const body = request.postDataJSON() as {
      operationName?: string
      variables?: Record<string, unknown>
    }

    if (body.operationName === 'GetProductList') {
      await route.fulfill({
        status: 200,
        contentType: 'application/json',
        json: {
          data: {
            products: [
              { id: 'sku-1001', name: 'Alice 80% 機械鍵盤', stock: 12 },
            ],
          },
        },
      })
      return
    }

    if (body.operationName === 'AddToCart') {
      await route.fulfill({
        status: 200,
        contentType: 'application/json',
        json: {
          data: {
            addToCart: {
              success: true,
              cartCount: 1,
            },
          },
        },
      })
      return
    }

    await route.continue()
  })

  await page.goto('https://example.com/products')

  await expect(page.getByText('Alice 80% 機械鍵盤')).toBeVisible()

  await page.getByRole('button', { name: '加入購物車' }).click()

  await expect(page.getByRole('link', { name: /購物車/i })).toContainText('1')
})
```

### GraphQL testing 最佳實踐

- 優先以 `operationName` 作為等待與 mock 的識別鍵。
- 若產品有 GraphQL error handling，務必測 `{ data, errors }` 共存情境。
- Variables 是測試邏輯的一部分時，也要驗證，例如頁碼、篩選條件、商品 id。
- 若同一頁面會送多個 GraphQL query，避免只靠單純 URL 等待。
- Mock 結構要貼近實際 schema，否則前端快取或 fragment 行為容易失真。

### GraphQL testing 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 只檢查 `/graphql` URL | 無法分辨是哪個 operation | 讀 `operationName` 與 `variables` |
| GraphQL mock 只回最內層資料 | 少了 `data` 包裝，前端會解析失敗 | 保持 GraphQL 標準回應形狀 |
| 只測 query 不測 error | 很多 UI 真正複雜處在錯誤處理 | 補測 partial error / mutation failure |
| 忽略 variables | 可能 API 被錯參數呼叫也不自知 | 驗證 request payload |

---

&nbsp;

## 🍀 0608 - WebSocket

WebSocket 是長連線、事件驅動的即時通訊模型。對 Playwright 而言，測試重點通常不是「重建整個 socket server」，而是**觀察連線是否建立、訊息是否抵達，以及 UI 是否對即時事件做出正確反應**。

### WebSocket 的核心觀念

| 面向 | 說明 |
|------|------|
| 與 HTTP 不同 | 不是一來一回，而是持續連線、雙向傳輸 |
| 測試重點常在 UI 結果 | 例如聊天室新訊息、通知數量、價格即時更新 |
| 即時系統常伴隨重連與心跳 | 測試時要區分功能驗證與基礎設施驗證 |
| Playwright 適合做觀測與 UI 驗證 | 若要完整協議模擬，常需額外測試層次 |

### WebSocket 常用 API

| API / 事件 | 用途 | 補充 |
|------------|------|------|
| `page.on('websocket', handler)` | 監聽頁面建立 WebSocket | 可拿到 socket 物件 |
| `webSocket.url()` | 取得 WebSocket URL | 方便驗證連線目標 |
| `webSocket.on('framesent')` | 監聽送出的 frame | 可觀測 client 發送內容 |
| `webSocket.on('framereceived')` | 監聽收到的 frame | 適合驗證推播事件 |
| `webSocket.on('close')` | 監聽連線關閉 | 觀測重連 / 異常 |

### WebSocket 實戰範例：客服聊天室即時訊息

```typescript
import { test, expect } from '@playwright/test'

test('客服聊天室收到新訊息時更新 UI', async ({ page }) => {
  const receivedPayloads: string[] = []

  page.on('websocket', (webSocket) => {
    if (!webSocket.url().includes('/ws/chat')) {
      return
    }

    webSocket.on('framereceived', (event) => {
      receivedPayloads.push(String(event.payload))
    })
  })

  await page.goto('https://example.com/support/chat')

  await expect(page.getByRole('heading', { name: '線上客服' })).toBeVisible()

  await expect
    .poll(() => receivedPayloads.some((payload) => payload.includes('歡迎來到客服聊天室')))
    .toBeTruthy()

  await expect(page.getByRole('log')).toContainText('歡迎來到客服聊天室')
})
```

### WebSocket 最佳實踐

- 先驗證 UI 是否反映事件，再決定是否需要深入驗 frame 內容。
- 只觀測與案例有關的 socket，不要把所有 socket 都當成測試重點。
- 對即時通知、聊天室、監控面板等功能，可用 `expect.poll()` 搭配事件收集。
- 若產品有重連機制，應獨立測「斷線後 UI 與提示」而不是混在基本 happy path。
- WebSocket 測試要特別避免固定睡眠時間，改用事件或 UI 狀態同步。

### WebSocket 常見錯誤

| 錯誤寫法 | 問題 | 正確做法 |
|---------|------|----------|
| 用 `waitForTimeout()` 等新訊息 | 即時系統時序不固定，容易 flaky | 用事件收集或 `expect.poll()` |
| 想在 E2E 內完整模擬 socket server | 成本高且難維護 | 聚焦 UI 結果與必要觀測 |
| 把所有即時情境都放一個測試 | 失敗時難定位 | 拆成基本訊息、重連、錯誤提示等情境 |
| 只驗 frame，不驗 UI | 不符合使用者視角 | 補上畫面驗證 |

---

&nbsp;

## ⚡更多細節

### 🍀 0602.01 - route matching 與清理策略

#### 為什麼攔截規則常讓測試互相污染

攔截規則最常見的問題不是 API 寫錯，而是**匹配範圍太大**。例如 `**/api/**` 很容易一次攔到商品、會員、通知、追蹤事件，最後造成某個測試只想 mock 商品列表，卻連登入資訊都一起被改掉。

建議的排查順序：

1. 先確認是 `page.route()` 還是 `context.route()`。
2. 檢查 glob / regex 是否過寬。
3. 若只針對單一案例，完成後記得 `unroute()`。
4. 若多個 handler 都可能命中，先重新整理匹配優先順序。

```typescript
const inventoryHandler = async (route: Parameters<typeof page.route>[1]) => {
  await route.fulfill({
    status: 200,
    contentType: 'application/json',
    json: { stock: 0 },
  })
}

await page.route('**/api/inventory/sku-1001', inventoryHandler)
await page.goto('https://example.com/products/sku-1001')
await page.unroute('**/api/inventory/sku-1001', inventoryHandler)
```

&nbsp;

### 🍀 0604.01 - modify response 的一致性檢查

#### 哪些欄位改了最容易讓前端進入假成功狀態

`modify response` 最怕的不是語法錯，而是**資料之間彼此不一致**。例如你把 `isVip` 改成 `true`，卻沒同步 `discountRate`；或把 `stock` 改成 `0`，但 `canAddToCart` 還是 `true`。這會讓 UI 進入現實世界不會出現的狀態。

| 常見欄位 | 常需一起調整的相關欄位 |
|----------|------------------------|
| `stock` | `isSoldOut`、`purchaseLimit`、按鈕 disabled 狀態 |
| `role` / `permission` | 功能選單、feature flag、可見欄位 |
| `price` | `discount`, `currency`, `formattedPrice` |
| `membershipLevel` | badge、優惠說明、推薦區塊 |

若你想測單一條件式 UI，改動越小越好，並且一定要補一個 UI assertion 確認最終畫面沒有自相矛盾。

&nbsp;

### 🍀 0605.01 - stateful fake backend 的設計方式

#### 如何讓 fake backend 既逼真又可重置

最實用的 fake backend 不是功能最多，而是：

- 有清楚的初始資料
- 每個測試都能重新建立
- 請求與回應結構接近真 API
- 失敗時容易看出目前 state

一個好做法是把 state 工廠化：

```typescript
type CartItem = {
  id: string
  name: string
  qty: number
  price: number
}

function createCartState(): CartItem[] {
  return [
    { id: 'item-1', name: '無線滑鼠', qty: 1, price: 990 },
    { id: 'item-2', name: '鍵盤手托', qty: 1, price: 490 },
  ]
}

let cart = createCartState()
```

當測試需要多步驟 CRUD 時，這種 reset 機制比在全域共用可變陣列安全很多。

&nbsp;

### 🍀 0606.01 - 何時不該依賴 networkidle

#### 比 `networkidle` 更穩定的替代策略

以下情境通常不該優先使用 `networkidle`：

| 情境 | 原因 | 建議替代 |
|------|------|----------|
| SPA 搜尋 / 篩選 | 背景請求多，且畫面更新常比網路狀態更重要 | `waitForResponse()` + `expect(list)` |
| 輪詢儀表板 | 永遠持續打 API | 驗證特定卡片數字或 loading 消失 |
| 聊天 / 通知中心 | WebSocket / SSE 持續活躍 | 直接等待訊息 UI |
| 埋點很多的商業站 | analytics 讓網路不安靜 | 只等待與業務流程有關的 endpoint |

一個簡單判斷原則是：**如果使用者不會看「網路安不安靜」，那你大多也不該拿它當成功條件。**

&nbsp;

### 🍀 0607.01 - GraphQL 測試的 operationName 策略

#### 同一支 `/graphql` API 為什麼不能只看 URL

REST 常把語意放在 URL，GraphQL 則把語意放在 body。也因此這兩個請求雖然 URL 一樣，測試意義完全不同：

```typescript
const getProductList = {
  operationName: 'GetProductList',
  variables: { keyword: 'keyboard' },
}

const addToCart = {
  operationName: 'AddToCart',
  variables: { productId: 'sku-1001', qty: 1 },
}
```

如果你只寫：

```typescript
await page.waitForResponse('**/graphql')
```

那其實無法保證你等到的是哪個操作。對 GraphQL 而言，更好的策略通常是：

- 用 `operationName` 辨識操作種類
- 用 `variables` 驗證業務參數
- 用 `{ data, errors }` 驗證前端錯誤處理

&nbsp;

### 🍀 0608.01 - WebSocket 的測試邊界與穩定性

#### 哪些即時功能適合驗證 UI，哪些應降到整合測試

E2E 最適合驗證的是**使用者能感受到的結果**，不一定要在瀏覽器測到每一層協議細節。

| 類型 | 建議測試層級 | 原因 |
|------|--------------|------|
| 新訊息出現、未讀數增加、價格標示更新 | E2E | 直接反映使用者體驗 |
| reconnect backoff、心跳封包、協議重試細節 | 整合 / 契約測試 | 較偏基礎設施，不必都壓在 UI 測試 |
| server push 導致按鈕 disabled / enabled | E2E | 屬於可見 UI 狀態 |
| frame schema 驗證 | 單元 / 整合測試 | 在 UI 層做成本高且訊號差 |

因此，當 WebSocket 測試變得非常難寫時，先問自己：你要保證的是 socket 協議正確，還是使用者真的看到了正確結果？前者未必需要全部放在 Playwright E2E 內完成。

---

&nbsp;
