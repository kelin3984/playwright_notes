

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
    - [🍀 0107 - playwright.config.ts 深度設定](#-0107---playwrightconfigts-深度設定)
    - [🍀 0108 - CLI 常用指令](#-0108---cli-常用指令)
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
    - [🍀 1109 - --last-failed / --shard](#-1109-----last-failed----shard)
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
    - [🍀 1607 - PDF 生成](#-1607---pdf-生成)
- [💫Chapter 17 — Playwright 進階原理](#chapter-17--playwright-進階原理)
  - [🧩學習目標](#學習目標-17)
  - [🧩章節內容](#章節內容-17)
    - [🍀 1701 - Chrome DevTools Protocol（CDP）](#-1701---chrome-devtools-protocolcdp)
    - [🍀 1702 - auto wait internals](#-1702---auto-wait-internals)
    - [🍀 1703 - browser process](#-1703---browser-process)
    - [🍀 1704 - rendering lifecycle](#-1704---rendering-lifecycle)
    - [🍀 1705 - event loop](#-1705---event-loop)
    - [🍀 1706 - network lifecycle](#-1706---network-lifecycle)
- [💫Chapter 18 — AI 時代的 Browser Automation 概觀](#chapter-18--ai-時代的-browser-automation-概觀)
  - [🧩學習目標](#學習目標-18)
  - [🧩章節內容](#章節內容-18)
    - [🍀 1801 - LLM + Browser Control 演進趨勢](#-1801---llm--browser-control-演進趨勢)
    - [🍀 1802 - 三種路線比較：MCP vs CLI+Skills vs Agent Loop](#-1802---三種路線比較mcp-vs-cliskills-vs-agent-loop)
    - [🍀 1803 - Accessibility Tree 作為 AI 頁面表示的核心](#-1803---accessibility-tree-作為-ai-頁面表示的核心)
    - [🍀 1804 - Vision mode vs Snapshot mode](#-1804---vision-mode-vs-snapshot-mode)
    - [🍀 1805 - agentic browser 設計模式](#-1805---agentic-browser-設計模式)
- [💫Chapter 19 — Playwright Agents](#chapter-19--playwright-agents)
  - [🧩學習目標](#學習目標-19)
  - [🧩章節內容](#章節內容-19)
    - [🍀 1901 - Agent Loop 模式](#-1901---agent-loop-模式)
    - [🍀 1902 - self-healing test](#-1902---self-healing-test)
    - [🍀 1903 - autonomous test generation](#-1903---autonomous-test-generation)
    - [🍀 1904 - Playwright 作為 Agent 的 browser tool](#-1904---playwright-作為-agent-的-browser-tool)
    - [🍀 1905 - agent tracing 與 debug 策略](#-1905---agent-tracing-與-debug-策略)
    - [🍀 1906 - 多 Agent 協作](#-1906---多-agent-協作)
- [💫Chapter 20 — Playwright MCP Server](#chapter-20--playwright-mcp-server)
  - [🧩學習目標](#學習目標-20)
  - [🧩章節內容](#章節內容-20)
    - [🍀 2001 - 什麼是 MCP 與 @playwright/mcp](#-2001---什麼是-mcp-與-playwrightmcp)
    - [🍀 2002 - 安裝與連接 AI Client](#-2002---安裝與連接-ai-client)
    - [🍀 2003 - Tool 類別與 --caps 選用功能模組](#-2003---tool-類別與---caps-選用功能模組)
    - [🍀 2004 - User Profile 三種模式](#-2004---user-profile-三種模式)
    - [🍀 2005 - --codegen 自動產生測試程式碼](#-2005-----codegen-自動產生測試程式碼)
    - [🍀 2006 - Chrome Extension 連接現有瀏覽器](#-2006---chrome-extension-連接現有瀏覽器)
    - [🍀 2007 - Standalone Server（headless / CI 環境）](#-2007---standalone-serverheadless--ci-環境)
- [💫Chapter 21 — Playwright CLI](#chapter-21--playwright-cli)
  - [🧩學習目標](#學習目標-21)
  - [🧩章節內容](#章節內容-21)
    - [🍀 2101 - @playwright/cli 安裝與定位](#-2101---playwrightcli-安裝與定位)
    - [🍀 2102 - 完整指令集實戰](#-2102---完整指令集實戰)
    - [🍀 2103 - Session 管理（多 session 並行）](#-2103---session-管理多-session-並行)
    - [🍀 2104 - playwright-cli show Monitoring Dashboard](#-2104---playwright-cli-show-monitoring-dashboard)
    - [🍀 2105 - Spec-driven testing 流程](#-2105---spec-driven-testing-流程)
- [💫Chapter 22 — Playwright Skills](#chapter-22--playwright-skills)
  - [🧩學習目標](#學習目標-22)
  - [🧩章節內容](#章節內容-22)
    - [🍀 2201 - Skills 是什麼](#-2201---skills-是什麼)
    - [🍀 2202 - playwright-cli install --skills](#-2202---playwright-cli-install---skills)
    - [🍀 2203 - Skills 目錄與 coding agent 自動發現](#-2203---skills-目錄與-coding-agent-自動發現)
    - [🍀 2204 - 自訂 Skills（撰寫 SKILL.md）](#-2204---自訂-skills撰寫-skillmd)
    - [🍀 2205 - Skills 在 Claude Code / GitHub Copilot 中的實際運作](#-2205---skills-在-claude-code--github-copilot-中的實際運作)
- [💫Chapter 23 — Accessibility Testing（a11y）](#chapter-23--accessibility-testinga11y)
  - [🧩學習目標](#學習目標-23)
  - [🧩章節內容](#章節內容-23)
    - [🍀 2301 - a11y 概念與重要性](#-2301---a11y-概念與重要性)
    - [🍀 2302 - getByRole 與語意選取](#-2302---getbyrole-與語意選取)
    - [🍀 2303 - axe-core 整合](#-2303---axe-core-整合)
    - [🍀 2304 - ARIA 屬性驗證](#-2304---aria-屬性驗證)
    - [🍀 2305 - keyboard navigation 測試](#-2305---keyboard-navigation-測試)
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
18. AI 時代的 Browser Automation 概觀
19. Playwright Agents
20. Playwright MCP Server
21. Playwright CLI
22. Playwright Skills
23. Accessibility Testing（a11y）
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
### 🍀 0107 - playwright.config.ts 深度設定

重點：

```txt
projects（多瀏覽器 / 多環境）
use（baseURL, viewport, locale…）
webServer（自動啟動前端 dev server）
reporter（html, json, github…）
timeout / retries
```

---

&nbsp;
### 🍀 0108 - CLI 常用指令

```bash
# 執行指定檔案
npx playwright test tests/login.spec.ts

# 只跑上次失敗的測試
npx playwright test --last-failed

# 分片執行（CI 平行化）
npx playwright test --shard=1/3

# 指定 project
npx playwright test --project=chromium
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
### 🍀 1109 - --last-failed / --shard

```bash
# 只重跑上次失敗
npx playwright test --last-failed

# CI 分片（把測試切成 N 份平行跑）
npx playwright test --shard=1/4
npx playwright test --shard=2/4
```

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
### 🍀 1607 - PDF 生成

```ts
await page.pdf({
  path: 'report.pdf',
  format: 'A4',
  printBackground: true,
})
```

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
# 💫Chapter 18 — AI 時代的 Browser Automation 概觀

## 🧩學習目標

建立 AI browser automation 的心智模型，理解各工具的定位與選擇依據。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 1801 - LLM + Browser Control 演進趨勢

理解從傳統 script → RPA → AI-driven automation 的演進脈絡：

```txt
手寫 Playwright script
→ Codegen 輔助錄製
→ LLM 生成測試
→ Agent 自主操控瀏覽器
```

---

&nbsp;
### 🍀 1802 - 三種路線比較：MCP vs CLI+Skills vs Agent Loop

| 路線 | 代表工具 | 適用情境 |
|---|---|---|
| MCP Server | `@playwright/mcp` | AI Client 長期操控瀏覽器 |
| CLI + Skills | `@playwright/cli` | Coding agent 呼叫瀏覽器指令 |
| Agent Loop | agentic browser | LLM 自主推理、行動、觀察循環 |

---

&nbsp;
### 🍀 1803 - Accessibility Tree 作為 AI 頁面表示的核心

理解為什麼 AI 操控瀏覽器不依賴截圖，而是使用 Accessibility Tree（結構化快照）：

```txt
截圖 → 需要視覺模型（高 token、高延遲）
Accessibility Tree → 結構化文字（低 token、高精度）
```

---

&nbsp;
### 🍀 1804 - Vision mode vs Snapshot mode

```txt
Snapshot mode（預設）：使用 Accessibility Tree
Vision mode（--caps=vision）：使用座標點擊，需視覺模型
```

---

&nbsp;
### 🍀 1805 - agentic browser 設計模式

理解：

```txt
Observe（快照頁面）
→ Plan（LLM 決策下一步）
→ Act（執行瀏覽器操作）
→ Verify（確認結果）
→ 循環
```

---

&nbsp;
# 💫Chapter 19 — Playwright Agents

## 🧩學習目標

理解 Agent Loop 的本質與如何用 Playwright 建立能自主運作的瀏覽器 agent。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 1901 - Agent Loop 模式

理解 agent 的推理行動循環：

```txt
Observe → Plan → Act → Verify → Loop
```

---

&nbsp;
### 🍀 1902 - self-healing test

理解測試自動修復：當 locator 失效時，agent 自動偵測並修復 selector。

---

&nbsp;
### 🍀 1903 - autonomous test generation

理解從使用者描述（自然語言）→ agent 自主生成可執行測試的流程。

---

&nbsp;
### 🍀 1904 - Playwright 作為 Agent 的 browser tool

理解 Playwright 在 agent 架構中的角色：

```txt
LLM（大腦）
↓
Playwright（手）：執行 click / fill / snapshot…
```

---

&nbsp;
### 🍀 1905 - agent tracing 與 debug 策略

使用 Playwright Trace Viewer 分析 agent 每一步的行為，定位失敗原因。

---

&nbsp;
### 🍀 1906 - 多 Agent 協作

理解 orchestrator agent + sub-agent 分工模式，多個 browser session 並行處理任務。

---

&nbsp;
# 💫Chapter 20 — Playwright MCP Server

## 🧩學習目標

學會透過 Model Context Protocol 讓 AI Client 直接操控瀏覽器。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 2001 - 什麼是 MCP 與 @playwright/mcp

```bash
npx @playwright/mcp@latest
```

透過 MCP 協議暴露瀏覽器工具給 AI Client（Claude Desktop、VS Code、Cursor 等）。

---

&nbsp;
### 🍀 2002 - 安裝與連接 AI Client

支援的 AI Client：

```txt
Claude Desktop
VS Code（GitHub Copilot）
Cursor
Windsurf
Goose
```

---

&nbsp;
### 🍀 2003 - Tool 類別與 --caps 選用功能模組

```txt
核心（預設）：click、fill、hover、snapshot、tab 管理…
--caps=network：網路攔截
--caps=storage：cookies、localStorage
--caps=devtools：console log、requests
--caps=vision：座標點擊（Vision mode）
--caps=pdf：匯出 PDF
--caps=testing：測試斷言工具
--caps=codegen：產生測試程式碼
```

---

&nbsp;
### 🍀 2004 - User Profile 三種模式

```txt
Persistent（預設）：儲存在本機 profile
Isolated（--isolated）：每次會話獨立，關閉即清除
Browser Extension（--extension）：連接現有已登入的 Chrome
```

---

&nbsp;
### 🍀 2005 - --codegen 自動產生測試程式碼

```bash
npx @playwright/mcp@latest --caps=codegen
```

AI 操作瀏覽器的同時，自動產生對應的 Playwright TypeScript 測試程式碼。

---

&nbsp;
### 🍀 2006 - Chrome Extension 連接現有瀏覽器

```bash
npx @playwright/mcp@latest --extension
```

連接已登入的 Chrome 分頁，無需重新驗證。

---

&nbsp;
### 🍀 2007 - Standalone Server（headless / CI 環境）

```bash
npx @playwright/mcp@latest --headless --port 3000
```

---

&nbsp;
# 💫Chapter 21 — Playwright CLI

## 🧩學習目標

學會使用 `@playwright/cli` 指令集，讓 coding agent 高效呼叫瀏覽器操作。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 2101 - @playwright/cli 安裝與定位

```bash
npm install -g @playwright/cli@latest
```

與 MCP 的核心差異：

```txt
CLI + Skills：Token 高效，不載入 schema，適合大型程式碼庫
MCP：Rich introspection，適合持久狀態、長期自主流程
```

---

&nbsp;
### 🍀 2102 - 完整指令集實戰

核心操作：

```bash
playwright-cli goto https://example.com
playwright-cli click [ref]
playwright-cli fill [ref] "text"
playwright-cli snapshot          # 取得 Accessibility Tree
playwright-cli screenshot
playwright-cli eval "document.title"
```

---

&nbsp;
### 🍀 2103 - Session 管理（多 session 並行）

```bash
playwright-cli -s=admin goto https://app.example.com/admin
playwright-cli -s=user  goto https://app.example.com/user

playwright-cli list        # 列出所有 session
playwright-cli close-all   # 關閉全部
```

---

&nbsp;
### 🍀 2104 - playwright-cli show Monitoring Dashboard

```bash
playwright-cli show
```

即時 Session Grid：所有 session 的縮圖、URL、頁標，支援遠端接管滑鼠鍵盤。

---

&nbsp;
### 🍀 2105 - Spec-driven testing 流程

```txt
1. plan   → 根據需求描述規劃測試案例
2. generate → 自動產生 .spec.ts
3. heal   → 測試失敗時自動修復 locator
```

---

&nbsp;
# 💫Chapter 22 — Playwright Skills

## 🧩學習目標

理解 Skills 機制，讓 coding agent 自動發現並使用瀏覽器工具。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 2201 - Skills 是什麼

Skills 是 Markdown 格式的「任務說明書」，放置在 coding agent 可讀取的目錄（如 `.claude/skills/`）中，告訴 agent 有哪些指令可用、如何使用。

```txt
Skills ≠ 工具本身
Skills = 教 agent 怎麼用工具的說明文件
```

---

&nbsp;
### 🍀 2202 - playwright-cli install --skills

```bash
playwright-cli install --skills
```

自動將 `SKILL.md` 寫入 agent 可讀取的目錄。

---

&nbsp;
### 🍀 2203 - Skills 目錄與 coding agent 自動發現

```txt
~/.claude/skills/playwright/SKILL.md   ← Claude Code 全域讀取
.claude/skills/playwright/SKILL.md     ← 專案層級讀取
```

Agent 啟動時自動掃描並載入，無需手動告知。

---

&nbsp;
### 🍀 2204 - 自訂 Skills（撰寫 SKILL.md）

可自行撰寫 `SKILL.md`，定義專案特有的自動化任務說明，讓 agent 了解自訂流程。

---

&nbsp;
### 🍀 2205 - Skills 在 Claude Code / GitHub Copilot 中的實際運作

理解 agent 讀取 Skills 後的行為：

```txt
agent 讀取 SKILL.md
→ 了解可呼叫的 CLI 指令
→ 自行決定何時呼叫、呼叫哪個指令
→ 執行任務
```

---

&nbsp;
# 💫Chapter 23 — Accessibility Testing（a11y）

## 🧩學習目標

掌握無障礙測試，提升產品品質與合規性。

&nbsp;
## 🧩章節內容

### 🍀 2301 - a11y 概念與重要性

理解：

```txt
WCAG 標準
ARIA roles
為什麼 a11y 測試能提升測試品質
```

---

&nbsp;
### 🍀 2302 - getByRole 與語意選取

理解 `getByRole` 本質就是 a11y 導向的選取策略。

---

&nbsp;
### 🍀 2303 - axe-core 整合

```ts
import AxeBuilder from '@axe-core/playwright'

const results = await new AxeBuilder({ page }).analyze()
expect(results.violations).toEqual([])
```

---

&nbsp;
### 🍀 2304 - ARIA 屬性驗證

```ts
await expect(locator).toHaveAttribute('aria-label', '關閉')
await expect(locator).toHaveAttribute('aria-expanded', 'true')
```

---

&nbsp;
### 🍀 2305 - keyboard navigation 測試

```ts
await page.keyboard.press('Tab')
await page.keyboard.press('Enter')
```

---

&nbsp;
# 推薦學習順序

```txt
0 → 1 → 2 → 3 → 5 → 6 → 7
→ 8 → 10 → 11 → 14
→ 15 → 16 → 18 → 19 → 20 → 21 → 22 → 23
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
20 Playwright MCP
21 Playwright CLI
23 Accessibility
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
18 AI 時代概觀
19 Playwright Agents
20 Playwright MCP
21 Playwright CLI
22 Playwright Skills
23 Accessibility
```

&nbsp;