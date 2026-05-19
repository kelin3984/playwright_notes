# 💫 Chapter 16 — Browser Automation / RPA

> 超越測試框架，運用 Playwright Library 模式實現瀏覽器自動化、網頁爬取、排程操作與報表生成。

&nbsp;

## 目錄

- [💫 Chapter 16 — Browser Automation / RPA](#-chapter-16--browser-automation--rpa)
  - [目錄](#目錄)
  - [🍀 1601 - browser scripting](#-1601---browser-scripting)
    - [Library 模式 vs Test Runner 模式](#library-模式-vs-test-runner-模式)
    - [基本腳本結構](#基本腳本結構)
    - [安裝與執行](#安裝與執行)
    - [最佳實踐](#最佳實踐)
    - [常見錯誤](#常見錯誤)
  - [🍀 1602 - crawling](#-1602---crawling)
    - [核心觀念](#核心觀念)
    - [常用爬取 API](#常用爬取-api)
    - [實際範例：電商商品列表爬取](#實際範例電商商品列表爬取)
    - [最佳實踐](#最佳實踐-1)
    - [常見錯誤](#常見錯誤-1)
  - [🍀 1603 - auto operation](#-1603---auto-operation)
    - [核心觀念](#核心觀念-1)
    - [實際範例：批次表單填寫自動化](#實際範例批次表單填寫自動化)
    - [最佳實踐](#最佳實踐-2)
    - [常見錯誤](#常見錯誤-2)
  - [🍀 1604 - download automation](#-1604---download-automation)
    - [核心觀念](#核心觀念-2)
    - [Download API 說明](#download-api-說明)
    - [實際範例：後台報表批次下載](#實際範例後台報表批次下載)
    - [最佳實踐](#最佳實踐-3)
    - [常見錯誤](#常見錯誤-3)
  - [🍀 1605 - upload automation](#-1605---upload-automation)
    - [核心觀念](#核心觀念-3)
    - [Upload API 說明](#upload-api-說明)
    - [實際範例：商品圖片批次上傳](#實際範例商品圖片批次上傳)
    - [最佳實踐](#最佳實踐-4)
    - [常見錯誤](#常見錯誤-4)
  - [🍀 1606 - report automation](#-1606---report-automation)
    - [核心觀念](#核心觀念-4)
    - [實際範例：自動化截圖報告生成](#實際範例自動化截圖報告生成)
    - [最佳實踐](#最佳實踐-5)
    - [常見錯誤](#常見錯誤-5)
  - [🍀 1607 - PDF 生成](#-1607---pdf-生成)
    - [核心觀念](#核心觀念-5)
    - [page.pdf() 參數說明](#pagepdf-參數說明)
    - [實際範例：電商訂單 PDF 報表](#實際範例電商訂單-pdf-報表)
    - [最佳實踐](#最佳實踐-6)
    - [常見錯誤](#常見錯誤-6)
  - [⚡更多細節](#更多細節)
    - [🍀 1601.01 - 保存與復用登入狀態（`storageState`）](#-160101---保存與復用登入狀態storagestate)
      - [流程說明](#流程說明)
      - [完整範例](#完整範例)
      - [重點提示](#重點提示)
    - [🍀 1601.02 - 錯誤處理與重試機制](#-160102---錯誤處理與重試機制)
      - [通用重試工具](#通用重試工具)
      - [使用範例](#使用範例)
      - [退縮等待時間示意](#退縮等待時間示意)
    - [🍀 1601.03 - 搭配排程工具自動執行](#-160103---搭配排程工具自動執行)
      - [方案一：`node-cron`（本地排程）](#方案一node-cron本地排程)
      - [方案二：GitHub Actions `schedule`（雲端排程）](#方案二github-actions-schedule雲端排程)
      - [方案比較](#方案比較)
    - [🍀 1602.01 - 無限捲動（Infinite Scroll）爬取](#-160201---無限捲動infinite-scroll爬取)
      - [核心策略](#核心策略)
      - [完整範例](#完整範例-1)
    - [🍀 1602.02 - 登入後爬取受保護內容](#-160202---登入後爬取受保護內容)
    - [🍀 1602.03 - 反爬蟲對策](#-160203---反爬蟲對策)
      - [常見反爬蟲機制與對策](#常見反爬蟲機制與對策)
      - [使用 `playwright-extra` Stealth 插件](#使用-playwright-extra-stealth-插件)
    - [🍀 1603.01 - 帶重試機制的健壯自動化封裝](#-160301---帶重試機制的健壯自動化封裝)
    - [🍀 1603.02 - 多頁面並行操作（Concurrency 控制）](#-160302---多頁面並行操作concurrency-控制)
      - [並行策略比較](#並行策略比較)
    - [🍀 1604.01 - 攔截 Network Response 取得下載 URL](#-160401---攔截-network-response-取得下載-url)
    - [🍀 1605.01 - 記憶體內容直接上傳（不需要實體檔案）](#-160501---記憶體內容直接上傳不需要實體檔案)
      - [使用情境](#使用情境)
    - [🍀 1606.01 - 生成 HTML 視覺化報告](#-160601---生成-html-視覺化報告)
    - [🍀 1606.02 - 差異比對監控報告](#-160602---差異比對監控報告)
    - [🍀 1607.01 - 從 HTML 模板生成發票 PDF（完整範例）](#-160701---從-html-模板生成發票-pdf完整範例)
    - [🍀 1607.02 - CSS `@media print` 列印排版技巧](#-160702---css-media-print-列印排版技巧)
      - [常用 `@media print` 技巧](#常用-media-print-技巧)
      - [在 Playwright 中動態注入 Print CSS](#在-playwright-中動態注入-print-css)
      - [`page-break` 屬性速查](#page-break-屬性速查)

---

&nbsp;

## 🍀 1601 - browser scripting

### Library 模式 vs Test Runner 模式

Playwright 有兩種使用方式，本章重點是**不依賴 Test Runner** 的純腳本模式：

| 項目 | Test Runner 模式 | Library 模式（RPA） |
|------|-----------------|-------------------|
| 執行方式 | `npx playwright test` | `npx ts-node script.ts` 或 `node script.js` |
| 框架依賴 | `@playwright/test` | `playwright` |
| 進入點 | `test()` callback | 直接 `async` function |
| Assertion | `expect()` | 自訂邏輯 |
| 適合情境 | E2E 測試 | 自動化腳本、RPA、爬蟲 |
| 生命週期管理 | 自動 | 手動 `browser.close()` |

### 基本腳本結構

```typescript
import { chromium, Browser, BrowserContext, Page } from 'playwright'

async function main(): Promise<void> {
  const browser: Browser = await chromium.launch({
    headless: false, // 開發時建議 false，方便觀察
  })

  const context: BrowserContext = await browser.newContext({
    viewport: { width: 1280, height: 720 },
    locale: 'zh-TW',
  })

  const page: Page = await context.newPage()

  try {
    await page.goto('https://example.com')
    await page.waitForLoadState('networkidle')

    const title = await page.title()
    console.log(`頁面標題：${title}`)

    // 執行自動化操作...
    await page.getByRole('button', { name: '登入' }).click()
    await page.getByLabel('帳號').fill('admin@example.com')
    await page.getByLabel('密碼').fill('password123')
    await page.getByRole('button', { name: '送出' }).click()

    await page.waitForURL('**/dashboard')
    console.log('登入成功，已跳轉到 Dashboard')
  } catch (error) {
    console.error('執行失敗：', error)
    await page.screenshot({ path: 'error.png', fullPage: true })
  } finally {
    await context.close()
    await browser.close()
  }
}

main()
```

&nbsp;

### 安裝與執行

```bash
# 安裝 playwright（Library 版本，非 Test Runner）
npm install playwright

# 安裝瀏覽器 binary
npx playwright install chromium

# 安裝 TypeScript 執行環境
npm install -D typescript ts-node @types/node

# 執行腳本
npx ts-node scripts/automation.ts
```

&nbsp;

### 最佳實踐

- **永遠使用 `try / finally`**：確保 `browser.close()` 在任何情況下都被呼叫，避免殘留瀏覽器程序
- **headless 設為 `false` 開發，`true` 部署**：開發時視覺化確認行為，部署時節省資源
- **使用 `context.storageState()` 保存登入狀態**：避免每次腳本執行都重新登入
- **加上適當的 `waitForLoadState`**：避免在頁面未完全載入時操作 DOM
- **建立 logger 記錄執行過程**：自動化腳本缺乏 test report，需自行建立日誌

&nbsp;

### 常見錯誤

| ❌ 錯誤做法 | ✅ 正確做法 |
|------------|------------|
| 忘記呼叫 `browser.close()`，導致殘留程序 | 使用 `try/finally` 確保關閉 |
| 使用 `@playwright/test` 的 `test()` 在 Library 腳本中 | 直接使用 `playwright` 套件，不依賴 test runner |
| 用 `setTimeout` 等待元素 | 使用 `page.waitForSelector()` 或 `locator.waitFor()` |
| 腳本執行失敗時無任何記錄 | `catch` 中截圖並記錄錯誤訊息 |

---

&nbsp;

## 🍀 1602 - crawling

### 核心觀念

Playwright 可作為全功能爬蟲，相比 cheerio / puppeteer，優勢在於：

```txt
✅ 可執行 JavaScript（SPA、動態載入）
✅ 支援登入後爬取（需授權的內容）
✅ 自動等待元素出現（穩定性高）
✅ 支援分頁 / 無限捲動爬取
```

&nbsp;

### 常用爬取 API

| API | 說明 | 回傳型別 |
|-----|------|----------|
| `locator.allTextContents()` | 取得所有匹配元素的文字內容 | `Promise<string[]>` |
| `locator.all()` | 取得所有匹配 Locator 的陣列 | `Promise<Locator[]>` |
| `page.$$eval(selector, fn)` | 批次取得 DOM 屬性 | `Promise<T>` |
| `locator.getAttribute(name)` | 取得元素屬性值 | `Promise<string \| null>` |
| `locator.innerHTML()` | 取得 innerHTML | `Promise<string>` |
| `page.waitForSelector()` | 等待元素出現 | `Promise<ElementHandle>` |

&nbsp;

### 實際範例：電商商品列表爬取

```typescript
import { chromium } from 'playwright'
import * as fs from 'fs'

interface Product {
  name: string
  price: string
  url: string
  imageUrl: string
}

async function crawlProducts(baseUrl: string): Promise<Product[]> {
  const browser = await chromium.launch({ headless: true })
  const context = await browser.newContext({
    userAgent:
      'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36',
  })
  const page = await context.newPage()
  const allProducts: Product[] = []

  try {
    let currentPage = 1
    let hasNextPage = true

    while (hasNextPage) {
      await page.goto(`${baseUrl}?page=${currentPage}`)
      await page.waitForLoadState('networkidle')

      // 等待商品列表出現
      await page.waitForSelector('.product-card', { timeout: 10000 })

      // 取得當頁所有商品
      const products = await page.$$eval('.product-card', (cards) =>
        cards.map((card) => ({
          name: card.querySelector('.product-name')?.textContent?.trim() ?? '',
          price: card.querySelector('.product-price')?.textContent?.trim() ?? '',
          url: (card.querySelector('a') as HTMLAnchorElement)?.href ?? '',
          imageUrl: (card.querySelector('img') as HTMLImageElement)?.src ?? '',
        }))
      )

      allProducts.push(...products)
      console.log(`第 ${currentPage} 頁爬取完成，累計 ${allProducts.length} 筆`)

      // 判斷是否有下一頁
      const nextBtn = page.locator('.pagination-next:not(.disabled)')
      hasNextPage = (await nextBtn.count()) > 0

      if (hasNextPage) {
        currentPage++
        // 避免請求過快
        await page.waitForTimeout(1000)
      }
    }

    return allProducts
  } finally {
    await browser.close()
  }
}

async function main() {
  const products = await crawlProducts('https://shop.example.com/products')

  // 儲存為 JSON
  fs.writeFileSync('products.json', JSON.stringify(products, null, 2), 'utf-8')
  console.log(`爬取完成，共 ${products.length} 筆商品資料`)
}

main()
```

&nbsp;

### 最佳實踐

- **設定合理的請求間隔**：使用 `page.waitForTimeout(500~2000)` 避免對目標網站造成過大壓力
- **設定 `userAgent`**：部分网站會封鎖預設 headless userAgent
- **搭配重試機制**：網路不穩定時，單頁爬取失敗不應中斷整個任務
- **即時儲存進度**：爬取時分批寫入檔案，避免程式崩潰後資料全部遺失
- **使用 `page.$$eval` 批次取得資料**：比逐一呼叫 `locator.textContent()` 效能更好

&nbsp;

### 常見錯誤

| ❌ 錯誤做法 | ✅ 正確做法 |
|------------|------------|
| 直接爬取動態 SPA 的原始 HTML | 使用 Playwright 等待 JS 渲染後再擷取 |
| 用固定頁數迴圈，不判斷是否有下一頁 | 動態判斷分頁按鈕狀態 |
| 爬取過於頻繁，IP 被封鎖 | 加入請求間隔與 User-Agent 偽裝 |
| `$$eval` callback 中使用外部變數 | `$$eval` 在 browser context 執行，外部變數無法直接存取，需透過參數傳入 |

---

&nbsp;

## 🍀 1603 - auto operation

### 核心觀念

自動化操作（RPA）是指用程式取代人工在瀏覽器中反覆執行的操作，常見情境：

| 情境 | 說明 |
|------|------|
| 批次表單填寫 | 從 CSV / JSON 讀取資料，逐筆填入系統 |
| 定期資料同步 | 定時登入後台，下載報表並上傳至其他系統 |
| 系統遷移 | 從舊系統逐筆讀取資料，重新建立到新系統 |
| 定期截圖監控 | 排程執行，若頁面異常則發送警示 |

&nbsp;

### 實際範例：批次表單填寫自動化

```typescript
import { chromium, Page } from 'playwright'
import * as fs from 'fs'

interface Employee {
  name: string
  email: string
  department: string
  role: string
}

async function login(page: Page): Promise<void> {
  await page.goto('https://hr.example.com/login')
  await page.getByLabel('帳號').fill(process.env.HR_ADMIN_EMAIL ?? '')
  await page.getByLabel('密碼').fill(process.env.HR_ADMIN_PASSWORD ?? '')
  await page.getByRole('button', { name: '登入' }).click()
  await page.waitForURL('**/dashboard')
}

async function addEmployee(page: Page, employee: Employee): Promise<void> {
  await page.goto('https://hr.example.com/employees/new')
  await page.waitForLoadState('networkidle')

  await page.getByLabel('姓名').fill(employee.name)
  await page.getByLabel('電子郵件').fill(employee.email)
  await page.getByLabel('部門').selectOption(employee.department)
  await page.getByLabel('職位').fill(employee.role)

  await page.getByRole('button', { name: '儲存' }).click()

  // 等待成功提示
  await page.waitForSelector('.toast-success', { timeout: 5000 })
  console.log(`✅ 已新增員工：${employee.name}`)
}

async function main(): Promise<void> {
  // 從 JSON 檔案讀取員工資料
  const rawData = fs.readFileSync('employees.json', 'utf-8')
  const employees: Employee[] = JSON.parse(rawData)

  const browser = await chromium.launch({ headless: true })
  const context = await browser.newContext()
  const page = await context.newPage()

  const successList: string[] = []
  const failList: Array<{ name: string; error: string }> = []

  try {
    await login(page)

    for (const employee of employees) {
      try {
        await addEmployee(page, employee)
        successList.push(employee.name)
      } catch (error) {
        const errMsg = error instanceof Error ? error.message : String(error)
        failList.push({ name: employee.name, error: errMsg })
        console.error(`❌ 新增失敗：${employee.name} — ${errMsg}`)
        // 截圖保存錯誤現場
        await page.screenshot({
          path: `errors/${employee.name.replace(/\s/g, '_')}.png`,
        })
      }

      // 操作間隔，避免後端觸發 rate limiting
      await page.waitForTimeout(800)
    }
  } finally {
    await browser.close()
  }

  console.log(`\n執行完畢：成功 ${successList.length} 筆，失敗 ${failList.length} 筆`)
  if (failList.length > 0) {
    fs.writeFileSync('failed.json', JSON.stringify(failList, null, 2))
  }
}

main()
```

&nbsp;

### 最佳實踐

- **使用環境變數儲存帳密**：絕對不要將帳號密碼硬編碼在腳本中
- **每筆操作加上 `try/catch`**：單筆失敗不中斷整個批次任務
- **操作失敗時截圖保存現場**：方便事後分析失敗原因
- **記錄失敗清單並輸出報告**：執行完畢後輸出 `failed.json` 供人工補件
- **加入操作間隔**：避免觸發後端 rate limiting 或反爬蟲機制

&nbsp;

### 常見錯誤

| ❌ 錯誤做法 | ✅ 正確做法 |
|------------|------------|
| 帳密寫死在程式碼中 | 使用 `process.env` 讀取環境變數 |
| 整個 `for` 迴圈只有一個 `try/catch` | 每筆資料獨立 `try/catch`，失敗不影響後續 |
| 沒有任何執行記錄 | 每筆操作結果都 `console.log` 並寫入日誌檔 |
| 遇到偶發性錯誤直接放棄 | 加入重試機制（最多重試 N 次再放棄） |

---

&nbsp;

## 🍀 1604 - download automation

### 核心觀念

Playwright 透過監聽 `download` 事件，可以攔截並儲存瀏覽器觸發的任意檔案下載：

```txt
使用者點擊下載按鈕
  → 瀏覽器觸發 download 事件
  → Playwright 攔截 Download 物件
  → 呼叫 download.saveAs() 儲存到本地指定路徑
```

&nbsp;

### Download API 說明

| API | 說明 |
|-----|------|
| `page.waitForEvent('download')` | 等待下載事件，回傳 `Download` 物件 |
| `download.suggestedFilename()` | 取得伺服器建議的檔名 |
| `download.saveAs(path)` | 將檔案另存到指定路徑 |
| `download.path()` | 取得暫存路徑（需先等待下載完成） |
| `download.failure()` | 若下載失敗，回傳錯誤訊息 |
| `download.url()` | 取得下載的原始 URL |

&nbsp;

### 實際範例：後台報表批次下載

```typescript
import { chromium, Page, Download } from 'playwright'
import * as fs from 'fs'
import * as path from 'path'

const DOWNLOAD_DIR = './downloads'

async function downloadReport(
  page: Page,
  reportName: string,
  month: string
): Promise<void> {
  // 導航到報表頁面
  await page.goto('https://admin.example.com/reports')
  await page.waitForLoadState('networkidle')

  // 選擇報表類型
  await page.getByLabel('報表類型').selectOption(reportName)

  // 選擇月份
  await page.getByLabel('月份').fill(month)

  // 同時監聽下載事件並點擊按鈕
  const downloadPromise = page.waitForEvent('download')
  await page.getByRole('button', { name: '匯出 Excel' }).click()
  const download: Download = await downloadPromise

  // 確認下載成功
  const failure = await download.failure()
  if (failure) {
    throw new Error(`下載失敗：${failure}`)
  }

  // 儲存到本地，使用原始建議檔名
  const filename = download.suggestedFilename()
  const savePath = path.join(DOWNLOAD_DIR, `${month}_${filename}`)
  await download.saveAs(savePath)

  console.log(`✅ 已下載：${savePath}`)
}

async function main(): Promise<void> {
  // 確保下載目錄存在
  fs.mkdirSync(DOWNLOAD_DIR, { recursive: true })

  const browser = await chromium.launch({ headless: true })
  const context = await browser.newContext({
    acceptDownloads: true, // 必須啟用，否則下載會被封鎖
  })
  const page = await context.newPage()

  try {
    // 登入
    await page.goto('https://admin.example.com/login')
    await page.getByLabel('帳號').fill(process.env.ADMIN_EMAIL ?? '')
    await page.getByLabel('密碼').fill(process.env.ADMIN_PASSWORD ?? '')
    await page.getByRole('button', { name: '登入' }).click()
    await page.waitForURL('**/dashboard')

    // 批次下載過去 6 個月的報表
    const reports = ['銷售報表', '庫存報表', '用戶報表']
    const months = ['2026-01', '2026-02', '2026-03', '2026-04', '2026-05']

    for (const report of reports) {
      for (const month of months) {
        await downloadReport(page, report, month)
        await page.waitForTimeout(500)
      }
    }
  } finally {
    await browser.close()
  }

  console.log('所有報表下載完成')
}

main()
```

&nbsp;

### 最佳實踐

- **`acceptDownloads: true` 必須在 `newContext` 中設定**：預設 `false`，下載會被封鎖
- **使用 `Promise.all` 同時啟動監聽與點擊**：避免點擊太快導致 `waitForEvent` 錯過事件
- **呼叫 `download.failure()` 確認下載成功**：不判斷的話即使下載失敗也不會拋出例外
- **使用 `suggestedFilename()` 保留原始檔名**：避免自行命名導致副檔名錯誤
- **多檔下載時加入間隔**：部分系統對並行下載有限制

&nbsp;

### 常見錯誤

| ❌ 錯誤做法 | ✅ 正確做法 |
|------------|------------|
| 先點擊按鈕，再 `waitForEvent('download')` | 必須先設好監聽再點擊，否則可能錯過事件 |
| 忘記設 `acceptDownloads: true` | 在 `browser.newContext()` 中明確設定 |
| 下載後直接讀取 `download.path()` | 先呼叫 `download.saveAs()` 儲存，再從儲存路徑讀取 |
| 未檢查 `download.failure()` | 下載失敗時靜默錯誤，需主動檢查 |

---

&nbsp;

## 🍀 1605 - upload automation

### 核心觀念

Playwright 上傳檔案有兩種方式：

| 方式 | 適用情境 |
|------|----------|
| `locator.setInputFiles()` | 直接操作 `<input type="file">` 元素（最常見） |
| `page.waitForEvent('filechooser')` | 點擊按鈕後觸發系統檔案選擇器的情境 |

&nbsp;

### Upload API 說明

| API | 說明 |
|-----|------|
| `locator.setInputFiles(files)` | 直接設定檔案到 `<input type="file">` |
| `page.waitForEvent('filechooser')` | 等待並攔截檔案選擇器事件 |
| `fileChooser.setFiles(files)` | 透過攔截的選擇器設定檔案 |
| `fileChooser.isMultiple()` | 判斷是否支援多檔選取 |

`setInputFiles` 的 `files` 參數可以是：
- 單一路徑字串：`'./photo.jpg'`
- 路徑陣列（多檔）：`['./a.jpg', './b.jpg']`
- 物件（模擬記憶體中的檔案）：`{ name: 'file.txt', mimeType: 'text/plain', buffer: Buffer.from('hello') }`

&nbsp;

### 實際範例：商品圖片批次上傳

```typescript
import { chromium, Page } from 'playwright'
import * as fs from 'fs'
import * as path from 'path'

interface ProductImage {
  productId: string
  imagePaths: string[]
}

async function uploadProductImages(
  page: Page,
  productId: string,
  imagePaths: string[]
): Promise<void> {
  await page.goto(`https://admin.example.com/products/${productId}/images`)
  await page.waitForLoadState('networkidle')

  // 方式一：直接操作 <input type="file"> 元素（支援多檔）
  await page.locator('input[type="file"]').setInputFiles(imagePaths)

  // 等待預覽圖出現（確認檔案已選取）
  await page.waitForSelector('.image-preview', { timeout: 5000 })

  // 點擊上傳按鈕
  await page.getByRole('button', { name: '上傳圖片' }).click()

  // 等待上傳完成（進度條消失）
  await page.waitForSelector('.upload-progress', { state: 'hidden', timeout: 30000 })

  console.log(`✅ 商品 ${productId} 已上傳 ${imagePaths.length} 張圖片`)
}

async function uploadViaFileChooser(page: Page): Promise<void> {
  // 方式二：透過攔截 file chooser 事件
  const fileChooserPromise = page.waitForEvent('filechooser')
  await page.getByRole('button', { name: '選擇檔案' }).click()
  const fileChooser = await fileChooserPromise

  console.log(`是否支援多選：${fileChooser.isMultiple()}`)

  await fileChooser.setFiles([
    './uploads/product-main.jpg',
    './uploads/product-side.jpg',
    './uploads/product-back.jpg',
  ])
}

async function main(): Promise<void> {
  const uploadList: ProductImage[] = JSON.parse(
    fs.readFileSync('upload-list.json', 'utf-8')
  )

  const browser = await chromium.launch({ headless: true })
  const context = await browser.newContext()
  const page = await context.newPage()

  try {
    // 登入
    await page.goto('https://admin.example.com/login')
    await page.getByLabel('帳號').fill(process.env.ADMIN_EMAIL ?? '')
    await page.getByLabel('密碼').fill(process.env.ADMIN_PASSWORD ?? '')
    await page.getByRole('button', { name: '登入' }).click()
    await page.waitForURL('**/dashboard')

    // 批次上傳
    for (const item of uploadList) {
      // 確認所有圖片檔案存在
      const existingFiles = item.imagePaths.filter((p) => fs.existsSync(p))
      if (existingFiles.length === 0) {
        console.warn(`⚠️ 商品 ${item.productId} 無可上傳的圖片，跳過`)
        continue
      }

      await uploadProductImages(page, item.productId, existingFiles)
      await page.waitForTimeout(1000)
    }
  } finally {
    await browser.close()
  }
}

main()
```

&nbsp;

### 最佳實踐

- **傳入絕對路徑**：`setInputFiles` 使用相對路徑時可能因 cwd 不同而失敗，建議使用 `path.resolve()`
- **先確認檔案存在**：上傳前用 `fs.existsSync()` 驗證，避免靜默失敗
- **等待上傳完成**：點擊上傳後需等待後端回應（進度條消失、成功訊息出現）
- **`filechooser` 監聽與觸發點擊需同步**：和 `download` 一樣，先設監聽再觸發動作

&nbsp;

### 常見錯誤

| ❌ 錯誤做法 | ✅ 正確做法 |
|------------|------------|
| 對隱藏的 `<input>` 直接 `.click()` | 直接呼叫 `.setInputFiles()`，不需要元素可見 |
| 上傳後立即繼續下一步 | 等待上傳完成的 UI 反饋（成功訊息、進度條消失） |
| 傳入相對路徑 | 使用 `path.resolve(__dirname, './image.jpg')` 確保路徑正確 |
| 多檔上傳時逐一呼叫 `setInputFiles` | 一次傳入路徑陣列 `setInputFiles([...paths])` |

---

&nbsp;

## 🍀 1606 - report automation

### 核心觀念

Report Automation 是指用腳本定期執行以下流程，取代人工操作：

```txt
登入系統
  → 導航到特定頁面
  → 截取關鍵畫面 / 擷取資料
  → 整合成報告（截圖、JSON、CSV、PDF）
  → 輸出或通知相關人員
```

常見應用：

| 情境 | 說明 |
|------|------|
| 每日監控報表 | 定時截取儀表板畫面，Email 通知管理者 |
| 資料匯出自動化 | 從後台系統定期抓取資料，存入資料庫或試算表 |
| 競品監控 | 定期截取競品報價頁面，比對差異 |
| SLA 合規截圖 | 定期截取系統可用性頁面，作為合規佐證 |

&nbsp;

### 實際範例：自動化截圖報告生成

```typescript
import { chromium, Page } from 'playwright'
import * as fs from 'fs'
import * as path from 'path'

interface ReportSection {
  name: string
  url: string
  selector?: string // 若指定，只截取特定區塊
}

interface ReportData {
  section: string
  value: string
  capturedAt: string
}

const reportSections: ReportSection[] = [
  {
    name: '銷售總覽',
    url: 'https://admin.example.com/dashboard/sales',
    selector: '.sales-overview-card',
  },
  {
    name: '用戶成長',
    url: 'https://admin.example.com/dashboard/users',
    selector: '.user-growth-chart',
  },
  {
    name: '庫存警示',
    url: 'https://admin.example.com/inventory/alerts',
  },
]

async function captureSection(
  page: Page,
  section: ReportSection,
  outputDir: string
): Promise<ReportData[]> {
  await page.goto(section.url)
  await page.waitForLoadState('networkidle')

  const screenshotPath = path.join(
    outputDir,
    `${section.name.replace(/\s/g, '_')}.png`
  )

  if (section.selector) {
    // 截取指定區塊
    await page.locator(section.selector).screenshot({ path: screenshotPath })
  } else {
    // 截取整頁
    await page.screenshot({ path: screenshotPath, fullPage: true })
  }

  console.log(`📸 已截圖：${screenshotPath}`)

  // 擷取數據（示例：取得統計數字）
  const dataRows = await page.$$eval('.metric-row', (rows) =>
    rows.map((row) => ({
      label: row.querySelector('.metric-label')?.textContent?.trim() ?? '',
      value: row.querySelector('.metric-value')?.textContent?.trim() ?? '',
    }))
  )

  return dataRows.map((row) => ({
    section: section.name,
    value: `${row.label}: ${row.value}`,
    capturedAt: new Date().toISOString(),
  }))
}

async function generateReport(): Promise<void> {
  const today = new Date().toISOString().slice(0, 10)
  const outputDir = path.join('./reports', today)
  fs.mkdirSync(outputDir, { recursive: true })

  const browser = await chromium.launch({ headless: true })
  const context = await browser.newContext({
    viewport: { width: 1440, height: 900 },
  })
  const page = await context.newPage()
  const allData: ReportData[] = []

  try {
    // 登入
    await page.goto('https://admin.example.com/login')
    await page.getByLabel('帳號').fill(process.env.ADMIN_EMAIL ?? '')
    await page.getByLabel('密碼').fill(process.env.ADMIN_PASSWORD ?? '')
    await page.getByRole('button', { name: '登入' }).click()
    await page.waitForURL('**/dashboard')

    // 逐頁截圖並擷取資料
    for (const section of reportSections) {
      const sectionData = await captureSection(page, section, outputDir)
      allData.push(...sectionData)
    }

    // 輸出 JSON 資料報告
    const jsonPath = path.join(outputDir, 'data.json')
    fs.writeFileSync(jsonPath, JSON.stringify(allData, null, 2), 'utf-8')

    // 輸出 CSV
    const csvHeader = 'section,value,capturedAt\n'
    const csvRows = allData
      .map((d) => `"${d.section}","${d.value}","${d.capturedAt}"`)
      .join('\n')
    fs.writeFileSync(path.join(outputDir, 'data.csv'), csvHeader + csvRows, 'utf-8')

    console.log(`\n✅ 報告生成完成，儲存於 ${outputDir}`)
  } finally {
    await browser.close()
  }
}

generateReport()
```

&nbsp;

### 最佳實踐

- **以日期命名輸出資料夾**：`./reports/2026-05-19/`，方便歸檔與比對歷史紀錄
- **截圖搭配資料 JSON 一起輸出**：截圖提供視覺佐證，JSON 方便程式後續處理
- **使用 `fullPage: true`** 確保長頁面完整截取
- **設定固定 `viewport`**：確保截圖尺寸一致，方便比對
- **結合排程工具**（`cron` / `GitHub Actions schedule`）實現全自動定期執行

&nbsp;

### 常見錯誤

| ❌ 錯誤做法 | ✅ 正確做法 |
|------------|------------|
| 截圖時動畫未結束 | 使用 `page.waitForLoadState('networkidle')` 並等待動畫完成 |
| 所有截圖存在同一目錄，檔名重複覆蓋 | 以日期或時間戳建立子目錄 |
| 只截圖、不輸出結構化資料 | 同時輸出截圖與 JSON/CSV，方便程式後續處理 |
| 腳本執行無任何記錄 | 每個步驟都輸出 log，加入執行時間戳 |

---

&nbsp;

## 🍀 1607 - PDF 生成

### 核心觀念

Playwright 使用 Chromium 的列印引擎，可將任何網頁渲染為高品質 PDF。這與「截圖轉 PDF」不同：

| 比較 | 截圖轉 PDF | `page.pdf()` |
|------|-----------|-------------|
| 輸出品質 | 點陣圖（解析度有限） | 向量 / 高品質印刷輸出 |
| 文字可複製 | ❌ | ✅ |
| 背景色 / 圖片 | 依截圖設定 | 需設 `printBackground: true` |
| 多頁自動分頁 | ❌ | ✅ |
| 適合列印 | 尚可 | ✅ |

> **限制**：`page.pdf()` 只在 Chromium 中支援，Firefox / WebKit 不支援。

&nbsp;

### page.pdf() 參數說明

| 參數 | 型別 | 說明 | 預設值 |
|------|------|------|--------|
| `path` | `string` | 輸出 PDF 路徑 | — |
| `format` | `string` | 紙張格式（`'A4'`、`'Letter'` 等） | `'Letter'` |
| `width` / `height` | `string` | 自訂頁面尺寸（優先於 `format`） | — |
| `printBackground` | `boolean` | 是否渲染背景色與圖片 | `false` |
| `margin` | `object` | 頁面邊距（`top`、`right`、`bottom`、`left`） | `{}` |
| `scale` | `number` | 縮放比例（`0.1` ~ `2`） | `1` |
| `landscape` | `boolean` | 橫向排版 | `false` |
| `displayHeaderFooter` | `boolean` | 顯示頁首頁尾 | `false` |
| `headerTemplate` | `string` | 頁首 HTML 模板 | `''` |
| `footerTemplate` | `string` | 頁尾 HTML 模板 | `''` |
| `pageRanges` | `string` | 指定頁範圍（如 `'1-5, 8, 11-13'`） | `''`（全部） |

&nbsp;

### 實際範例：電商訂單 PDF 報表

```typescript
import { chromium, Browser, Page } from 'playwright'
import * as path from 'path'
import * as fs from 'fs'

interface OrderReport {
  orderId: string
  customerName: string
  outputPath: string
}

async function generateOrderPdf(
  page: Page,
  orderId: string,
  outputPath: string
): Promise<void> {
  // 導航到訂單詳細頁面
  await page.goto(`https://admin.example.com/orders/${orderId}/print-view`)
  await page.waitForLoadState('networkidle')

  // 等待所有圖片載入完成
  await page.waitForFunction(() => {
    const images = document.querySelectorAll('img')
    return Array.from(images).every((img) => img.complete)
  })

  await page.pdf({
    path: outputPath,
    format: 'A4',
    printBackground: true,
    margin: {
      top: '20mm',
      right: '15mm',
      bottom: '20mm',
      left: '15mm',
    },
    displayHeaderFooter: true,
    headerTemplate: `
      <div style="font-size: 10px; width: 100%; text-align: right; padding-right: 20mm; color: #666;">
        訂單系統 — 機密文件
      </div>
    `,
    footerTemplate: `
      <div style="font-size: 10px; width: 100%; text-align: center; color: #666;">
        第 <span class="pageNumber"></span> 頁，共 <span class="totalPages"></span> 頁
        &nbsp;&nbsp;|&nbsp;&nbsp;
        列印時間：<span class="date"></span>
      </div>
    `,
  })

  console.log(`✅ PDF 已生成：${outputPath}`)
}

async function batchGenerateOrderPdfs(orders: OrderReport[]): Promise<void> {
  const outputDir = './order-pdfs'
  fs.mkdirSync(outputDir, { recursive: true })

  // PDF 生成需要 Chromium，明確指定
  const browser: Browser = await chromium.launch({ headless: true })
  const context = await browser.newContext()
  const page: Page = await context.newPage()

  try {
    // 登入
    await page.goto('https://admin.example.com/login')
    await page.getByLabel('帳號').fill(process.env.ADMIN_EMAIL ?? '')
    await page.getByLabel('密碼').fill(process.env.ADMIN_PASSWORD ?? '')
    await page.getByRole('button', { name: '登入' }).click()
    await page.waitForURL('**/dashboard')

    for (const order of orders) {
      const pdfPath = path.join(outputDir, `order_${order.orderId}.pdf`)
      await generateOrderPdf(page, order.orderId, pdfPath)
      await page.waitForTimeout(300) // 短暫間隔
    }
  } finally {
    await browser.close()
  }
}

// 也可以直接把 HTML 內容渲染成 PDF（不需要後端）
async function generatePdfFromHtml(
  htmlContent: string,
  outputPath: string
): Promise<void> {
  const browser = await chromium.launch({ headless: true })
  const page = await browser.newPage()

  try {
    // 直接設定 HTML 內容，不需要實際的 URL
    await page.setContent(htmlContent, { waitUntil: 'networkidle' })

    await page.pdf({
      path: outputPath,
      format: 'A4',
      printBackground: true,
      margin: { top: '15mm', right: '15mm', bottom: '15mm', left: '15mm' },
    })

    console.log(`✅ PDF 生成完成：${outputPath}`)
  } finally {
    await browser.close()
  }
}

// 使用範例：從 HTML 模板生成 PDF
const invoiceHtml = `
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <style>
    body { font-family: 'Noto Sans TC', sans-serif; margin: 0; }
    .header { background: #1a1a2e; color: white; padding: 20px; }
    .title { font-size: 24px; font-weight: bold; }
    .invoice-table { width: 100%; border-collapse: collapse; margin-top: 20px; }
    .invoice-table th, .invoice-table td { border: 1px solid #ddd; padding: 8px; text-align: left; }
    .invoice-table th { background: #f5f5f5; }
    .total-row { font-weight: bold; background: #fff3cd; }
  </style>
</head>
<body>
  <div class="header">
    <div class="title">發票 #INV-2026-001</div>
    <div>開立日期：2026-05-19</div>
  </div>
  <div style="padding: 20px;">
    <table class="invoice-table">
      <thead>
        <tr><th>品項</th><th>數量</th><th>單價</th><th>小計</th></tr>
      </thead>
      <tbody>
        <tr><td>Playwright 教學課程</td><td>1</td><td>NT$2,000</td><td>NT$2,000</td></tr>
        <tr><td>技術顧問服務（小時）</td><td>3</td><td>NT$1,500</td><td>NT$4,500</td></tr>
        <tr class="total-row"><td colspan="3">合計</td><td>NT$6,500</td></tr>
      </tbody>
    </table>
  </div>
</body>
</html>
`

generatePdfFromHtml(invoiceHtml, './invoice-INV-2026-001.pdf')
```

&nbsp;

### 最佳實踐

- **`printBackground: true` 幾乎每次都需要開啟**：否則背景色、色塊全部消失，版面會走樣
- **等待圖片載入完成再呼叫 `pdf()`**：圖片未載入會導致 PDF 中出現破圖
- **使用 `page.setContent()` 搭配 HTML 模板**：不依賴後端，直接用 HTML 渲染 PDF，最靈活
- **`margin` 建議明確設定**：預設邊距在不同系統可能不一致
- **處理中文字型**：確保頁面有載入中文字型（Google Fonts 或本地字型），否則中文可能顯示為方塊

&nbsp;

### 常見錯誤

| ❌ 錯誤做法 | ✅ 正確做法 |
|------------|------------|
| 未設 `printBackground: true`，背景與色塊消失 | 幾乎所有視覺設計的頁面都需要開啟此選項 |
| 在 Firefox / WebKit 呼叫 `page.pdf()` | `pdf()` 只支援 Chromium，必須使用 `chromium.launch()` |
| PDF 生成後中文顯示為方塊 | 在 HTML 中明確引入支援中文的字型 |
| 頁面含有動畫，PDF 截取到中間狀態 | 呼叫 `pdf()` 前先 `waitForLoadState('networkidle')` 並暫停動畫 |
| 使用截圖代替 PDF | 截圖生成的是點陣圖，文字無法複製；真正需要列印品質請用 `page.pdf()` |

---

&nbsp;

## ⚡更多細節

### 🍀 1601.01 - 保存與復用登入狀態（`storageState`）

每次腳本執行都重新登入，不僅耗時，也容易觸發帳號鎖定。Playwright 的 `storageState` 能將 cookies 與 localStorage 序列化存檔，下次直接復用。

#### 流程說明

```txt
首次執行：登入 → context.storageState() 序列化 → 寫入 auth.json
後續執行：讀取 auth.json → newContext({ storageState }) → 跳過登入
偵測失效：嘗試訪問受保護頁面 → 若被導向 /login 則重新登入並更新 auth.json
```

#### 完整範例

```typescript
import { chromium, BrowserContext } from 'playwright'
import * as fs from 'fs'

const AUTH_FILE = './auth.json'

async function saveAuthState(): Promise<void> {
  const browser = await chromium.launch({ headless: true })
  const context: BrowserContext = await browser.newContext()
  const page = await context.newPage()

  await page.goto('https://admin.example.com/login')
  await page.getByLabel('帳號').fill(process.env.ADMIN_EMAIL ?? '')
  await page.getByLabel('密碼').fill(process.env.ADMIN_PASSWORD ?? '')
  await page.getByRole('button', { name: '登入' }).click()
  await page.waitForURL('**/dashboard')

  // 序列化目前的 cookies + localStorage
  await context.storageState({ path: AUTH_FILE })
  console.log(`✅ 已儲存登入狀態到 ${AUTH_FILE}`)

  await browser.close()
}

async function runWithSavedAuth(): Promise<void> {
  // 若 auth.json 不存在，先執行登入流程
  if (!fs.existsSync(AUTH_FILE)) {
    await saveAuthState()
  }

  const browser = await chromium.launch({ headless: true })
  const context: BrowserContext = await browser.newContext({
    storageState: AUTH_FILE, // 直接注入已保存的登入狀態
  })
  const page = await context.newPage()

  try {
    await page.goto('https://admin.example.com/dashboard')

    // 偵測 Session 是否過期（被導向登入頁）
    if (page.url().includes('/login')) {
      console.warn('⚠️ Session 已過期，重新登入...')
      await browser.close()
      await saveAuthState()          // 重新登入並更新 auth.json
      return runWithSavedAuth()       // 遞迴重試
    }

    console.log('已使用保存的 Session，跳過登入流程')
    // ... 執行主要自動化任務
  } finally {
    await browser.close()
  }
}

runWithSavedAuth()
```

#### 重點提示

| 項目 | 說明 |
|------|------|
| `storageState` 包含的內容 | cookies、localStorage、sessionStorage |
| 安全注意事項 | `auth.json` 含敏感資訊，必須加入 `.gitignore` |
| Session 有效期 | 依網站設定而異，建議腳本偵測是否被導向登入頁 |
| 多帳號情境 | 為每個帳號建立獨立的 `auth-{role}.json` |

---

&nbsp;

### 🍀 1601.02 - 錯誤處理與重試機制

網路波動、目標網站短暫異常都可能導致單次操作失敗。透過通用的重試工具函式，可讓腳本更具彈性。

#### 通用重試工具

```typescript
interface RetryOptions {
  maxAttempts?: number    // 最大嘗試次數（含第一次），預設 3
  delayMs?: number        // 每次重試前的等待毫秒，預設 1000
  backoff?: boolean       // 是否啟用指數退縮，預設 true
}

async function withRetry<T>(
  fn: () => Promise<T>,
  options: RetryOptions = {}
): Promise<T> {
  const { maxAttempts = 3, delayMs = 1000, backoff = true } = options
  let lastError: unknown

  for (let attempt = 1; attempt <= maxAttempts; attempt++) {
    try {
      return await fn()
    } catch (error) {
      lastError = error
      if (attempt === maxAttempts) break

      const wait = backoff ? delayMs * 2 ** (attempt - 1) : delayMs
      console.warn(`⚠️ 第 ${attempt} 次失敗，${wait}ms 後重試...`, error)
      await new Promise((resolve) => setTimeout(resolve, wait))
    }
  }

  throw lastError
}
```

#### 使用範例

```typescript
// 將任意操作包進 withRetry
const result = await withRetry(
  async () => {
    await page.goto('https://admin.example.com/orders')
    await page.waitForSelector('.order-list', { timeout: 8000 })
    return await page.locator('.order-row').count()
  },
  { maxAttempts: 3, delayMs: 1500, backoff: true }
)

console.log(`訂單筆數：${result}`)
```

#### 退縮等待時間示意

| 嘗試次數 | `delayMs=1000` + `backoff=true` | `backoff=false` |
|----------|--------------------------------|-----------------|
| 第 1 次失敗後 | 1000 ms | 1000 ms |
| 第 2 次失敗後 | 2000 ms | 1000 ms |
| 第 3 次失敗後 | 4000 ms（若仍有重試） | 1000 ms |

---

&nbsp;

### 🍀 1601.03 - 搭配排程工具自動執行

自動化腳本通常需要定期執行。常見方案有本地端的 `node-cron`，以及 CI/CD 的 GitHub Actions `schedule`。

#### 方案一：`node-cron`（本地排程）

```typescript
import cron from 'node-cron'
import { runDailyReport } from './report'

// 每天早上 8:00 執行
cron.schedule('0 8 * * *', async () => {
  console.log(`[${new Date().toISOString()}] 開始執行每日報表...`)
  try {
    await runDailyReport()
    console.log('✅ 報表執行完成')
  } catch (error) {
    console.error('❌ 報表執行失敗：', error)
  }
}, {
  timezone: 'Asia/Taipei',
})

console.log('排程已啟動，等待執行...')
```

安裝：

```bash
npm install node-cron
npm install -D @types/node-cron
```

#### 方案二：GitHub Actions `schedule`（雲端排程）

```yaml
# .github/workflows/daily-report.yml
name: Daily Report

on:
  schedule:
    - cron: '0 0 * * *'   # UTC 00:00 = 台北 08:00
  workflow_dispatch:        # 允許手動觸發

jobs:
  run-report:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npx playwright install --with-deps chromium
      - name: Run automation script
        run: npx ts-node scripts/daily-report.ts
        env:
          ADMIN_EMAIL: ${{ secrets.ADMIN_EMAIL }}
          ADMIN_PASSWORD: ${{ secrets.ADMIN_PASSWORD }}
```

#### 方案比較

| 面向 | `node-cron` | GitHub Actions |
|------|-------------|----------------|
| 執行環境 | 本地機器需保持開機 | 雲端，無需本地機器 |
| 設定複雜度 | 簡單 | 需撰寫 YAML |
| 機密管理 | `.env` 檔 | Secrets + 環境變數 |
| 適合情境 | 開發 / 內部自動化 | CI/CD、正式部署 |

---

&nbsp;

### 🍀 1602.01 - 無限捲動（Infinite Scroll）爬取

部分網站不使用分頁，而是捲動到底部時動態載入更多內容。需要循環捲動並偵測「無新資料」的終止條件。

#### 核心策略

```txt
記錄目前項目數
  → 捲動到頁面最底部
  → 等待網路請求完成
  → 再次計算項目數
  → 若項目數未增加，計數器 +1
  → 若計數器超過 MAX_NO_NEW 次，視為載入完畢
```

#### 完整範例

```typescript
import { chromium } from 'playwright'

interface Article {
  title: string
  url: string
  publishedAt: string
}

async function crawlInfiniteScroll(url: string): Promise<Article[]> {
  const browser = await chromium.launch({ headless: true })
  const page = await browser.newPage()

  try {
    await page.goto(url)
    await page.waitForLoadState('networkidle')

    const MAX_NO_NEW = 3  // 連續 N 次無新內容後停止
    let noNewCount = 0
    let prevCount = 0

    while (noNewCount < MAX_NO_NEW) {
      // 捲動到頁面底部
      await page.evaluate(() => window.scrollTo(0, document.body.scrollHeight))
      // 等待動態載入
      await page.waitForLoadState('networkidle')
      await page.waitForTimeout(800)

      const currentCount = await page.locator('.article-card').count()

      if (currentCount > prevCount) {
        console.log(`已載入 ${currentCount} 筆，繼續捲動...`)
        prevCount = currentCount
        noNewCount = 0
      } else {
        noNewCount++
        console.log(`無新資料（${noNewCount}/${MAX_NO_NEW}）`)
      }
    }

    // 資料全部載入後，一次性擷取
    const articles = await page.$$eval('.article-card', (cards) =>
      cards.map((card) => ({
        title: card.querySelector('.title')?.textContent?.trim() ?? '',
        url: (card.querySelector('a') as HTMLAnchorElement)?.href ?? '',
        publishedAt: card.querySelector('.date')?.textContent?.trim() ?? '',
      }))
    )

    console.log(`✅ 共擷取 ${articles.length} 篇文章`)
    return articles
  } finally {
    await browser.close()
  }
}
```

---

&nbsp;

### 🍀 1602.02 - 登入後爬取受保護內容

需要授權的內容（會員專區、後台資料）必須先登入再爬取。結合 `storageState` 可避免每次重新登入。

```typescript
import { chromium } from 'playwright'
import * as fs from 'fs'

const AUTH_FILE = './auth.json'

async function crawlProtectedContent(targetUrl: string) {
  if (!fs.existsSync(AUTH_FILE)) {
    throw new Error('尚未登入，請先執行 saveAuthState()')
  }

  const browser = await chromium.launch({ headless: true })
  const context = await browser.newContext({ storageState: AUTH_FILE })
  const page = await context.newPage()

  try {
    await page.goto(targetUrl)

    // 偵測是否被重導向至登入頁（Session 過期）
    if (page.url().includes('/login')) {
      throw new Error('Session 已過期，請重新執行登入流程')
    }

    await page.waitForLoadState('networkidle')

    // 爬取受保護資料
    const rows = await page.$$eval('table tbody tr', (trs) =>
      trs.map((tr) =>
        Array.from(tr.querySelectorAll('td')).map(
          (td) => td.textContent?.trim() ?? ''
        )
      )
    )

    return rows
  } finally {
    await browser.close()
  }
}
```

---

&nbsp;

### 🍀 1602.03 - 反爬蟲對策

許多網站會偵測並封鎖自動化瀏覽器。以下是常見的對抗策略：

#### 常見反爬蟲機制與對策

| 反爬蟲機制 | 對策 |
|----------|------|
| 偵測 `navigator.webdriver` 標記 | 使用 `playwright-extra` + `puppeteer-extra-plugin-stealth` |
| 封鎖 headless User-Agent | 設定真實瀏覽器 UA 字串 |
| 請求頻率過高 | 加入隨機等待時間 `500~2000ms` |
| 滑鼠軌跡分析 | 使用 `page.mouse.move()` 模擬真實移動 |
| IP 封鎖 | 搭配 Proxy 輪替（`proxy` in `newContext`） |
| CAPTCHA | 使用第三方解碼服務（如 2captcha），或降低爬取頻率 |

#### 使用 `playwright-extra` Stealth 插件

```typescript
import { chromium } from 'playwright-extra'
import StealthPlugin from 'puppeteer-extra-plugin-stealth'

// 套用 stealth 插件（抹除自動化特徵）
chromium.use(StealthPlugin())

async function crawlWithStealth() {
  const browser = await chromium.launch({ headless: true })
  const context = await browser.newContext({
    userAgent:
      'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 ' +
      '(KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36',
    locale: 'zh-TW',
    timezoneId: 'Asia/Taipei',
  })
  const page = await context.newPage()

  // 模擬隨機等待（人類操作不會精確到毫秒）
  const randomDelay = () =>
    page.waitForTimeout(800 + Math.floor(Math.random() * 1200))

  await page.goto('https://target.example.com')
  await randomDelay()

  // ... 操作
  await browser.close()
}
```

安裝：

```bash
npm install playwright-extra puppeteer-extra-plugin-stealth
```

---

&nbsp;

### 🍀 1603.01 - 帶重試機制的健壯自動化封裝

將 `withRetry`（見 1601.02）應用到具體的 RPA 操作流程，讓整個批次任務更為堅固。

```typescript
import { Page } from 'playwright'

// 帶重試的訂單確認操作
async function confirmOrderWithRetry(
  page: Page,
  orderId: string
): Promise<void> {
  await withRetry(
    async () => {
      await page.goto(`https://admin.example.com/orders/${orderId}`)
      await page.waitForSelector('.order-detail', { timeout: 8000 })

      const statusText = await page
        .locator('.order-status')
        .textContent()
      if (statusText?.includes('已確認')) {
        console.log(`訂單 ${orderId} 已是確認狀態，跳過`)
        return
      }

      await page.getByRole('button', { name: '確認訂單' }).click()
      // 等待成功 Toast 出現
      await page.waitForSelector('.toast-success', { timeout: 5000 })
      console.log(`✅ 訂單 ${orderId} 確認成功`)
    },
    { maxAttempts: 3, delayMs: 2000 }
  )
}

// 批次處理訂單
async function batchConfirmOrders(
  page: Page,
  orderIds: string[]
): Promise<void> {
  const results = { success: 0, fail: 0 }

  for (const orderId of orderIds) {
    try {
      await confirmOrderWithRetry(page, orderId)
      results.success++
    } catch (error) {
      console.error(`❌ 訂單 ${orderId} 最終失敗：`, error)
      results.fail++
    }
  }

  console.log(
    `\n批次完成：成功 ${results.success} 筆，失敗 ${results.fail} 筆`
  )
}
```

---

&nbsp;

### 🍀 1603.02 - 多頁面並行操作（Concurrency 控制）

對獨立任務使用 `Promise.all` 並行執行，可大幅縮短總耗時。但需控制最大並行數，避免超載目標網站或本機資源。

```typescript
import { Browser, BrowserContext } from 'playwright'

interface ProcessItem {
  id: string
  data: Record<string, string>
}

// 並行處理，每批最多 CONCURRENCY 個
async function processItemsInParallel(
  browser: Browser,
  items: ProcessItem[],
  concurrency = 3
): Promise<void> {
  // 將 items 切成每批 concurrency 個
  for (let i = 0; i < items.length; i += concurrency) {
    const batch = items.slice(i, i + concurrency)

    // 每個 item 使用獨立的 context（隔離 Session / cookie）
    await Promise.all(
      batch.map(async (item) => {
        const context: BrowserContext = await browser.newContext({
          storageState: './auth.json',
        })
        const page = await context.newPage()

        try {
          await page.goto(`https://admin.example.com/items/${item.id}`)
          // ... 執行操作
          console.log(`✅ 完成：${item.id}`)
        } catch (error) {
          console.error(`❌ 失敗：${item.id}`, error)
        } finally {
          await context.close() // 每個任務結束後關閉 context
        }
      })
    )

    console.log(`批次 ${Math.ceil((i + concurrency) / concurrency)} 完成`)
  }
}
```

#### 並行策略比較

| 策略 | 說明 | 適合情境 |
|------|------|----------|
| 單一 `page`，逐一執行 | 安全、無風險 | 操作有依賴順序、網站有 rate limit |
| 多 `context` 並行 | 最隔離、最穩定 | 完全獨立的任務（推薦） |
| 同一 `context` 多 `page` | Cookies 共享 | 需要同一登入 Session 的並行操作 |

---

&nbsp;

### 🍀 1604.01 - 攔截 Network Response 取得下載 URL

有些下載按鈕不直接觸發 `download` 事件，而是先打 API 取得一個暫時下載 URL，再由前端跳轉。此時需監聽 network response 來攔截該 URL。

```typescript
import { chromium, Response } from 'playwright'
import * as https from 'https'
import * as fs from 'fs'

async function interceptAndDownload(
  reportId: string,
  outputPath: string
): Promise<void> {
  const browser = await chromium.launch({ headless: true })
  const context = await browser.newContext({ storageState: './auth.json' })
  const page = await context.newPage()

  try {
    let downloadUrl: string | null = null

    // 監聽所有 Response，找到含下載 URL 的 API 回應
    page.on('response', async (response: Response) => {
      if (!response.url().includes('/api/reports/download')) return

      try {
        const body = await response.json()
        if (body?.downloadUrl) {
          downloadUrl = body.downloadUrl as string
          console.log('攔截到下載 URL：', downloadUrl)
        }
      } catch {
        // 非 JSON response，忽略
      }
    })

    await page.goto(`https://admin.example.com/reports/${reportId}`)
    await page.getByRole('button', { name: '匯出' }).click()

    // 等待 downloadUrl 被設定（最多 10 秒）
    await page.waitForFunction(() => (window as any).__dlReady, { timeout: 10000 })
      .catch(() => {/* 用輪詢替代 */})

    // 輪詢等待 downloadUrl
    const start = Date.now()
    while (!downloadUrl && Date.now() - start < 10000) {
      await page.waitForTimeout(300)
    }

    if (!downloadUrl) throw new Error('無法取得下載 URL')

    // 直接用 Node.js 下載
    await new Promise<void>((resolve, reject) => {
      const file = fs.createWriteStream(outputPath)
      https.get(downloadUrl!, (res) => {
        res.pipe(file)
        file.on('finish', () => { file.close(); resolve() })
      }).on('error', reject)
    })

    console.log(`✅ 下載完成：${outputPath}`)
  } finally {
    await browser.close()
  }
}
```

---

&nbsp;

### 🍀 1605.01 - 記憶體內容直接上傳（不需要實體檔案）

`setInputFiles` 支援傳入 `{ name, mimeType, buffer }` 物件，可直接上傳程式動態產生的內容，無需先寫到磁碟。

```typescript
import { chromium } from 'playwright'

async function uploadGeneratedContent(): Promise<void> {
  const browser = await chromium.launch({ headless: true })
  const context = await browser.newContext({ storageState: './auth.json' })
  const page = await context.newPage()

  try {
    await page.goto('https://admin.example.com/import')

    // 範例 1：上傳動態生成的 CSV（不需要寫入磁碟）
    const csvContent = [
      'name,email,role',
      'Alice,alice@example.com,editor',
      'Bob,bob@example.com,viewer',
    ].join('\n')

    await page.locator('input[type="file"]').setInputFiles({
      name: 'users.csv',
      mimeType: 'text/csv',
      buffer: Buffer.from(csvContent, 'utf-8'),
    })

    // 範例 2：上傳含 BOM 的 UTF-8 CSV（Excel 正確顯示中文必備）
    const BOM = Buffer.from([0xef, 0xbb, 0xbf])  // UTF-8 BOM
    const csvWithBom = Buffer.concat([
      BOM,
      Buffer.from('姓名,電子郵件,部門\n王小明,wang@example.com,工程部', 'utf-8'),
    ])

    await page.locator('input[type="file"]').setInputFiles({
      name: 'employees.csv',
      mimeType: 'text/csv',
      buffer: csvWithBom,
    })

    await page.getByRole('button', { name: '開始匯入' }).click()
    await page.waitForSelector('.import-success', { timeout: 10000 })
    console.log('✅ 記憶體內容上傳成功')
  } finally {
    await browser.close()
  }
}
```

#### 使用情境

| 情境 | 做法 |
|------|------|
| 上傳動態產生的 CSV / JSON | `Buffer.from(content, 'utf-8')` |
| Excel 開啟中文不亂碼 | 加入 UTF-8 BOM（`0xEF 0xBB 0xBF`） |
| 上傳從 API 取得的二進位資料 | `Buffer.from(apiResponse.data)` |
| 測試上傳功能（不需要真實檔案） | 傳入模擬的 buffer 即可 |

---

&nbsp;

### 🍀 1606.01 - 生成 HTML 視覺化報告

將自動化截圖與擷取的資料整合成單一 HTML 報告，方便分發給非技術人員查閱。

```typescript
import * as fs from 'fs'
import * as path from 'path'

interface SectionResult {
  title: string
  screenshotPath: string
  metrics: Array<{ label: string; value: string }>
  capturedAt: string
}

function generateHtmlReport(
  results: SectionResult[],
  outputPath: string
): void {
  const sectionsHtml = results
    .map((section) => {
      // 將截圖轉為 base64 嵌入 HTML（報告完全自包含）
      const imgBuffer = fs.readFileSync(section.screenshotPath)
      const base64 = imgBuffer.toString('base64')
      const mimeType = 'image/png'

      const metricsHtml = section.metrics
        .map(
          (m) => `
          <tr>
            <td style="padding:6px 12px;border-bottom:1px solid #eee;color:#666;">${m.label}</td>
            <td style="padding:6px 12px;border-bottom:1px solid #eee;font-weight:bold;">${m.value}</td>
          </tr>`
        )
        .join('')

      return `
        <div style="margin-bottom:40px;border:1px solid #ddd;border-radius:8px;overflow:hidden;">
          <div style="background:#1a1a2e;color:white;padding:12px 20px;">
            <h2 style="margin:0;font-size:18px;">${section.title}</h2>
            <small style="opacity:0.7;">擷取時間：${section.capturedAt}</small>
          </div>
          <div style="display:flex;gap:20px;padding:20px;">
            <img src="data:${mimeType};base64,${base64}"
                 style="max-width:60%;border:1px solid #ddd;border-radius:4px;" />
            <table style="flex:1;height:fit-content;border-collapse:collapse;">
              ${metricsHtml}
            </table>
          </div>
        </div>`
    })
    .join('')

  const html = `<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <title>自動化監控報告 — ${new Date().toLocaleDateString('zh-TW')}</title>
  <style>
    body { font-family: 'Noto Sans TC', sans-serif; max-width: 1200px; margin: 40px auto; padding: 0 20px; color: #333; }
    h1 { border-bottom: 3px solid #1a1a2e; padding-bottom: 12px; }
  </style>
</head>
<body>
  <h1>📊 自動化監控報告</h1>
  <p>報告生成時間：${new Date().toLocaleString('zh-TW')}</p>
  ${sectionsHtml}
</body>
</html>`

  fs.writeFileSync(outputPath, html, 'utf-8')
  console.log(`✅ HTML 報告已生成：${outputPath}`)
}
```

---

&nbsp;

### 🍀 1606.02 - 差異比對監控報告

定期爬取關鍵數值並與上次快照比對，若有差異則發出警示。適合競品監控、商品價格追蹤等情境。

```typescript
import * as fs from 'fs'

interface Snapshot {
  capturedAt: string
  items: Array<{ id: string; name: string; value: string }>
}

interface DiffResult {
  id: string
  name: string
  oldValue: string
  newValue: string
}

function compareSnapshots(
  prevSnapshot: Snapshot,
  currSnapshot: Snapshot
): DiffResult[] {
  const prevMap = new Map(prevSnapshot.items.map((i) => [i.id, i]))
  const diffs: DiffResult[] = []

  for (const curr of currSnapshot.items) {
    const prev = prevMap.get(curr.id)
    if (prev && prev.value !== curr.value) {
      diffs.push({
        id: curr.id,
        name: curr.name,
        oldValue: prev.value,
        newValue: curr.value,
      })
    }
  }

  return diffs
}

// 典型使用流程：價格監控
async function priceMonitor(): Promise<void> {
  const SNAPSHOT_FILE = './price-snapshot.json'

  // 爬取今日價格（省略爬取細節，見 1602）
  const todaySnapshot: Snapshot = {
    capturedAt: new Date().toISOString(),
    items: [
      { id: 'prod-001', name: 'Playwright 課程', value: 'NT$1,999' },
      { id: 'prod-002', name: '技術顧問包月', value: 'NT$8,800' },
    ],
  }

  if (fs.existsSync(SNAPSHOT_FILE)) {
    const prevSnapshot: Snapshot = JSON.parse(
      fs.readFileSync(SNAPSHOT_FILE, 'utf-8')
    )
    const diffs = compareSnapshots(prevSnapshot, todaySnapshot)

    if (diffs.length > 0) {
      console.log(`\n⚠️ 偵測到 ${diffs.length} 項變動：`)
      diffs.forEach((d) =>
        console.log(`  ${d.name}：${d.oldValue} → ${d.newValue}`)
      )
      // 可在此接入 Email / Slack 通知
    } else {
      console.log('✅ 無變動')
    }
  }

  // 更新快照
  fs.writeFileSync(SNAPSHOT_FILE, JSON.stringify(todaySnapshot, null, 2))
}
```

---

&nbsp;

### 🍀 1607.01 - 從 HTML 模板生成發票 PDF（完整範例）

使用 `page.setContent()` 搭配精心設計的 HTML 模板，可不依賴任何後端產生樣式精美的 PDF 發票。

```typescript
import { chromium } from 'playwright'

interface InvoiceItem {
  description: string
  quantity: number
  unitPrice: number
}

interface Invoice {
  invoiceNo: string
  issueDate: string
  customerName: string
  customerEmail: string
  items: InvoiceItem[]
}

function buildInvoiceHtml(invoice: Invoice): string {
  const fmt = (n: number) =>
    new Intl.NumberFormat('zh-TW', {
      style: 'currency',
      currency: 'TWD',
      minimumFractionDigits: 0,
    }).format(n)

  const total = invoice.items.reduce(
    (sum, item) => sum + item.quantity * item.unitPrice,
    0
  )
  const tax = Math.round(total * 0.05)

  const itemsHtml = invoice.items
    .map(
      (item) => `
      <tr>
        <td>${item.description}</td>
        <td class="num">${item.quantity}</td>
        <td class="num">${fmt(item.unitPrice)}</td>
        <td class="num">${fmt(item.quantity * item.unitPrice)}</td>
      </tr>`
    )
    .join('')

  return `<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700&display=swap');
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { font-family: 'Noto Sans TC', sans-serif; padding: 40px; color: #333; font-size: 14px; }
    .header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 40px; }
    .logo { font-size: 28px; font-weight: 700; color: #1a1a2e; }
    .invoice-meta { text-align: right; }
    .invoice-no { font-size: 22px; font-weight: 700; color: #1a1a2e; }
    .customer-box { background: #f8f9fa; border-left: 4px solid #1a1a2e; padding: 16px 20px; margin-bottom: 30px; }
    table { width: 100%; border-collapse: collapse; margin-bottom: 20px; }
    thead tr { background: #1a1a2e; color: white; }
    th, td { padding: 10px 14px; text-align: left; border-bottom: 1px solid #eee; }
    .num { text-align: right; }
    .total-section { text-align: right; }
    .total-row { font-size: 18px; font-weight: 700; color: #1a1a2e; }
    .footer { margin-top: 60px; text-align: center; color: #999; font-size: 12px; border-top: 1px solid #eee; padding-top: 20px; }
  </style>
</head>
<body>
  <div class="header">
    <div>
      <div class="logo">My Company</div>
      <div style="color:#666;margin-top:4px;">mycompany.example.com</div>
    </div>
    <div class="invoice-meta">
      <div class="invoice-no">發票 ${invoice.invoiceNo}</div>
      <div style="color:#666;margin-top:4px;">開立日期：${invoice.issueDate}</div>
    </div>
  </div>

  <div class="customer-box">
    <div style="font-weight:700;margin-bottom:4px;">客戶資訊</div>
    <div>${invoice.customerName}</div>
    <div style="color:#666;">${invoice.customerEmail}</div>
  </div>

  <table>
    <thead>
      <tr><th>品項說明</th><th class="num">數量</th><th class="num">單價</th><th class="num">小計</th></tr>
    </thead>
    <tbody>
      ${itemsHtml}
    </tbody>
  </table>

  <div class="total-section">
    <div style="margin-bottom:6px;">小計：${fmt(total)}</div>
    <div style="margin-bottom:6px;">稅額（5%）：${fmt(tax)}</div>
    <div class="total-row">總計：${fmt(total + tax)}</div>
  </div>

  <div class="footer">
    感謝您的惠顧！如有疑問請聯繫 billing@mycompany.example.com
  </div>
</body>
</html>`
}

async function generateInvoicePdf(
  invoice: Invoice,
  outputPath: string
): Promise<void> {
  const browser = await chromium.launch({ headless: true })
  const page = await browser.newPage()

  try {
    const html = buildInvoiceHtml(invoice)
    await page.setContent(html, { waitUntil: 'networkidle' })

    await page.pdf({
      path: outputPath,
      format: 'A4',
      printBackground: true,
      margin: { top: '15mm', right: '15mm', bottom: '15mm', left: '15mm' },
    })

    console.log(`✅ 發票 PDF 已生成：${outputPath}`)
  } finally {
    await browser.close()
  }
}

// 使用範例
generateInvoicePdf(
  {
    invoiceNo: 'INV-2026-042',
    issueDate: '2026-05-19',
    customerName: '測試科技股份有限公司',
    customerEmail: 'contact@testtech.example.com',
    items: [
      { description: 'Playwright 自動化顧問服務', quantity: 8, unitPrice: 1500 },
      { description: '技術文件撰寫', quantity: 1, unitPrice: 3000 },
    ],
  },
  './invoices/INV-2026-042.pdf'
)
```

---

&nbsp;

### 🍀 1607.02 - CSS `@media print` 列印排版技巧

PDF 輸出實際上走的是瀏覽器列印管道，因此所有 `@media print` CSS 規則都會生效。善用這點可精確控制 PDF 版面。

#### 常用 `@media print` 技巧

```css
@media print {
  /* 隱藏不需要出現在 PDF 的元素 */
  .sidebar, nav, .print-hide { display: none !important; }

  /* 避免在元素中間自動分頁 */
  .invoice-item, table tr { page-break-inside: avoid; }

  /* 強制在特定元素前分頁（每張訂單佔一頁）*/
  .order-card { page-break-before: always; }

  /* 強制在特定元素後分頁 */
  .chapter-end { page-break-after: always; }

  /* 設定背景色需搭配 -webkit-print-color-adjust */
  * { -webkit-print-color-adjust: exact; print-color-adjust: exact; }
}
```

#### 在 Playwright 中動態注入 Print CSS

```typescript
import { Page } from 'playwright'

async function injectPrintStyles(page: Page): Promise<void> {
  await page.evaluate(() => {
    const style = document.createElement('style')
    style.textContent = `
      @media print {
        .no-print { display: none !important; }
        .page-break { page-break-before: always; }
        body { font-size: 12pt; }
        a { color: inherit; text-decoration: none; }
      }
    `
    document.head.appendChild(style)
  })
}

// 使用範例：在生成 PDF 前注入樣式
async function generateWithCustomPrintStyle(
  page: Page,
  outputPath: string
): Promise<void> {
  await page.goto('https://admin.example.com/report/monthly')
  await page.waitForLoadState('networkidle')

  // 動態注入列印樣式
  await injectPrintStyles(page)

  await page.pdf({
    path: outputPath,
    format: 'A4',
    printBackground: true,
  })
}
```

#### `page-break` 屬性速查

| CSS 屬性 | 值 | 效果 |
|----------|-----|------|
| `page-break-before` | `always` | 此元素前強制分頁 |
| `page-break-after` | `always` | 此元素後強制分頁 |
| `page-break-inside` | `avoid` | 禁止在此元素內部分頁 |
| `orphans` | `3`（數字） | 段落結尾最少保留幾行在同頁 |
| `widows` | `3`（數字） | 段落開頭最少保留幾行在同頁 |

> **注意**：現代 CSS 規範已改用 `break-before` / `break-after` / `break-inside`，但 Chromium 仍完整支援舊版 `page-break-*` 屬性。

---

&nbsp;
