# 💫 Chapter 1 — Playwright 基本操作

> 學會初始化與執行第一個 Playwright 測試。

&nbsp;

## 目錄

- [💫 Chapter 1 — Playwright 基本操作](#-chapter-1--playwright-基本操作)
  - [目錄](#目錄)
  - [🍀 0101 - 安裝](#-0101---安裝)
    - [互動式安裝流程](#互動式安裝流程)
    - [安裝後產生的檔案](#安裝後產生的檔案)
    - [單獨安裝瀏覽器 binary](#單獨安裝瀏覽器-binary)
  - [🍀 0102 - 第一個測試](#-0102---第一個測試)
    - [最小測試範例](#最小測試範例)
    - [測試結構說明](#測試結構說明)
    - [執行測試](#執行測試)
  - [🍀 0103 - 啟動瀏覽器](#-0103---啟動瀏覽器)
    - [兩種模式：Library vs Test Runner](#兩種模式library-vs-test-runner)
    - [手動管理生命週期（Library 模式）](#手動管理生命週期library-模式)
    - [Test Runner 自動管理（推薦）](#test-runner-自動管理推薦)
  - [🍀 0104 - headed vs headless](#-0104---headed-vs-headless)
    - [模式比較](#模式比較)
    - [切換方式](#切換方式)
  - [🍀 0105 - Playwright Test Runner](#-0105---playwright-test-runner)
    - [Test Runner 職責](#test-runner-職責)
    - [Library 模式 vs Test Runner 模式](#library-模式-vs-test-runner-模式)
  - [🍀 0106 - 專案結構](#-0106---專案結構)
    - [標準目錄結構](#標準目錄結構)
    - [各目錄 / 檔案說明](#各目錄--檔案說明)
  - [🍀 0107 - playwright.config.ts 深度設定](#-0107---playwrightconfigts-深度設定)
    - [完整範例](#完整範例)
    - [五大重點設定說明](#五大重點設定說明)
  - [🍀 0108 - CLI 常用指令](#-0108---cli-常用指令)
    - [指令總覽](#指令總覽)
    - [常用組合範例](#常用組合範例)

---

&nbsp;

## 🍀 0101 - 安裝

### 互動式安裝流程

```bash
npm init playwright@latest
```

執行後會出現互動式問答，依序設定：

| 問題 | 說明 | 推薦選項 |
|------|------|----------|
| Where to put your end-to-end tests? | 測試檔案放置目錄 | `tests`（預設） |
| Add a GitHub Actions workflow? | 是否產生 CI 設定 | `true`（建議） |
| Install Playwright browsers? | 是否下載瀏覽器 binary | `true` |

### 安裝後產生的檔案

```txt
.
├── tests/
│   └── example.spec.ts       ← 範例測試檔
├── tests-examples/
│   └── demo-todo-app.spec.ts ← 更完整的範例
├── playwright.config.ts      ← 主要設定檔
├── package.json
└── .github/
    └── workflows/
        └── playwright.yml    ← GitHub Actions CI（如有選擇）
```

### 單獨安裝瀏覽器 binary

```bash
# 安裝所有瀏覽器（Chromium / Firefox / WebKit）
npx playwright install

# 只安裝特定瀏覽器
npx playwright install chromium
npx playwright install firefox

# 安裝含系統依賴（Linux / CI 環境常用）
npx playwright install --with-deps
```

---

&nbsp;

## 🍀 0102 - 第一個測試

### 最小測試範例

```ts
import { test, expect } from '@playwright/test'

test('homepage 標題正確', async ({ page }) => {
  // 導航至目標頁面
  await page.goto('https://playwright.dev')

  // 斷言頁面標題包含指定文字
  await expect(page).toHaveTitle(/Playwright/)
})
```

### 測試結構說明

| 部分 | 說明 |
|------|------|
| `test(title, fn)` | 定義一個測試案例，`title` 為名稱，`fn` 為非同步測試函式 |
| `{ page }` | Playwright 自動注入的 fixture，代表一個瀏覽器分頁 |
| `page.goto(url)` | 導航到指定 URL，等待頁面 load 完成 |
| `expect(page).toHaveTitle()` | 斷言當前頁面標題，具備 auto-retry |

### 執行測試

```bash
# 執行所有測試
npx playwright test

# 執行後自動開啟 HTML 報告
npx playwright test --reporter=html
npx playwright show-report
```

---

&nbsp;

## 🍀 0103 - 啟動瀏覽器

### 兩種模式：Library vs Test Runner

Playwright 可以用兩種方式使用：

| 模式 | 使用場景 | 生命週期管理 |
|------|----------|--------------|
| **Test Runner 模式** | 撰寫測試（`*.spec.ts`） | 自動管理（推薦） |
| **Library 模式** | 自定義腳本、爬蟲、RPA | 手動管理 |

### 手動管理生命週期（Library 模式）

```ts
import { chromium } from '@playwright/test'

const browser = await chromium.launch()           // 啟動瀏覽器進程
const context = await browser.newContext()         // 建立隔離的 Context
const page = await context.newPage()               // 建立分頁

await page.goto('https://example.com')
await page.screenshot({ path: 'screenshot.png' })

await context.close()   // 關閉 Context（清除 Cookie / Storage）
await browser.close()   // 關閉瀏覽器進程
```

### Test Runner 自動管理（推薦）

```ts
import { test, expect } from '@playwright/test'

// Test Runner 自動建立 context 與 page，測試結束後自動清除
test('自動管理', async ({ browser, context, page }) => {
  await page.goto('https://example.com')
  // 不需要手動 close
})
```

> **重點**：每個 `test` 預設都有**獨立的 BrowserContext**，確保測試間完全隔離，不會互相污染 Cookie / Session。

---

&nbsp;

## 🍀 0104 - headed vs headless

### 模式比較

| 面向 | headless（無頭） | headed（有頭） |
|------|-----------------|----------------|
| **瀏覽器視窗** | 不開啟，在背景執行 | 開啟可見視窗 |
| **執行速度** | 較快 | 較慢 |
| **適用場景** | CI/CD、自動化執行 | 本地開發、除錯 |
| **資源消耗** | 較低 | 較高 |
| **預設值** | ✅ 預設 headless | 需手動指定 |

### 切換方式

**方式一：CLI 旗標**

```bash
# headed 模式（開啟瀏覽器視窗）
npx playwright test --headed

# headless 模式（預設，可省略）
npx playwright test
```

**方式二：playwright.config.ts**

```ts
import { defineConfig } from '@playwright/test'

export default defineConfig({
  use: {
    headless: false,  // true = headless（預設）, false = headed
  },
})
```

**方式三：程式碼中指定（Library 模式）**

```ts
const browser = await chromium.launch({
  headless: false,  // 開啟有頭模式
  slowMo: 500,      // 每個動作暫停 500ms，方便觀察
})
```

---

&nbsp;

## 🍀 0105 - Playwright Test Runner

### Test Runner 職責

```txt
┌─────────────────────────────────────────────┐
│              Playwright Test Runner          │
│                                             │
│  ① 發現測試檔案（*.spec.ts）                │
│  ② 依 projects 設定平行分配至各 Worker      │
│  ③ 每個 test 建立獨立 BrowserContext        │
│  ④ 執行 test，捕捉錯誤與 assertion 結果     │
│  ⑤ retry 失敗的測試（依 retries 設定）      │
│  ⑥ 產生測試報告（HTML / JSON / GitHub…）    │
└─────────────────────────────────────────────┘
```

### Library 模式 vs Test Runner 模式

| 功能 | Library 模式 | Test Runner 模式 |
|------|-------------|-----------------|
| **平行執行** | 需自行實作 | ✅ 內建 |
| **自動 retry** | 需自行實作 | ✅ 內建 |
| **報告產生** | ❌ | ✅ HTML / JSON / JUnit |
| **Fixtures** | ❌ | ✅ `{ page, context, browser }` |
| **Hooks** | ❌ | ✅ `beforeAll / afterEach` |
| **assert（expect）** | 需另外引入 | ✅ 內建 auto-retry assertion |
| **trace / screenshot / video** | 需手動呼叫 | ✅ 設定後自動產生 |

> **結論**：撰寫測試時永遠使用 Test Runner 模式；Library 模式僅用於爬蟲、RPA 等非測試腳本。

---

&nbsp;

## 🍀 0106 - 專案結構

### 標準目錄結構

```txt
my-project/
├── tests/                        ← 測試檔案目錄
│   ├── auth/
│   │   └── login.spec.ts
│   ├── dashboard/
│   │   └── dashboard.spec.ts
│   └── example.spec.ts
├── tests-examples/               ← 官方範例（可刪除）
├── pages/                        ← Page Object Model（建議建立）
│   └── LoginPage.ts
├── fixtures/                     ← 自定義 fixtures（建議建立）
│   └── index.ts
├── .auth/                        ← storageState 存放（自動產生）
│   └── user.json
├── test-results/                 ← 測試結果（自動產生，加入 .gitignore）
├── playwright-report/            ← HTML 報告（自動產生）
├── playwright.config.ts          ← 主要設定檔
├── package.json
└── tsconfig.json
```

### 各目錄 / 檔案說明

| 路徑 | 說明 |
|------|------|
| `tests/` | 放置所有測試檔案（`*.spec.ts`），建議依功能分資料夾 |
| `pages/` | Page Object Model 類別，封裝頁面操作邏輯 |
| `fixtures/` | 自定義 fixtures，擴充 `test` 物件的注入能力 |
| `.auth/` | 儲存登入後的 `storageState`（Cookie / Token），供測試復用 |
| `test-results/` | 失敗截圖、trace 檔案，**加入 `.gitignore`** |
| `playwright-report/` | HTML 報告，**加入 `.gitignore`** |
| `playwright.config.ts` | 全域設定：瀏覽器、timeout、baseURL、報告格式等 |

---

&nbsp;

## 🍀 0107 - playwright.config.ts 深度設定

### 完整範例

```ts
import { defineConfig, devices } from '@playwright/test'

export default defineConfig({
  // 測試檔案目錄
  testDir: './tests',

  // 全域 timeout（單一 test 最長執行時間）
  timeout: 30 * 1000,

  // expect() 的 timeout
  expect: {
    timeout: 5000,
  },

  // 失敗自動 retry 次數（CI 建議設 2）
  retries: process.env.CI ? 2 : 0,

  // Worker 數量（平行度）
  workers: process.env.CI ? 1 : undefined,

  // 報告格式
  reporter: [
    ['html'],                          // 產生 HTML 報告
    ['github'],                        // GitHub Actions 友善輸出
    ['json', { outputFile: 'results.json' }],
  ],

  // 全域共用設定（所有 project 繼承）
  use: {
    baseURL: 'http://localhost:3000',  // 讓 page.goto('/login') 自動補齊
    trace: 'on-first-retry',           // 失敗第一次 retry 時錄製 trace
    screenshot: 'only-on-failure',     // 失敗時截圖
    video: 'retain-on-failure',        // 失敗時保留影片
    viewport: { width: 1280, height: 720 },
    locale: 'zh-TW',
    timezoneId: 'Asia/Taipei',
  },

  // 多瀏覽器 / 多環境設定
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
    // 行動裝置模擬
    {
      name: 'Mobile Chrome',
      use: { ...devices['Pixel 5'] },
    },
  ],

  // 自動啟動前端 dev server（執行測試前先啟動）
  webServer: {
    command: 'npm run dev',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
    timeout: 120 * 1000,
  },
})
```

### 五大重點設定說明

**① `projects` — 多瀏覽器 / 多環境**

```ts
projects: [
  // 跨瀏覽器測試
  { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
  { name: 'firefox',  use: { ...devices['Desktop Firefox'] } },

  // 多環境（staging / production）
  {
    name: 'staging',
    use: { baseURL: 'https://staging.example.com' },
  },
]
```

> 執行特定 project：`npx playwright test --project=chromium`

---

**② `use` — 共用瀏覽器行為設定**

| 設定項 | 說明 | 範例值 |
|--------|------|--------|
| `baseURL` | 自動補齊相對路徑 | `'http://localhost:3000'` |
| `viewport` | 視窗大小 | `{ width: 1280, height: 720 }` |
| `locale` | 瀏覽器語系 | `'zh-TW'` |
| `timezoneId` | 時區 | `'Asia/Taipei'` |
| `storageState` | 預載 Cookie / Token | `'.auth/user.json'` |
| `trace` | trace 錄製時機 | `'on-first-retry'` |
| `screenshot` | 截圖時機 | `'only-on-failure'` |
| `video` | 影片錄製時機 | `'retain-on-failure'` |

---

**③ `webServer` — 自動啟動前端 dev server**

```ts
webServer: {
  command: 'npm run dev',        // 啟動指令
  url: 'http://localhost:3000',  // Playwright 等待此 URL 回應後才開始測試
  reuseExistingServer: !process.env.CI, // 本地開發時復用已啟動的 server
}
```

> 搭配 `baseURL` 使用，讓測試完全不依賴手動啟動 server。

---

**④ `reporter` — 報告格式**

| reporter | 說明 | 使用時機 |
|----------|------|----------|
| `html` | 產生互動式 HTML 報告 | 本地查閱 |
| `github` | GitHub Actions 友善輸出 | CI |
| `json` | 機器可讀的 JSON 結果 | 外部整合 |
| `junit` | JUnit XML 格式 | 企業 CI 工具 |
| `dot` | 簡潔的點狀輸出 | 本地快速確認 |

---

**⑤ `timeout / retries` — 穩定性控制**

```ts
timeout: 30000,          // 單一 test 最長 30 秒
retries: process.env.CI ? 2 : 0,   // CI 上允許 retry 2 次
workers: process.env.CI ? 1 : undefined, // CI 單執行緒，本地多執行緒
```

---

&nbsp;

## 🍀 0108 - CLI 常用指令

### 指令總覽

| 指令 | 說明 |
|------|------|
| `npx playwright test` | 執行所有測試 |
| `npx playwright test <file>` | 執行指定測試檔案 |
| `npx playwright test --headed` | 以 headed 模式執行（開啟瀏覽器視窗） |
| `npx playwright test --ui` | 開啟 UI Mode（互動式測試瀏覽器） |
| `npx playwright test --debug` | 以 debug 模式執行（開啟 Playwright Inspector） |
| `npx playwright test --grep <pattern>` | 只執行名稱符合 pattern 的測試 |
| `npx playwright test --project <name>` | 只執行指定 project |
| `npx playwright test --last-failed` | 只重新執行上次失敗的測試 |
| `npx playwright test --shard=<i>/<n>` | CI 分片執行（第 i 份，共 n 份） |
| `npx playwright test --workers <n>` | 指定 Worker 數量（平行度） |
| `npx playwright test --retries <n>` | 指定 retry 次數 |
| `npx playwright test --reporter=html` | 指定報告格式 |
| `npx playwright show-report` | 開啟最近一次的 HTML 報告 |
| `npx playwright codegen <url>` | 開啟錄製工具，自動產生測試程式碼 |

### 常用組合範例

```bash
# 本地開發：headed + debug 模式執行單一測試
npx playwright test tests/login.spec.ts --headed --debug

# 只執行名稱含「登入」的測試
npx playwright test --grep "登入"

# 指定瀏覽器跑測試
npx playwright test --project=chromium

# CI 分片：把測試平均切成 3 份，各自在不同 runner 執行
npx playwright test --shard=1/3
npx playwright test --shard=2/3
npx playwright test --shard=3/3

# 只跑上次失敗的測試（快速 re-run）
npx playwright test --last-failed

# 錄製測試（codegen）
npx playwright codegen https://example.com
```

> **提示**：`--ui` 模式（UI Mode）是本地開發時最強大的工具，可以視覺化地逐步執行、時間軸回放，強烈建議熟悉。

```bash
npx playwright test --ui
```
