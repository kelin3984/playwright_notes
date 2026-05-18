

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
    - [🍀 測試種類](#-測試種類)
    - [🍀 E2E 測試觀念](#-e2e-測試觀念)
    - [🍀 Playwright 核心架構](#-playwright-核心架構)
    - [🍀 Playwright vs Cypress vs Selenium](#-playwright-vs-cypress-vs-selenium)
- [💫Chapter 1 — Playwright 基本操作](#chapter-1--playwright-基本操作)
  - [🧩學習目標](#學習目標-1)
  - [🧩章節內容](#章節內容-1)
    - [🍀 安裝](#-安裝)
    - [🍀 第一個測試](#-第一個測試)
    - [🍀 啟動瀏覽器](#-啟動瀏覽器)
    - [🍀 headed vs headless](#-headed-vs-headless)
    - [🍀 Playwright Test Runner](#-playwright-test-runner)
    - [🍀 專案結構](#-專案結構)
- [💫Chapter 2 — Locator 與 DOM 操作](#chapter-2--locator-與-dom-操作)
  - [🧩學習目標](#學習目標-2)
  - [🧩章節內容](#章節內容-2)
    - [🍀 Locator 概念](#-locator-概念)
    - [🍀 getByRole](#-getbyrole)
    - [🍀 getByText](#-getbytext)
    - [🍀 getByLabel](#-getbylabel)
    - [🍀 getByPlaceholder](#-getbyplaceholder)
    - [🍀 getByTestId](#-getbytestid)
    - [🍀 locator chaining](#-locator-chaining)
    - [🍀 strict mode](#-strict-mode)
    - [🍀 Shadow DOM](#-shadow-dom)
    - [🍀 iframe](#-iframe)
    - [🍀 最佳 selector 策略](#-最佳-selector-策略)
- [💫Chapter 3 — Assertion 與驗證](#chapter-3--assertion-與驗證)
  - [🧩學習目標](#學習目標-3)
  - [🧩章節內容](#章節內容-3)
    - [🍀 expect](#-expect)
    - [🍀 toBeVisible](#-tobevisible)
    - [🍀 toContainText](#-tocontaintext)
    - [🍀 toHaveValue](#-tohavevalue)
    - [🍀 toHaveURL](#-tohaveurl)
    - [🍀 auto retry](#-auto-retry)
    - [🍀 軟斷言 vs 硬斷言](#-軟斷言-vs-硬斷言)
- [💫Chapter 4 — Browser 與 Context 機制](#chapter-4--browser-與-context-機制)
  - [🧩學習目標](#學習目標-4)
  - [🧩章節內容](#章節內容-4)
    - [🍀 Browser](#-browser)
    - [🍀 BrowserContext](#-browsercontext)
    - [🍀 Cookie](#-cookie)
    - [🍀 LocalStorage](#-localstorage)
    - [🍀 SessionStorage](#-sessionstorage)
    - [🍀 popup](#-popup)
    - [🍀 多分頁](#-多分頁)
- [💫Chapter 5 — 表單與 UI 自動化](#chapter-5--表單與-ui-自動化)
  - [🧩學習目標](#學習目標-5)
  - [🧩章節內容](#章節內容-5)
    - [🍀 click](#-click)
    - [🍀 fill](#-fill)
    - [🍀 keyboard](#-keyboard)
    - [🍀 select](#-select)
    - [🍀 upload](#-upload)
    - [🍀 drag and drop](#-drag-and-drop)
    - [🍀 hover](#-hover)
    - [🍀 dialog](#-dialog)
    - [🍀 clipboard](#-clipboard)
    - [🍀 file chooser](#-file-chooser)
- [💫Chapter 6 — Network 與 API 控制](#chapter-6--network-與-api-控制)
  - [🧩學習目標](#學習目標-6)
  - [🧩章節內容](#章節內容-6)
    - [🍀 waitForResponse](#-waitforresponse)
    - [🍀 route interception](#-route-interception)
    - [🍀 mock API](#-mock-api)
    - [🍀 modify response](#-modify-response)
    - [🍀 fake backend](#-fake-backend)
    - [🍀 network idle](#-network-idle)
    - [🍀 GraphQL testing](#-graphql-testing)
    - [🍀 WebSocket](#-websocket)
- [💫Chapter 7 — Authentication 與 Session](#chapter-7--authentication-與-session)
  - [🧩學習目標](#學習目標-7)
  - [🧩章節內容](#章節內容-7)
    - [🍀 login automation](#-login-automation)
    - [🍀 storageState](#-storagestate)
    - [🍀 token persistence](#-token-persistence)
    - [🍀 cookie auth](#-cookie-auth)
    - [🍀 JWT auth](#-jwt-auth)
    - [🍀 OAuth flow](#-oauth-flow)
    - [🍀 SSO flow](#-sso-flow)
    - [🍀 多角色測試](#-多角色測試)
- [💫Chapter 8 — 測試架構設計](#chapter-8--測試架構設計)
  - [🧩學習目標](#學習目標-8)
  - [🧩章節內容](#章節內容-8)
    - [🍀 測試分層](#-測試分層)
    - [🍀 reusable design](#-reusable-design)
    - [🍀 fixtures](#-fixtures)
    - [🍀 hooks](#-hooks)
    - [🍀 test isolation](#-test-isolation)
    - [🍀 test lifecycle](#-test-lifecycle)
    - [🍀 parallel testing](#-parallel-testing)
    - [🍀 flaky test](#-flaky-test)
- [💫Chapter 9 — 測試資料管理](#chapter-9--測試資料管理)
  - [🧩學習目標](#學習目標-9)
  - [🧩章節內容](#章節內容-9)
    - [🍀 mock data](#-mock-data)
    - [🍀 seed data](#-seed-data)
    - [🍀 test db](#-test-db)
    - [🍀 cleanup](#-cleanup)
    - [🍀 dynamic data](#-dynamic-data)
    - [🍀 faker](#-faker)
- [💫Chapter 10 — Page Object Model（POM）](#chapter-10--page-object-modelpom)
  - [🧩學習目標](#學習目標-10)
  - [🧩章節內容](#章節內容-10)
    - [🍀 POM 概念](#-pom-概念)
    - [🍀 page abstraction](#-page-abstraction)
    - [🍀 reusable methods](#-reusable-methods)
    - [🍀 component object](#-component-object)
    - [🍀 anti-pattern](#-anti-pattern)
- [💫Chapter 11 — Debug 與 Trace](#chapter-11--debug-與-trace)
  - [🧩學習目標](#學習目標-11)
  - [🧩章節內容](#章節內容-11)
    - [🍀 UI mode](#-ui-mode)
    - [🍀 debug mode](#-debug-mode)
    - [🍀 pause](#-pause)
    - [🍀 trace viewer](#-trace-viewer)
    - [🍀 screenshot](#-screenshot)
    - [🍀 video](#-video)
    - [🍀 console log](#-console-log)
    - [🍀 network inspection](#-network-inspection)
- [💫Chapter 12 — Visual Testing](#chapter-12--visual-testing)
  - [🧩學習目標](#學習目標-12)
  - [🧩章節內容](#章節內容-12)
    - [🍀 screenshot compare](#-screenshot-compare)
    - [🍀 snapshot testing](#-snapshot-testing)
    - [🍀 visual regression](#-visual-regression)
    - [🍀 masking dynamic content](#-masking-dynamic-content)
- [💫Chapter 13 — Component Testing](#chapter-13--component-testing)
  - [🧩學習目標](#學習目標-13)
  - [🧩章節內容](#章節內容-13)
    - [🍀 isolated component](#-isolated-component)
    - [🍀 mount](#-mount)
    - [🍀 component interaction](#-component-interaction)
    - [🍀 mocking props](#-mocking-props)
- [💫Chapter 14 — CI/CD 整合](#chapter-14--cicd-整合)
  - [🧩學習目標](#學習目標-14)
  - [🧩章節內容](#章節內容-14)
    - [🍀 GitHub Actions](#-github-actions)
    - [🍀 GitLab CI](#-gitlab-ci)
    - [🍀 Docker](#-docker)
    - [🍀 headless CI](#-headless-ci)
    - [🍀 artifact](#-artifact)
    - [🍀 report upload](#-report-upload)
    - [🍀 failed retry](#-failed-retry)
- [💫Chapter 15 — 大型專案測試策略](#chapter-15--大型專案測試策略)
  - [🧩學習目標](#學習目標-15)
  - [🧩章節內容](#章節內容-15)
    - [🍀 test pyramid](#-test-pyramid)
    - [🍀 risk-based testing](#-risk-based-testing)
    - [🍀 critical flow](#-critical-flow)
    - [🍀 smoke test](#-smoke-test)
    - [🍀 regression test](#-regression-test)
    - [🍀 release validation](#-release-validation)
- [💫Chapter 16 — Browser Automation / RPA](#chapter-16--browser-automation--rpa)
  - [🧩學習目標](#學習目標-16)
  - [🧩章節內容](#章節內容-16)
    - [🍀 browser scripting](#-browser-scripting)
    - [🍀 crawling](#-crawling)
    - [🍀 auto operation](#-auto-operation)
    - [🍀 download automation](#-download-automation)
    - [🍀 upload automation](#-upload-automation)
    - [🍀 report automation](#-report-automation)
- [💫Chapter 17 — Playwright 進階原理](#chapter-17--playwright-進階原理)
  - [🧩學習目標](#學習目標-17)
  - [🧩章節內容](#章節內容-17)
    - [🍀 Chrome DevTools Protocol（CDP）](#-chrome-devtools-protocolcdp)
    - [🍀 auto wait internals](#-auto-wait-internals)
    - [🍀 browser process](#-browser-process)
    - [🍀 rendering lifecycle](#-rendering-lifecycle)
    - [🍀 event loop](#-event-loop)
    - [🍀 network lifecycle](#-network-lifecycle)
- [💫Chapter 18 — AI Agent 與 Browser Agent](#chapter-18--ai-agent-與-browser-agent)
  - [🧩學習目標](#學習目標-18)
  - [🧩章節內容](#章節內容-18)
    - [🍀 browser-use](#-browser-use)
    - [🍀 computer-use](#-computer-use)
    - [🍀 AI automation](#-ai-automation)
    - [🍀 agentic browser](#-agentic-browser)
    - [🍀 LLM + browser control](#-llm--browser-control)
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

### 🍀 測試種類

- Unit Test
- Integration Test
- E2E Test
- Component Test

---
&nbsp;
### 🍀 E2E 測試觀念

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
### 🍀 Playwright 核心架構

```txt
Test Runner
Browser
Context
Page
Locator
```

---
&nbsp;
### 🍀 Playwright vs Cypress vs Selenium

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

### 🍀 安裝

```bash
npm init playwright@latest
```

---

&nbsp;
### 🍀 第一個測試

```ts
test('homepage', async ({ page }) => {}
```

---

&nbsp;
### 🍀 啟動瀏覽器

```ts
browser.newContext()
context.newPage()
```

---

&nbsp;
### 🍀 headed vs headless

---

&nbsp;
### 🍀 Playwright Test Runner

---

&nbsp;
### 🍀 專案結構

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

### 🍀 Locator 概念

理解：

```txt
Locator != querySelector
```

---

&nbsp;
### 🍀 getByRole

---

&nbsp;
### 🍀 getByText

---

&nbsp;
### 🍀 getByLabel

---

&nbsp;
### 🍀 getByPlaceholder

---

&nbsp;
### 🍀 getByTestId

---

&nbsp;
### 🍀 locator chaining

---

&nbsp;
### 🍀 strict mode

---

&nbsp;
### 🍀 Shadow DOM

---

&nbsp;
### 🍀 iframe

---

&nbsp;
### 🍀 最佳 selector 策略

---

&nbsp;
# 💫Chapter 3 — Assertion 與驗證

## 🧩學習目標

學會真正「驗證」頁面狀態。

&nbsp;
## 🧩章節內容

### 🍀 expect

---

&nbsp;
### 🍀 toBeVisible

---

&nbsp;
### 🍀 toContainText

---

&nbsp;
### 🍀 toHaveValue

---

&nbsp;
### 🍀 toHaveURL

---

&nbsp;
### 🍀 auto retry

---

&nbsp;
### 🍀 軟斷言 vs 硬斷言

---

&nbsp;
# 💫Chapter 4 — Browser 與 Context 機制

## 🧩學習目標

理解瀏覽器 automation 本質。

&nbsp;
## 🧩章節內容

### 🍀 Browser

---

&nbsp;
### 🍀 BrowserContext

```txt
Context ≈ 無痕視窗
```

---

&nbsp;
### 🍀 Cookie

---

&nbsp;
### 🍀 LocalStorage

---

&nbsp;
### 🍀 SessionStorage

---

&nbsp;
### 🍀 popup

---

&nbsp;
### 🍀 多分頁

---

&nbsp;
# 💫Chapter 5 — 表單與 UI 自動化

## 🧩學習目標

模擬真實使用者操作。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 click

---

&nbsp;
### 🍀 fill

---

&nbsp;
### 🍀 keyboard

---

&nbsp;
### 🍀 select

---

&nbsp;
### 🍀 upload

---

&nbsp;
### 🍀 drag and drop

---

&nbsp;
### 🍀 hover

---

&nbsp;
### 🍀 dialog

---

&nbsp;
### 🍀 clipboard

---

&nbsp;
### 🍀 file chooser

---

&nbsp;
# 💫Chapter 6 — Network 與 API 控制

## 🧩學習目標

學會控制與攔截 network。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 waitForResponse

---

&nbsp;
### 🍀 route interception

---

&nbsp;
### 🍀 mock API

---

&nbsp;
### 🍀 modify response

---

&nbsp;
### 🍀 fake backend

---

&nbsp;
### 🍀 network idle

---

&nbsp;
### 🍀 GraphQL testing

---

&nbsp;
### 🍀 WebSocket

---

&nbsp;
# 💫Chapter 7 — Authentication 與 Session

## 🧩學習目標

處理企業系統常見登入流程。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 login automation

---

&nbsp;
### 🍀 storageState

---

&nbsp;
### 🍀 token persistence

---

&nbsp;
### 🍀 cookie auth

---

&nbsp;
### 🍀 JWT auth

---

&nbsp;
### 🍀 OAuth flow

---

&nbsp;
### 🍀 SSO flow

---

&nbsp;
### 🍀 多角色測試

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

### 🍀 測試分層

---

&nbsp;
### 🍀 reusable design

---

&nbsp;
### 🍀 fixtures

---

&nbsp;
### 🍀 hooks

---

&nbsp;
### 🍀 test isolation

---

&nbsp;
### 🍀 test lifecycle

---

&nbsp;
### 🍀 parallel testing

---

&nbsp;
### 🍀 flaky test

---

&nbsp;
# 💫Chapter 9 — 測試資料管理

## 🧩學習目標

學會企業級測試資料管理。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 mock data

---

&nbsp;
### 🍀 seed data

---

&nbsp;
### 🍀 test db

---

&nbsp;
### 🍀 cleanup

---

&nbsp;
### 🍀 dynamic data

---

&nbsp;
### 🍀 faker

---

&nbsp;
# 💫Chapter 10 — Page Object Model（POM）

## 🧩學習目標

建立可重用的測試 abstraction。

&nbsp;
## 🧩章節內容

### 🍀 POM 概念

---

&nbsp;
### 🍀 page abstraction

---

&nbsp;
### 🍀 reusable methods

---

&nbsp;
### 🍀 component object

---

&nbsp;
### 🍀 anti-pattern

---

&nbsp;
# 💫Chapter 11 — Debug 與 Trace

## 🧩學習目標

提升除錯與分析能力。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 UI mode

---

&nbsp;
### 🍀 debug mode

---

&nbsp;
### 🍀 pause

---

&nbsp;
### 🍀 trace viewer

---

&nbsp;
### 🍀 screenshot

---

&nbsp;
### 🍀 video

---

&nbsp;
### 🍀 console log

---

&nbsp;
### 🍀 network inspection

---

&nbsp;
# 💫Chapter 12 — Visual Testing

## 🧩學習目標

學會 UI regression 驗證。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 screenshot compare

---

&nbsp;
### 🍀 snapshot testing

---

&nbsp;
### 🍀 visual regression

---

&nbsp;
### 🍀 masking dynamic content

---

&nbsp;
# 💫Chapter 13 — Component Testing

## 🧩學習目標

理解 isolated component testing。

&nbsp;
## 🧩章節內容

### 🍀 isolated component

---

&nbsp;
### 🍀 mount

---

&nbsp;
### 🍀 component interaction

---

&nbsp;
### 🍀 mocking props

---

&nbsp;
# 💫Chapter 14 — CI/CD 整合

## 🧩學習目標

整合企業級 pipeline。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 GitHub Actions

---

&nbsp;
### 🍀 GitLab CI

---

&nbsp;
### 🍀 Docker

---

&nbsp;
### 🍀 headless CI

---

&nbsp;
### 🍀 artifact

---

&nbsp;
### 🍀 report upload

---

&nbsp;
### 🍀 failed retry

---

&nbsp;
# 💫Chapter 15 — 大型專案測試策略

## 🧩學習目標

建立 senior 等級測試思維。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 test pyramid

---

&nbsp;
### 🍀 risk-based testing

---

&nbsp;
### 🍀 critical flow

---

&nbsp;
### 🍀 smoke test

---

&nbsp;
### 🍀 regression test

---

&nbsp;
### 🍀 release validation

---

&nbsp;
# 💫Chapter 16 — Browser Automation / RPA

&nbsp;
## 🧩學習目標

超越測試，進入 browser automation。

&nbsp;
## 🧩章節內容

### 🍀 browser scripting

---

&nbsp;
### 🍀 crawling

---

&nbsp;
### 🍀 auto operation

---

&nbsp;
### 🍀 download automation

---

&nbsp;
### 🍀 upload automation

---

&nbsp;
### 🍀 report automation

---

&nbsp;
# 💫Chapter 17 — Playwright 進階原理

## 🧩學習目標

理解 Playwright 底層機制。

&nbsp;
## 🧩章節內容

### 🍀 Chrome DevTools Protocol（CDP）

---

&nbsp;
### 🍀 auto wait internals

---

&nbsp;
### 🍀 browser process

---

&nbsp;
### 🍀 rendering lifecycle

---

&nbsp;
### 🍀 event loop

---

&nbsp;
### 🍀 network lifecycle

---

&nbsp;
# 💫Chapter 18 — AI Agent 與 Browser Agent

## 🧩學習目標

理解未來 AI browser automation 趨勢。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 browser-use

---

&nbsp;
### 🍀 computer-use

---

&nbsp;
### 🍀 AI automation

---

&nbsp;
### 🍀 agentic browser

---

&nbsp;
### 🍀 LLM + browser control

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