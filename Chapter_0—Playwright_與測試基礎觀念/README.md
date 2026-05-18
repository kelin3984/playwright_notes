# 💫 Chapter 0 — Playwright 與測試基礎觀念

> 建立正確測試觀念與 Playwright 心智模型。

&nbsp;

## 目錄

- [💫 Chapter 0 — Playwright 與測試基礎觀念](#-chapter-0--playwright-與測試基礎觀念)
  - [目錄](#目錄)
  - [🍀 0001 - 測試種類](#-0001---測試種類)
    - [測試金字塔](#測試金字塔)
  - [🍀 0002 - E2E 測試觀念](#-0002---e2e-測試觀念)
    - [核心思維](#核心思維)
    - [正確觀念 vs 錯誤觀念](#正確觀念-vs-錯誤觀念)
    - [E2E 適合測試什麼？](#e2e-適合測試什麼)
  - [🍀 0003 - Playwright 核心架構](#-0003---playwright-核心架構)
    - [各層說明](#各層說明)
  - [🍀 0004 - Playwright vs Cypress vs Selenium](#-0004---playwright-vs-cypress-vs-selenium)
    - [架構比較](#架構比較)
    - [核心差異重點](#核心差異重點)
    - [選型建議](#選型建議)

---

&nbsp;
## 🍀 0001 - 測試種類

軟體測試依照範圍與目的可分為四大類型：

| 類型 | 範圍 | 說明 | 工具範例 |
|------|------|------|----------|
| **Unit Test** | 最小單元 | 測試單一 function / class，不依賴外部資源 | Vitest、Jest |
| **Integration Test** | 模組整合 | 測試多個模組或服務之間的介面與協作 | Jest、Testing Library |
| **Component Test** | UI 元件 | 在隔離環境中掛載並測試單一 UI 元件 | Playwright CT、Storybook |
| **E2E Test** | 全端流程 | 模擬真實使用者在瀏覽器中的完整操作流程 | Playwright、Cypress |

### 測試金字塔

```txt
         ▲
       /E2E\         ← 少量，成本高，執行慢
      /─────\
     / Integ \       ← 中量
    /─────────\
   /  Unit Test \    ← 大量，成本低，執行快
  /─────────────\
```

> **原則**：越底層的測試越多、越快、越便宜；E2E 測試應聚焦在關鍵流程，而非覆蓋所有邏輯。

---

&nbsp;
## 🍀 0002 - E2E 測試觀念

### 核心思維

```txt
E2E = 模擬真實使用者行為
```

E2E 測試的角色不是「測試 function」，而是站在使用者的視角，驗證整個系統端到端能否正常運作。

### 正確觀念 vs 錯誤觀念

| ❌ 錯誤觀念 | ✅ 正確觀念 |
|------------|------------|
| 用 E2E 測試每一個 function | 用 E2E 測試關鍵使用者流程 |
| E2E 取代 Unit Test | 各層測試互補，不能互相取代 |
| 越多 E2E 越好 | E2E 應精準覆蓋 critical path |
| 測試能通過就代表程式正確 | 測試只驗證「被測情境」，無法涵蓋所有邊界 |

### E2E 適合測試什麼？

```txt
✅ 登入 / 登出流程
✅ 結帳 / 付款流程
✅ 表單送出與錯誤提示
✅ 跨頁面的狀態流轉
✅ 第三方整合（OAuth、SSO）
```

---

&nbsp;
## 🍀 0003 - Playwright 核心架構

Playwright 的架構由上到下分為五層：

```txt
┌─────────────────────────┐
│      Test Runner        │  ← 管理測試執行、報告、retry
├─────────────────────────┤
│        Browser          │  ← Chromium / Firefox / WebKit 進程
├─────────────────────────┤
│     BrowserContext      │  ← 隔離的瀏覽器環境（類似無痕視窗）
├─────────────────────────┤
│          Page           │  ← 單一分頁，操作 DOM / 網路 / 對話框
├─────────────────────────┤
│         Locator         │  ← 元素選取器，內建 auto-wait
└─────────────────────────┘
```

### 各層說明

**Test Runner**
- 負責發現並執行 `*.spec.ts` 測試檔案
- 支援平行執行、retry、報告產生
- 設定來源：`playwright.config.ts`

**Browser**
- 代表一個瀏覽器進程（Chromium / Firefox / WebKit）
- 透過 `chromium.launch()` 建立

**BrowserContext**
- 每個 Context 是完全隔離的瀏覽器環境
- 各自擁有獨立的 Cookie、Storage、Session
- 預設每個 test 都有獨立的 Context → **自動隔離**

```ts
const context = await browser.newContext()
const page = await context.newPage()
```

**Page**
- 代表一個分頁
- 提供 `goto()`、`click()`、`fill()`、`waitForResponse()` 等操作

**Locator**
- 用來描述「要操作的元素」
- 具備 **auto-wait**：操作前自動等待元素可見 / 可互動
- 與 `querySelector` 最大差異：Locator 是惰性求值，不會立即查詢 DOM

```ts
// Locator 不會立即查詢 DOM，點擊時才自動等待元素就緒
const btn = page.getByRole('button', { name: '送出' })
await btn.click()
```

---

&nbsp;
## 🍀 0004 - Playwright vs Cypress vs Selenium

### 架構比較

| 面向 | Playwright | Cypress | Selenium |
|------|-----------|---------|----------|
| **架構** | 跨進程 CDP / WebSocket | 同進程注入 JS | WebDriver 協議 |
| **執行位置** | 瀏覽器進程外 | 瀏覽器進程內 | 瀏覽器進程外 |
| **語言支援** | JS/TS、Python、Java、C# | JS/TS | 多語言 |
| **瀏覽器支援** | Chromium、Firefox、WebKit | Chrome、Firefox、Edge | 所有主流瀏覽器 |
| **Auto Wait** | ✅ 基於可互動狀態（actionability） | ✅ 基於 DOM 存在 | ❌ 需手動 `WebDriverWait` |
| **iframe 支援** | ✅ 完整 | ⚠️ 受限 | ✅ 完整 |
| **多分頁 / popup** | ✅ 原生支援 | ⚠️ 限制較多 | ✅ 支援 |
| **執行速度** | 快 | 中 | 慢 |
| **穩定性** | 高 | 中高 | 中（需自行處理等待） |

### 核心差異重點

**Playwright 的優勢**
- 使用 **Chrome DevTools Protocol（CDP）**，直接與瀏覽器底層溝通，速度快且控制力強
- **Auto-wait** 基於元素的可互動狀態（actionability），比 Cypress 的 DOM 存在判斷更精準
- 原生支援多分頁、iframe、Shadow DOM、WebSocket
- 支援多語言（Python / Java / C#），適合跨團隊推廣

**Cypress 的優勢**
- 開發者體驗（DX）佳，Debug 介面直覺
- 適合前端工程師快速上手
- Time-travel debugging

**Selenium 的優勢**
- 歷史最久、生態最成熟
- 支援最廣泛的瀏覽器版本與平台
- 企業老系統整合度高

### 選型建議

```txt
新專案 / 前端工程師主導   → Playwright（首選）
已有 Cypress、輕量需求   → 維持 Cypress
多語言團隊 / 老系統整合  → Selenium / Playwright
```
