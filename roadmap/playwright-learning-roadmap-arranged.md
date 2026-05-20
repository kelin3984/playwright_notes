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
      - [等待指定 API 回應完成](#等待指定-api-回應完成)
    - [🍀 0602 - route interception](#-0602---route-interception)
      - [攔截、放行、阻擋與改寫 request](#攔截放行阻擋與改寫-request)
    - [🍀 0603 - mock API](#-0603---mock-api)
      - [建立可重現的前端測試情境](#建立可重現的前端測試情境)
    - [🍀 0604 - modify response](#-0604---modify-response)
      - [保留真後端流程，只局部改寫回應](#保留真後端流程只局部改寫回應)
    - [🍀 0605 - fake backend](#-0605---fake-backend)
      - [用有狀態的假後端支撐前端流程](#用有狀態的假後端支撐前端流程)
    - [🍀 0606 - network idle](#-0606---network-idle)
      - [理解 `networkidle` 的適用時機與限制](#理解-networkidle-的適用時機與限制)
    - [🍀 0607 - GraphQL testing](#-0607---graphql-testing)
      - [用 `operationName` 區分 GraphQL 操作](#用-operationname-區分-graphql-操作)
    - [🍀 0608 - WebSocket](#-0608---websocket)
      - [觀察即時事件與驗證 UI 回應](#觀察即時事件與驗證-ui-回應)
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
    - [🍀 1002 - page abstraction with class and functional](#-1002---page-abstraction-with-class-and-functional)
    - [🍀 1003 - reusable methods](#-1003---reusable-methods)
    - [🍀 1004 - component object](#-1004---component-object)
    - [🍀 1005 - boundaries and anti-pattern](#-1005---boundaries-and-anti-pattern)
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
    - [🍀 1802 - 三種官方路線比較：MCP vs CLI+Skills vs Test Agents](#-1802---三種官方路線比較mcp-vs-cliskills-vs-test-agents)
    - [🍀 1803 - Accessibility Tree 作為 AI 頁面表示的核心](#-1803---accessibility-tree-作為-ai-頁面表示的核心)
    - [🍀 1804 - Snapshot mode vs Vision mode：優勢與限制](#-1804---snapshot-mode-vs-vision-mode優勢與限制)
    - [🍀 1805 - agentic browser 共通流程與適用邊界](#-1805---agentic-browser-共通流程與適用邊界)
- [💫Chapter 19 — Playwright MCP Server](#chapter-19--playwright-mcp-server)
  - [🧩學習目標](#學習目標-19)
  - [🧩章節內容](#章節內容-19)
    - [🍀 1901 - 什麼是 MCP 與 @playwright/mcp](#-1901---什麼是-mcp-與-playwrightmcp)
    - [🍀 1902 - 連接 AI Client 與使用情境](#-1902---連接-ai-client-與使用情境)
    - [🍀 1903 - Tool 類別、caps 與版本敏感設定](#-1903---tool-類別caps-與版本敏感設定)
    - [🍀 1904 - User Profile、Extension 與 Headless 模式](#-1904---user-profileextension-與-headless-模式)
    - [🍀 1905 - 安全邊界與風險控管](#-1905---安全邊界與風險控管)
    - [🍀 1906 - codegen、trace 與 CI 實務](#-1906---codegentrace-與-ci-實務)
- [💫Chapter 20 — Playwright CLI](#chapter-20--playwright-cli)
  - [🧩學習目標](#學習目標-20)
  - [🧩章節內容](#章節內容-20)
    - [🍀 2001 - @playwright/cli 安裝與定位](#-2001---playwrightcli-安裝與定位)
    - [🍀 2002 - open / attach 與 session 基本模型](#-2002---open--attach-與-session-基本模型)
    - [🍀 2003 - 常用命令：goto、click、fill、snapshot](#-2003---常用命令gotoclickfillsnapshot)
    - [🍀 2004 - 多 session 管理與 Monitoring Dashboard](#-2004---多-session-管理與-monitoring-dashboard)
    - [🍀 2005 - CLI 適合的工作型態與限制](#-2005---cli-適合的工作型態與限制)
- [💫Chapter 21 — Playwright Skills](#chapter-21--playwright-skills)
  - [🧩學習目標](#學習目標-21)
  - [🧩章節內容](#章節內容-21)
    - [🍀 2101 - Skills 是什麼](#-2101---skills-是什麼)
    - [🍀 2102 - playwright-cli install --skills](#-2102---playwright-cli-install---skills)
    - [🍀 2103 - Skills、Instructions 與 Agent 發現機制](#-2103---skillsinstructions-與-agent-發現機制)
    - [🍀 2104 - 自訂 SKILL.md 的設計原則](#-2104---自訂-skillmd-的設計原則)
    - [🍀 2105 - Claude Code / GitHub Copilot / 其他 Client 差異](#-2105---claude-code--github-copilot--其他-client-差異)
- [💫Chapter 22 — Playwright Test Agents](#chapter-22--playwright-test-agents)
  - [🧩學習目標](#學習目標-22)
  - [🧩章節內容](#章節內容-22)
    - [🍀 2201 - 什麼是 Playwright Test Agents](#-2201---什麼是-playwright-test-agents)
    - [🍀 2202 - planner / generator / healer 的角色](#-2202---planner--generator--healer-的角色)
    - [🍀 2203 - Spec-driven workflow：seed.spec.ts → specs → tests](#-2203---spec-driven-workflowseedspects--specs--tests)
    - [🍀 2204 - self-healing 與 human review 邊界](#-2204---self-healing-與-human-review-邊界)
    - [🍀 2205 - tracing、debug 與 observability](#-2205---tracingdebug-與-observability)
    - [🍀 2206 - 適用情境、限制與導入策略](#-2206---適用情境限制與導入策略)
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
19. Playwright MCP Server
20. Playwright CLI
21. Playwright Skills
22. Playwright Test Agents
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
test('homepage', async ({ page }) => {})
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

#### 等待指定 API 回應完成

適合搭配點擊、送出表單、切換篩選條件等操作，精準等待特定 API 回來，而不是盲目使用固定秒數。

---

&nbsp;
### 🍀 0602 - route interception

#### 攔截、放行、阻擋與改寫 request

理解 `route` 如何在 request 發送前介入，並區分 `continue`、`abort`、`fulfill` 三種常見用途。

---

&nbsp;
### 🍀 0603 - mock API

#### 建立可重現的前端測試情境

用固定回應重現空資料、錯誤狀態、特殊邊界資料，降低測試對真後端與 seed data 的依賴。

---

&nbsp;
### 🍀 0604 - modify response

#### 保留真後端流程，只局部改寫回應

先取得真實 response，再針對局部欄位做 patch，適合測試 feature flag、折扣、權限與缺貨狀態。

---

&nbsp;
### 🍀 0605 - fake backend

#### 用有狀態的假後端支撐前端流程

不只 mock 單一回應，而是用記憶體資料模擬 list / detail / create / delete 等完整互動流程。

---

&nbsp;
### 🍀 0606 - network idle

#### 理解 `networkidle` 的適用時機與限制

知道它不是萬用等待條件，特別是在 SPA、輪詢、analytics、WebSocket 場景中要避免濫用。

---

&nbsp;
### 🍀 0607 - GraphQL testing

#### 用 `operationName` 區分 GraphQL 操作

因為多數 GraphQL 請求共用同一支 endpoint，所以等待與 mock 時要讀 request body，而不只看 URL。

---

&nbsp;
### 🍀 0608 - WebSocket

#### 觀察即時事件與驗證 UI 回應

掌握如何監聽 socket 建立、frame 收發與關閉事件，並把重點放在使用者真正看得到的即時 UI 結果。

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

理解如何區分：

```txt
UI / E2E
API
Auth / Session
共用支援層（pages / fixtures / data / helpers）
```

---

&nbsp;
### 🍀 0802 - reusable design

包含常見資料夾切法：

```txt
tests/ 依功能切
pages/ 放頁面抽象
fixtures/ 放測試注入
data/ 放測試資料
helpers/ 放共用工具
```

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

建立可維護、可重組的頁面抽象，並理解 POM 不等於 class-only。

&nbsp;
## 🧩章節內容

### 🍀 1001 - POM 概念

理解：

```txt
POM = 用頁面或元件物件封裝 locator 與操作流程
目的不是炫技抽象，而是降低 selector 外洩、提升維護性
POM != 只能用 class
POM != 把所有測試邏輯都塞進 page file
```

重點先建立心智模型：

- spec 負責描述案例與驗證結果
- POM 負責提供頁面操作 API
- locator 集中管理，避免測試檔四處散落 selector

---

&nbsp;
### 🍀 1002 - page abstraction with class and functional

同一個 POM 模式，可以有兩種常見表達方式：

```txt
class style
- 適合大型專案、官方文件風格、搭配 fixture 注入時較直觀

functional style
- 適合現代前端常見寫法
- 用 factory function 回傳 page API
```

本章不把 POM 綁死在語法，而是比較兩者共同原則：

- 都要綁定 `page`
- 都要封裝 locators
- 都要暴露有語意的 domain actions
- 都要避免直接把 low-level click/fill 散在 spec

---

&nbsp;
### 🍀 1003 - reusable methods

學會設計真正可重用的方法，而不是把每一步都包成薄 wrapper。

```txt
login(email, password)
addItemToCart(productName)
submitSearch(keyword)
```

重點：

- 方法名稱要貼近使用者意圖，不要只剩 `clickX()`、`fillY()`
- 只封裝穩定且重複出現的操作
- 可接受少量頁面 readiness 檢查，但不要把所有 business assertion 都塞進 POM
- class 與 functional 都應遵守相同設計原則

---

&nbsp;
### 🍀 1004 - component object

當頁面中有可重複使用的區塊時，應抽成 component object，而不是把所有東西都留在單一 page object。

例如：

```txt
Header
Sidebar
Modal
ProductCard
DatePicker
```

理解：

```txt
Page Object = 整頁流程
Component Object = 可重用區塊
組合（composition）比無限制繼承更常見也更穩定
```

---

&nbsp;
### 🍀 1005 - boundaries and anti-pattern

本節要明確建立 POM 與其他層的邊界：

```txt
POM：封裝頁面結構與操作
fixture：負責注入依賴與測試生命週期
helper：放純工具函式
data：放 seed、faker、測試資料 builder
spec：描述案例、驗證結果、組合流程
```

常見反模式：

- God object：一個 page file 管全站所有功能
- selector 泄漏：spec 直接寫一堆 locator，POM 變半套
- assertion 雜燴：把大量業務驗證塞進 POM
- abstraction 過度：每個 click 都包一層，卻沒有提升可讀性
- 把 functional POM 誤寫成一般 helper，失去頁面邊界

也要知道：當抽象層變厚之後，debug、trace、step 設計會更重要，這會直接銜接下一章。

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

保留最重要、最不能壞的核心使用者流程。

---

&nbsp;
### 🍀 1504 - smoke test

快速確認主流程可用，通常適合用少量高價值案例覆蓋。

---

&nbsp;
### 🍀 1505 - regression test

用來防止既有功能回歸；實務上通常以標記、測試清單或 CI pipeline 管理，不一定另外切資料夾。

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

建立 AI browser automation 的心智模型，理解各工具的定位、限制與選擇依據。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 1801 - LLM + Browser Control 演進趨勢

理解從傳統 script → AI-assisted automation 的演進脈絡：

```txt
手寫 Playwright script
→ Codegen / 錄製輔助原型
→ LLM 輔助產生測試與腳本
→ Agent 參與規劃、執行、修復
```

---

&nbsp;
### 🍀 1802 - 三種官方路線比較：MCP vs CLI+Skills vs Test Agents

| 路線 | 代表工具 | 適用情境 |
|---|---|---|
| MCP Server | `@playwright/mcp` | AI Client 長期操控瀏覽器 |
| CLI + Skills | `@playwright/cli` + `SKILL.md` | Coding agent 高效率呼叫瀏覽器指令 |
| Test Agents | Playwright Test Agents | 規格導向測試生成、修復與維護 |

補充：

```txt
三條路線的底層都可能使用 Observe → Plan → Act → Verify 的 agent 模式，
差別在於介面形式、持久狀態、適用任務與人機分工比例。
```

---

&nbsp;
### 🍀 1803 - Accessibility Tree 作為 AI 頁面表示的核心

理解為什麼 AI 操控瀏覽器常優先使用 Accessibility Tree（結構化快照）：

```txt
截圖 → 需要視覺模型（高 token、高延遲）
Accessibility Tree → 結構化文字（低 token、高精度）
```

但也要理解限制：

```txt
canvas / chart / 地圖 / 純視覺佈局
自訂元件語意不完整
需依座標互動的拖拉與繪圖場景
```

---

&nbsp;
### 🍀 1804 - Snapshot mode vs Vision mode：優勢與限制

```txt
Snapshot mode（預設）：使用 Accessibility Tree
優點：低 token、可結構化推理、適合表單與一般網頁操作

Vision mode：依賴畫面與座標
優點：可處理純視覺元素、canvas、圖表、座標操作
代價：較慢、較貴、穩定性依賴畫面品質
```

---

&nbsp;
### 🍀 1805 - agentic browser 共通流程與適用邊界

理解：

```txt
Observe（快照頁面）
→ Plan（LLM 決策下一步）
→ Act（執行瀏覽器操作）
→ Verify（確認結果）
→ 循環
```

也要知道什麼情境不該優先使用 agent：

```txt
高風險金流 / 刪除操作
嚴格合規或需明確審批的流程
已有穩定 deterministic script 的核心流程
```

---

&nbsp;
# 💫Chapter 19 — Playwright MCP Server

## 🧩學習目標

學會透過 Model Context Protocol 讓 AI Client 直接操控瀏覽器，並理解其能力邊界。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 1901 - 什麼是 MCP 與 @playwright/mcp

```bash
npx @playwright/mcp@latest
```

透過 MCP 協議暴露瀏覽器工具給 AI Client（例如 Claude Desktop、VS Code、Cursor 等）。

---

&nbsp;
### 🍀 1902 - 連接 AI Client 與使用情境

適合情境：

```txt
AI Client 長期操控瀏覽器
需要持久 session / profile
需要多輪觀察與操作
```

---

&nbsp;
### 🍀 1903 - Tool 類別、caps 與版本敏感設定

```txt
核心（預設）：click、fill、hover、snapshot、tab 管理…
選用能力可能包含：network、storage、devtools、vision、pdf、testing…
```

重點：

```txt
不同版本的 flags / caps / 功能集合可能變動
文件中應標記「版本敏感內容，需定期驗證」
```

---

&nbsp;
### 🍀 1904 - User Profile、Extension 與 Headless 模式

```txt
Persistent（預設）：儲存在本機 profile
Isolated：每次會話獨立，關閉即清除
Browser Extension：連接既有已登入瀏覽器
Headless：適合 CI / server 環境
```

---

&nbsp;
### 🍀 1905 - 安全邊界與風險控管

理解以下觀念：

```txt
MCP 不是 security boundary
需考慮 allowed / blocked origins
需注意 secrets、下載檔案、檔案系統存取與 profile 汙染
需避免多個 client 同時搶用同一 persistent profile
```

---

&nbsp;
### 🍀 1906 - codegen、trace 與 CI 實務

```txt
codegen：可作為操作示範與測試草稿來源
trace / console / network：作為觀測與除錯核心
headless / port：適合 server 與 CI 整合
```

---

&nbsp;
# 💫Chapter 20 — Playwright CLI

## 🧩學習目標

學會使用 `@playwright/cli` 指令集，讓 coding agent 高效呼叫瀏覽器操作。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 2001 - @playwright/cli 安裝與定位

```bash
npm install -g @playwright/cli@latest
```

與 MCP 的核心差異：

```txt
CLI：較輕量、命令導向、適合快速呼叫與大型程式碼庫
MCP：較適合持久狀態、多輪互動與 rich tool integration
```

---

&nbsp;
### 🍀 2002 - open / attach 與 session 基本模型

先理解：

```txt
CLI 並不是只有單次命令
通常要先建立或附加 session，再對該 session 持續操作
```

---

&nbsp;
### 🍀 2003 - 常用命令：goto、click、fill、snapshot

核心操作：

```bash
playwright-cli open https://example.com
playwright-cli snapshot
playwright-cli click [ref]
playwright-cli fill [ref] "text"
playwright-cli screenshot
playwright-cli eval "document.title"
```

---

&nbsp;
### 🍀 2004 - 多 session 管理與 Monitoring Dashboard

```txt
可同時維護 admin / user / guest 等 session
可查看 session 清單、狀態與 dashboard
適合多角色流程與 agent 協作場景
```

---

&nbsp;
### 🍀 2005 - CLI 適合的工作型態與限制

```txt
適合：快速操作、低額外上下文成本、命令式自動化
限制：較不適合長期持久狀態與高度 schema 驅動的互動
spec-driven workflow 不應與單一 CLI 子命令混為一談
```

---

&nbsp;
# 💫Chapter 21 — Playwright Skills

## 🧩學習目標

理解 Skills 機制，讓 coding agent 自動發現並使用瀏覽器工具。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 2101 - Skills 是什麼

Skills 是給 agent 讀取的任務說明與操作慣例。

```txt
Skills ≠ 工具本身
Skills = 教 agent 何時、如何、安全地用工具
```

---

&nbsp;
### 🍀 2102 - playwright-cli install --skills

```bash
playwright-cli install --skills
```

用途：安裝可被 agent 讀取的 skill 文件或範本。

---

&nbsp;
### 🍀 2103 - Skills、Instructions 與 Agent 發現機制

```txt
Skills 是 instruction layer，不是 runtime layer
不同 agent / client 的發現方式、目錄、命名規則可能不同
文件應區分「概念共通」與「client-specific 實作」
```

---

&nbsp;
### 🍀 2104 - 自訂 SKILL.md 的設計原則

```txt
描述適用任務
列出可用指令與範例
加上 guardrails、禁止操作、輸出格式要求
避免把易變的 CLI 細節寫死
```

---

&nbsp;
### 🍀 2105 - Claude Code / GitHub Copilot / 其他 Client 差異

```txt
有些 client 會自動發現 skills
有些 client 主要依靠 workspace instructions / tool schema
不要把單一 client 的目錄規則當成所有 agent 的通則
```

---

&nbsp;
# 💫Chapter 22 — Playwright Test Agents

## 🧩學習目標

理解官方 Playwright Test Agents 的角色，學會 spec-driven 測試生成、修復與人機協作。

&nbsp;
## 🧩章節內容

&nbsp;
### 🍀 2201 - 什麼是 Playwright Test Agents

理解這不是泛稱的 agent loop，而是針對 Playwright Test 工作流設計的 agent 能力。

---

&nbsp;
### 🍀 2202 - planner / generator / healer 的角色

```txt
planner：根據需求整理測試案例與策略
generator：產生測試檔與步驟
healer：在失敗後協助修復 locator 與測試內容
```

---

&nbsp;
### 🍀 2203 - Spec-driven workflow：seed.spec.ts → specs → tests

理解典型流程：

```txt
seed.spec.ts：提供初始種子與範圍
specs/：整理需求、案例、測試規格
tests/：產生或維護可執行的 Playwright 測試
```

---

&nbsp;
### 🍀 2204 - self-healing 與 human review 邊界

```txt
agent 可協助修復，但不代表應完全自動合併
核心業務流程、破壞性操作、權限相關變更仍需人工審查
```

---

&nbsp;
### 🍀 2205 - tracing、debug 與 observability

```txt
trace viewer
console / network logs
agent 執行紀錄
PR review 與 diff 檢查
```

---

&nbsp;
### 🍀 2206 - 適用情境、限制與導入策略

適合：

```txt
需求規格明確、測試案例可描述、團隊接受 agent 協作
```

限制：

```txt
不是所有 flaky test 都能靠 healer 解決
測試資料、環境穩定度、review 流程仍是成功關鍵
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
19 Playwright MCP
20 Playwright CLI
22 Playwright Test Agents
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
19 Playwright MCP
20 Playwright CLI
21 Playwright Skills
22 Playwright Test Agents
23 Accessibility
```

&nbsp;
