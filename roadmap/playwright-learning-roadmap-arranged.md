

# Playwright 學習章節地圖（前端工程師通用版）

> 版本：Frontend General Roadmap
> 適用對象：
>
> - Frontend Engineer
> - Vue / React / Angular 開發者
> - 想學 Browser Automation / E2E Testing 的工程師
> - 想建立企業級測試架構的人

&nbsp;
# 目錄
- [Playwright 學習章節地圖（前端工程師通用版）](#playwright-學習章節地圖前端工程師通用版)
- [目錄](#目錄)
- [學習總地圖](#學習總地圖)
- [💫Chapter 0 — Playwright 與測試基礎觀念](#chapter-0--playwright-與測試基礎觀念)
  - [🧩學習目標](#學習目標)
  - [🧩章節內容](#章節內容)
    - [🍀 0001 - 測試種類](#-0001---測試種類)
    - [🍀 0002 - E2E 測試觀念](#-0002---e2e-測試觀念)
    - [🍀 0003 - Playwright 核心架構](#-0003---playwright-核心架構)
    - [🍀 0004 - Playwright vs Cypress vs Selenium](#-0004---playwright-vs-cypress-vs-selenium)
- [💫Chapter 1 — Playwright 基本操作](#chapter-1--playwright-基本操作)
  - [🧩學習目標](#學習目標-1)
  - [🧩章節內容](#章節內容-1)
    - [🍀 0101 - 安裝](#-0101---安裝)
    - [🍀 0102 - 第一個測試](#-0102---第一個測試)
    - [🍀 0103 - 啟動瀏覽器](#-0103---啟動瀏覽器)
    - [🍀 0104 - headed vs headless](#-0104---headed-vs-headless)
    - [🍀 0105 - Playwright Test Runner](#-0105---playwright-test-runner)
    - [🍀 0106 - 專案結構](#-0106---專案結構)
- [💫Chapter 2 — Locator 與 DOM 操作](#chapter-2--locator-與-dom-操作)
  - [🧩學習目標](#學習目標-2)
  - [🧩章節內容](#章節內容-2)
    - [🍀 0201 - Locator 概念](#-0201---locator-概念)
    - [🍀 0202 - getByRole](#-0202---getbyrole)
    - [🍀 0203 - getByText](#-0203---getbytext)
    - [🍀 0204 - getByLabel](#-0204---getbylabel)
    - [🍀 0205 - getByPlaceholder](#-0205---getbyplaceholder)
    - [🍀 0206 - getByTestId](#-0206---getbytestid)
    - [🍀 0207 - locator chaining](#-0207---locator-chaining)
    - [🍀 0208 - strict mode](#-0208---strict-mode)
    - [🍀 0209 - Shadow DOM](#-0209---shadow-dom)
    - [🍀 0210 - iframe](#-0210---iframe)
    - [🍀 0211 - 最佳 selector 策略](#-0211---最佳-selector-策略)
- [💫Chapter 3 — Assertion 與驗證](#chapter-3--assertion-與驗證)
  - [🧩學習目標](#學習目標-3)
  - [🧩章節內容](#章節內容-3)
    - [🍀 0301 - expect](#-0301---expect)
    - [🍀 0302 - toBeVisible](#-0302---tobevisible)
    - [🍀 0303 - toContainText](#-0303---tocontaintext)
    - [🍀 0304 - toHaveValue](#-0304---tohavevalue)
    - [🍀 0305 - toHaveURL](#-0305---tohaveurl)
    - [🍀 0306 - auto retry](#-0306---auto-retry)
    - [🍀 0307 - 軟斷言 vs 硬斷言](#-0307---軟斷言-vs-硬斷言)
- [💫Chapter 4 — Browser 與 Context 機制](#chapter-4--browser-與-context-機制)
  - [🧩學習目標](#學習目標-4)
  - [🧩章節內容](#章節內容-4)
    - [🍀 0401 - Browser](#-0401---browser)
    - [🍀 0402 - BrowserContext](#-0402---browsercontext)
    - [🍀 0403 - Cookie](#-0403---cookie)
    - [🍀 0404 - LocalStorage](#-0404---localstorage)
    - [🍀 0405 - SessionStorage](#-0405---sessionstorage)
    - [🍀 0406 - popup](#-0406---popup)
    - [🍀 0407 - 多分頁](#-0407---多分頁)
- [💫Chapter 5 — 表單與 UI 自動化](#chapter-5--表單與-ui-自動化)
  - [🧩學習目標](#學習目標-5)
  - [🧩章節內容](#章節內容-5)
    - [🍀 0501 - click](#-0501---click)
    - [🍀 0502 - fill](#-0502---fill)
    - [🍀 0503 - keyboard](#-0503---keyboard)
    - [🍀 0504 - select](#-0504---select)
    - [🍀 0505 - upload](#-0505---upload)
    - [🍀 0506 - drag and drop](#-0506---drag-and-drop)
    - [🍀 0507 - hover](#-0507---hover)
    - [🍀 0508 - dialog](#-0508---dialog)
    - [🍀 0509 - clipboard](#-0509---clipboard)
    - [🍀 0510 - file chooser](#-0510---file-chooser)
- [💫Chapter 6 — Network 與 API 控制](#chapter-6--network-與-api-控制)
  - [🧩學習目標](#學習目標-6)
  - [🧩章節內容](#章節內容-6)
    - [🍀 0601 - waitForResponse](#-0601---waitforresponse)
    - [🍀 0602 - route interception](#-0602---route-interception)
    - [🍀 0603 - mock API](#-0603---mock-api)
    - [🍀 0604 - modify response](#-0604---modify-response)
    - [🍀 0605 - fake backend](#-0605---fake-backend)
    - [🍀 0606 - network idle](#-0606---network-idle)
    - [🍀 0607 - GraphQL testing](#-0607---graphql-testing)
    - [🍀 0608 - WebSocket](#-0608---websocket)
- [💫Chapter 7 — Authentication 與 Session](#chapter-7--authentication-與-session)
  - [🧩學習目標](#學習目標-7)
  - [🧩章節內容](#章節內容-7)
    - [🍀 0701 - login automation](#-0701---login-automation)
    - [🍀 0702 - storageState](#-0702---storagestate)
    - [🍀 0703 - token persistence](#-0703---token-persistence)
    - [🍀 0704 - cookie auth](#-0704---cookie-auth)
    - [🍀 0705 - JWT auth](#-0705---jwt-auth)
    - [🍀 0706 - OAuth flow](#-0706---oauth-flow)
    - [🍀 0707 - SSO flow](#-0707---sso-flow)
    - [🍀 0708 - 多角色測試](#-0708---多角色測試)
- [💫Chapter 8 — 測試架構設計](#chapter-8--測試架構設計)
  - [🧩學習目標](#學習目標-8)
  - [🧩章節內容](#章節內容-8)
    - [🍀 0801 - 測試分層](#-0801---測試分層)
    - [🍀 0802 - reusable design](#-0802---reusable-design)
    - [🍀 0803 - fixtures](#-0803---fixtures)
    - [🍀 0804 - hooks](#-0804---hooks)
    - [🍀 0805 - test isolation](#-0805---test-isolation)
    - [🍀 0806 - test lifecycle](#-0806---test-lifecycle)
    - [🍀 0807 - parallel testing](#-0807---parallel-testing)
    - [🍀 0808 - flaky test](#-0808---flaky-test)
- [💫Chapter 9 — 測試資料管理](#chapter-9--測試資料管理)
  - [🧩學習目標](#學習目標-9)
  - [🧩章節內容](#章節內容-9)
    - [🍀 0901 - mock data](#-0901---mock-data)
    - [🍀 0902 - seed data](#-0902---seed-data)
    - [🍀 0903 - test db](#-0903---test-db)
    - [🍀 0904 - cleanup](#-0904---cleanup)
    - [🍀 0905 - dynamic data](#-0905---dynamic-data)
    - [🍀 0906 - faker](#-0906---faker)
- [💫Chapter 10 — Page Object Model（POM）](#chapter-10--page-object-modelpom)
  - [🧩學習目標](#學習目標-10)
  - [🧩章節內容](#章節內容-10)
    - [🍀 1001 - POM 概念](#-1001---pom-概念)
    - [🍀 1002 - page abstraction](#-1002---page-abstraction)
    - [🍀 1003 - reusable methods](#-1003---reusable-methods)
    - [🍀 1004 - component object](#-1004---component-object)
    - [🍀 1005 - anti-pattern](#-1005---anti-pattern)
- [💫Chapter 11 — Debug 與 Trace](#chapter-11--debug-與-trace)
  - [🧩學習目標](#學習目標-11)
  - [🧩章節內容](#章節內容-11)
    - [🍀 1101 - UI mode](#-1101---ui-mode)
    - [🍀 1102 - debug mode](#-1102---debug-mode)
    - [🍀 1103 - pause](#-1103---pause)
    - [🍀 1104 - trace viewer](#-1104---trace-viewer)
    - [🍀 1105 - screenshot](#-1105---screenshot)
    - [🍀 1106 - video](#-1106---video)
    - [🍀 1107 - console log](#-1107---console-log)
    - [🍀 1108 - network inspection](#-1108---network-inspection)
- [💫Chapter 12 — Visual Testing](#chapter-12--visual-testing)
  - [🧩學習目標](#學習目標-12)
  - [🧩章節內容](#章節內容-12)
    - [🍀 1201 - screenshot compare](#-1201---screenshot-compare)
    - [🍀 1202 - snapshot testing](#-1202---snapshot-testing)
    - [🍀 1203 - visual regression](#-1203---visual-regression)
    - [🍀 1204 - masking dynamic content](#-1204---masking-dynamic-content)
- [💫Chapter 13 — Component Testing](#chapter-13--component-testing)
  - [🧩學習目標](#學習目標-13)
  - [🧩章節內容](#章節內容-13)
    - [🍀 1301 - isolated component](#-1301---isolated-component)
    - [🍀 1302 - mount](#-1302---mount)
    - [🍀 1303 - component interaction](#-1303---component-interaction)
    - [🍀 1304 - mocking props](#-1304---mocking-props)
- [💫Chapter 14 — CI/CD 整合](#chapter-14--cicd-整合)
  - [🧩學習目標](#學習目標-14)
  - [🧩章節內容](#章節內容-14)
    - [🍀 1401 - GitHub Actions](#-1401---github-actions)
    - [🍀 1402 - GitLab CI](#-1402---gitlab-ci)
    - [🍀 1403 - Docker](#-1403---docker)
    - [🍀 1404 - headless CI](#-1404---headless-ci)
    - [🍀 1405 - artifact](#-1405---artifact)
    - [🍀 1406 - report upload](#-1406---report-upload)
    - [🍀 1407 - failed retry](#-1407---failed-retry)
- [💫Chapter 15 — 大型專案測試策略](#chapter-15--大型專案測試策略)
  - [🧩學習目標](#學習目標-15)
  - [🧩章節內容](#章節內容-15)
    - [🍀 1501 - test pyramid](#-1501---test-pyramid)
    - [🍀 1502 - risk-based testing](#-1502---risk-based-testing)
    - [🍀 1503 - critical flow](#-1503---critical-flow)
    - [🍀 1504 - smoke test](#-1504---smoke-test)
    - [🍀 1505 - regression test](#-1505---regression-test)
    - [🍀 1506 - release validation](#-1506---release-validation)
- [💫Chapter 16 — Browser Automation / RPA](#chapter-16--browser-automation--rpa)
  - [🧩學習目標](#學習目標-16)
  - [🧩章節內容](#章節內容-16)
    - [🍀 1601 - browser scripting](#-1601---browser-scripting)
    - [🍀 1602 - crawling](#-1602---crawling)
    - [🍀 1603 - auto operation](#-1603---auto-operation)
    - [🍀 1604 - download automation](#-1604---download-automation)
    - [🍀 1605 - upload automation](#-1605---upload-automation)
    - [🍀 1606 - report automation](#-1606---report-automation)
- [💫Chapter 17 — Playwright 進階原理](#chapter-17--playwright-進階原理)
  - [🧩學習目標](#學習目標-17)
  - [🧩章節內容](#章節內容-17)
    - [🍀 1701 - Chrome DevTools Protocol（CDP）](#-1701---chrome-devtools-protocolcdp)
    - [🍀 1702 - auto wait internals](#-1702---auto-wait-internals)
    - [🍀 1703 - browser process](#-1703---browser-process)
    - [🍀 1704 - rendering lifecycle](#-1704---rendering-lifecycle)
    - [🍀 1705 - event loop](#-1705---event-loop)
    - [🍀 1706 - network lifecycle](#-1706---network-lifecycle)
- [💫Chapter 18 — AI Agent 與 Browser Agent](#chapter-18--ai-agent-與-browser-agent)
  - [🧩學習目標](#學習目標-18)
  - [🧩章節內容](#章節內容-18)
    - [🍀 1801 - browser-use](#-1801---browser-use)
    - [🍀 1802 - computer-use](#-1802---computer-use)
    - [🍀 1803 - AI automation](#-1803---ai-automation)
    - [🍀 1804 - agentic browser](#-1804---agentic-browser)
    - [🍀 1805 - LLM + browser control](#-1805---llm--browser-control)
- [推薦學習順序](#推薦學習順序)
- [企業前端最重要章節](#企業前端最重要章節)
- [最能拉開資深度的章節](#最能拉開資深度的章節)

&nbsp;
# 學習總地圖

```txt
0. Playwright 與測試基礎觀念
1. Playwright 基本操作
2. Locator 與 DOM 操作
3. Assertion 與驗證
4. Browser 與 Context 機制
5. 表單、自動化互動
6. Network 與 API 控制
7. Authentication 與 Session
8. 測試架構設計
9. 測試資料管理
10. Page Object Model
11. Debug 與 Trace
12. Visual Testing
13. Component Testing
14. CI/CD 整合
15. 大型專案測試策略
16. Browser Automation / RPA
17. Playwright 進階原理
18. AI Agent 與 Browser Agent
```

&nbsp;
# 💫Chapter 0 — Playwright 與測試基礎觀念

&nbsp;
## 🧩學習目標

建立正確測試觀念與 Playwright 心智模型。

&nbsp;
## 🧩章節內容

### 🍀 0001 - 測試種類

- Unit Test
- Integration Test
- E2E Test
- Component Test

---
&nbsp;
### 🍀 0002 - E2E 測試觀念

理解：

```txt
E2E = 模擬真實使用者行為
```

而不是：

```txt
測試 function
```

---
&nbsp;
### 🍀 0003 - Playwright 核心架構

```txt
Test Runner
Browser
Context
Page
Locator
```

---
&nbsp;
### 🍀 0004 - Playwright vs Cypress vs Selenium

理解：

- 架構差異
- 執行方式
- 穩定性
- auto wait
- browser support

---

&nbsp;
# 💫Chapter 1 — Playwright 基本操作

&nbsp;
## 🧩學習目標

學會初始化與執行第一個 Playwright 測試。

&nbsp;
## 🧩章節內容

### 🍀 0101 - 安裝

```bash
npm init playwright@latest
```

---

&nbsp;
### 🍀 0102 - 第一個測試

```ts
test('homepage', async ({ page }) => {}
```

---

&nbsp;
### 🍀 0103 - 啟動瀏覽器

```ts
browser.newContext()
context.newPage()
```

---

&nbsp;
### 🍀 0104 - headed vs headless

---

&nbsp;
### 🍀 0105 - Playwright Test Runner

---

&nbsp;
### 🍀 0106 - 專案結構

```txt
tests/
playwright.config.ts
```

---

&nbsp;
# 💫Chapter 2 — Locator 與 DOM 操作

&nbsp;
## 🧩學習目標

掌握 Playwright 最核心能力。

&nbsp;
## 🧩章節內容

### 🍀 0201 - Locator 概念

理解：

```txt
Locator != querySelector
```

---

&nbsp;
### 🍀 0202 - getByRole

---

&nbsp;
### 🍀 0203 - getByText

---

&nbsp;
### 🍀 0204 - getByLabel

---

&nbsp;
### 🍀 0205 - getByPlaceholder

---

&nbsp;
### 🍀 0206 - getByTestId

---

&nbsp;
### 🍀 0207 - locator chaining

---

&nbsp;
### 🍀 0208 - strict mode

---

&nbsp;
### 🍀 0209 - Shadow DOM

---

&nbsp;
### 🍀 0210 - iframe

---

&nbsp;
### 🍀 0211 - 最佳 selector 策略

---

&nbsp;
# 💫Chapter 3 — Assertion 與驗證

## 🧩學習目標

學會真正「驗證」頁面狀態。

&nbsp;
## 🧩章節內容

### 🍀 0301 - expect

---

&nbsp;
### 🍀 0302 - toBeVisible

---

&nbsp;
### 🍀 0303 - toContainText

---

&nbsp;
### 🍀 0304 - toHaveValue

---

&nbsp;
### 🍀 0305 - toHaveURL

---

&nbsp;
### 🍀 0306 - auto retry

---

&nbsp;
### 🍀 0307 - 軟斷言 vs 硬斷言

---

&nbsp;
# 💫Chapter 4 — Browser 與 Context 機制

## 🧩學習目標

理解瀏覽器 automation 本質。

&nbsp;
## 🧩章節內容

### 🍀 0401 - Browser

---

&nbsp;
### 🍀 0402 - BrowserContext

```txt
Context ≈ 無痕視窗
```

---

&nbsp;
### 🍀 0403 - Cookie

---

&nbsp;
### 🍀 0404 - LocalStorage

---

&nbsp;
### 🍀 0405 - SessionStorage

---

&nbsp;
### 🍀 0406 - popup

---

&nbsp;
### 🍀 0407 - 多分頁

---

&nbsp;
# 💫Chapter 5 — 表單與 UI 自動化

## 🧩學習目標

模擬真實使用者操作。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 0501 - click

---

&nbsp;
### 🍀 0502 - fill

---

&nbsp;
### 🍀 0503 - keyboard

---

&nbsp;
### 🍀 0504 - select

---

&nbsp;
### 🍀 0505 - upload

---

&nbsp;
### 🍀 0506 - drag and drop

---

&nbsp;
### 🍀 0507 - hover

---

&nbsp;
### 🍀 0508 - dialog

---

&nbsp;
### 🍀 0509 - clipboard

---

&nbsp;
### 🍀 0510 - file chooser

---

&nbsp;
# 💫Chapter 6 — Network 與 API 控制

## 🧩學習目標

學會控制與攔截 network。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 0601 - waitForResponse

---

&nbsp;
### 🍀 0602 - route interception

---

&nbsp;
### 🍀 0603 - mock API

---

&nbsp;
### 🍀 0604 - modify response

---

&nbsp;
### 🍀 0605 - fake backend

---

&nbsp;
### 🍀 0606 - network idle

---

&nbsp;
### 🍀 0607 - GraphQL testing

---

&nbsp;
### 🍀 0608 - WebSocket

---

&nbsp;
# 💫Chapter 7 — Authentication 與 Session

## 🧩學習目標

處理企業系統常見登入流程。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 0701 - login automation

---

&nbsp;
### 🍀 0702 - storageState

---

&nbsp;
### 🍀 0703 - token persistence

---

&nbsp;
### 🍀 0704 - cookie auth

---

&nbsp;
### 🍀 0705 - JWT auth

---

&nbsp;
### 🍀 0706 - OAuth flow

---

&nbsp;
### 🍀 0707 - SSO flow

---

&nbsp;
### 🍀 0708 - 多角色測試

```txt
admin
user
guest
```

---

&nbsp;
# 💫Chapter 8 — 測試架構設計

## 🧩學習目標

建立可維護的大型測試架構。

&nbsp;
## 🧩章節內容

### 🍀 0801 - 測試分層

---

&nbsp;
### 🍀 0802 - reusable design

---

&nbsp;
### 🍀 0803 - fixtures

---

&nbsp;
### 🍀 0804 - hooks

---

&nbsp;
### 🍀 0805 - test isolation

---

&nbsp;
### 🍀 0806 - test lifecycle

---

&nbsp;
### 🍀 0807 - parallel testing

---

&nbsp;
### 🍀 0808 - flaky test

---

&nbsp;
# 💫Chapter 9 — 測試資料管理

## 🧩學習目標

學會企業級測試資料管理。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 0901 - mock data

---

&nbsp;
### 🍀 0902 - seed data

---

&nbsp;
### 🍀 0903 - test db

---

&nbsp;
### 🍀 0904 - cleanup

---

&nbsp;
### 🍀 0905 - dynamic data

---

&nbsp;
### 🍀 0906 - faker

---

&nbsp;
# 💫Chapter 10 — Page Object Model（POM）

## 🧩學習目標

建立可重用的測試 abstraction。

&nbsp;
## 🧩章節內容

### 🍀 1001 - POM 概念

---

&nbsp;
### 🍀 1002 - page abstraction

---

&nbsp;
### 🍀 1003 - reusable methods

---

&nbsp;
### 🍀 1004 - component object

---

&nbsp;
### 🍀 1005 - anti-pattern

---

&nbsp;
# 💫Chapter 11 — Debug 與 Trace

## 🧩學習目標

提升除錯與分析能力。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 1101 - UI mode

---

&nbsp;
### 🍀 1102 - debug mode

---

&nbsp;
### 🍀 1103 - pause

---

&nbsp;
### 🍀 1104 - trace viewer

---

&nbsp;
### 🍀 1105 - screenshot

---

&nbsp;
### 🍀 1106 - video

---

&nbsp;
### 🍀 1107 - console log

---

&nbsp;
### 🍀 1108 - network inspection

---

&nbsp;
# 💫Chapter 12 — Visual Testing

## 🧩學習目標

學會 UI regression 驗證。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 1201 - screenshot compare

---

&nbsp;
### 🍀 1202 - snapshot testing

---

&nbsp;
### 🍀 1203 - visual regression

---

&nbsp;
### 🍀 1204 - masking dynamic content

---

&nbsp;
# 💫Chapter 13 — Component Testing

## 🧩學習目標

理解 isolated component testing。

&nbsp;
## 🧩章節內容

### 🍀 1301 - isolated component

---

&nbsp;
### 🍀 1302 - mount

---

&nbsp;
### 🍀 1303 - component interaction

---

&nbsp;
### 🍀 1304 - mocking props

---

&nbsp;
# 💫Chapter 14 — CI/CD 整合

## 🧩學習目標

整合企業級 pipeline。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 1401 - GitHub Actions

---

&nbsp;
### 🍀 1402 - GitLab CI

---

&nbsp;
### 🍀 1403 - Docker

---

&nbsp;
### 🍀 1404 - headless CI

---

&nbsp;
### 🍀 1405 - artifact

---

&nbsp;
### 🍀 1406 - report upload

---

&nbsp;
### 🍀 1407 - failed retry

---

&nbsp;
# 💫Chapter 15 — 大型專案測試策略

## 🧩學習目標

建立 senior 等級測試思維。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 1501 - test pyramid

---

&nbsp;
### 🍀 1502 - risk-based testing

---

&nbsp;
### 🍀 1503 - critical flow

---

&nbsp;
### 🍀 1504 - smoke test

---

&nbsp;
### 🍀 1505 - regression test

---

&nbsp;
### 🍀 1506 - release validation

---

&nbsp;
# 💫Chapter 16 — Browser Automation / RPA

&nbsp;
## 🧩學習目標

超越測試，進入 browser automation。

&nbsp;
## 🧩章節內容

### 🍀 1601 - browser scripting

---

&nbsp;
### 🍀 1602 - crawling

---

&nbsp;
### 🍀 1603 - auto operation

---

&nbsp;
### 🍀 1604 - download automation

---

&nbsp;
### 🍀 1605 - upload automation

---

&nbsp;
### 🍀 1606 - report automation

---

&nbsp;
# 💫Chapter 17 — Playwright 進階原理

## 🧩學習目標

理解 Playwright 底層機制。

&nbsp;
## 🧩章節內容

### 🍀 1701 - Chrome DevTools Protocol（CDP）

---

&nbsp;
### 🍀 1702 - auto wait internals

---

&nbsp;
### 🍀 1703 - browser process

---

&nbsp;
### 🍀 1704 - rendering lifecycle

---

&nbsp;
### 🍀 1705 - event loop

---

&nbsp;
### 🍀 1706 - network lifecycle

---

&nbsp;
# 💫Chapter 18 — AI Agent 與 Browser Agent

## 🧩學習目標

理解未來 AI browser automation 趨勢。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 1801 - browser-use

---

&nbsp;
### 🍀 1802 - computer-use

---

&nbsp;
### 🍀 1803 - AI automation

---

&nbsp;
### 🍀 1804 - agentic browser

---

&nbsp;
### 🍀 1805 - LLM + browser control

---

&nbsp;
# 推薦學習順序

```txt
0 → 1 → 2 → 3 → 5 → 6 → 7
→ 8 → 10 → 11 → 14
→ 15 → 16 → 18
```

---

&nbsp;
# 企業前端最重要章節

```txt
2 Locator
3 Assertion
6 Network
7 Auth
8 Architecture
10 POM
11 Debug
14 CI/CD
15 Testing Strategy
```

---

&nbsp;
# 最能拉開資深度的章節

```txt
8 測試架構設計
10 POM
14 CI/CD
15 Testing Strategy
17 Playwright 原理
18 AI Agent
```

&nbsp;