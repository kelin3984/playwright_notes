# 專案結構規劃 & 範例

&nbsp;
## 範例參考
通常可以這樣分：

```
.
├─ tests/                        # 放各種測試案例
│  ├─ ui/                        # 單一畫面 / 單一功能 UI 測試
│  │  ├─ login.spec.ts
│  │  ├─ order-create.spec.ts
│  │  └─ account-settings.spec.ts
│  ├─ api/                       # API 測試
│  │  ├─ auth.api.spec.ts
│  │  ├─ orders.api.spec.ts
│  │  └─ customers.api.spec.ts
│  ├─ e2e/                       # 完整使用者流程測試
│  │  ├─ user-buy-product.spec.ts
│  │  └─ admin-approve-order.spec.ts
│  ├─ integration/               # 跨模組 / 跨系統整合驗證
│  │  └─ order-status-sync.spec.ts
│  └─ smoke/                     # 部署後快速健康檢查
│     └─ critical-path.spec.ts
│
├─ src/                          # 可重用的測試支援程式碼
│  ├─ pages/                     # Page Object Model
│  │  ├─ LoginPage.ts
│  │  └─ OrdersPage.ts
│  ├─ components/                # 可重用 UI 區塊封裝
│  │  ├─ Header.ts
│  │  └─ Sidebar.ts
│  └─ api/                       # API 操作封裝
│     ├─ AuthApi.ts
│     └─ OrdersApi.ts
│
├─ fixtures/                     # 共用 fixture / 測試上下文
│  ├─ test.ts
│  └─ users.fixture.ts
│
├─ test-data/                    # 測試資料
│  ├─ users.ts
│  └─ orders.ts
│
├─ utils/                        # 共用工具函式
│  ├─ date.ts
│  ├─ random.ts
│  └─ assertions.ts
│
└─ playwright.config.ts          # Playwright 全域設定
```

### 一眼看懂每一層在做什麼

| 目錄 | 用途 |
| --- | --- |
| `tests/` | 真正執行的測試案例，依測試類型分資料夾 |
| `src/pages/` | 頁面操作與 selector 集中管理 |
| `src/components/` | 可重複使用的區塊，例如 header、sidebar、dialog |
| `src/api/` | API 呼叫封裝，避免測試檔直接寫 request 細節 |
| `fixtures/` | 建立共用測試前置與注入測試物件 |
| `test-data/` | 放假資料、測試案例資料、payload |
| `utils/` | 共用 helper，例如日期、亂數、客製 assertion |
| `playwright.config.ts` | baseURL、projects、reporter、timeout 等設定 |

&nbsp;
## 建議分類邏輯：

### 1. `tests/ui`：單一畫面或功能的 UI 測試

例如登入頁、查詢頁、表單驗證、按鈕狀態。  
這類測試主要驗證「畫面互動是否正確」。

&nbsp;
### 2. `tests/api`：API 測試

用 Playwright 內建的 `request` fixture 測 API，不需要開瀏覽器。  
官方也有支援 API testing。

&nbsp;
### 3. `tests/e2e`：完整業務流程測試

例如：

「使用者登入 → 建立訂單 → 付款 → 查看狀態」

這類測試數量不要太多，因為成本高、較慢、較容易受環境影響。

&nbsp;
### 4. `tests/integration`：跨系統或跨模組驗證

例如前台送出申請後，後台是否看到資料；  
API 更新後 UI 是否同步。

&nbsp;
### 5. `tests/smoke`：部署後快速檢查

只放最關鍵流程，例如登入、首頁載入、核心查詢功能。

&nbsp;
## 核心原則：

### - 測試檔只寫「測試意圖與驗證」

測試檔只寫「測試意圖與驗證」，  
不要塞太多 selector 或操作細節。

```ts
test('使用者可以登入', async ({ loginPage }) => {
  await loginPage.goto();

  await loginPage.login('user', 'password');

  await expect(loginPage.homeTitle).toBeVisible();
});
```

&nbsp;
### - selector 和操作細節放在 `pages/`：

```ts
export class LoginPage {
  constructor(private page: Page) {}

  username = this.page.getByLabel('帳號');

  password = this.page.getByLabel('密碼');

  loginButton =
    this.page.getByRole('button', { name: '登入' });

  homeTitle = this.page.getByText('首頁');

  async goto() {
    await this.page.goto('/login');
  }

  async login(username: string, password: string) {
    await this.username.fill(username);

    await this.password.fill(password);

    await this.loginButton.click();
  }
}
```

**Playwright 官方也建議大型測試套件可用 Page Object Model，**  
**把 selector 和頁面操作集中管理，降低重複與維護成本。**

&nbsp;
## 實務建議：

### 小專案：

```
.
├─ tests/
│  ├─ login.spec.ts
│  └─ orders.spec.ts
└─ pages/
   ├─ LoginPage.ts
   └─ OrdersPage.ts
```

&nbsp;
### 中大型專案：

```
.
├─ tests/
│  ├─ ui/
│  ├─ api/
│  └─ e2e/
└─ src/
   ├─ pages/
   ├─ components/
   ├─ api/
   ├─ fixtures/
   ├─ test-data/
   └─ utils/
```

&nbsp;
### 企業級系統 / 多業務模組：

```
.
├─ tests/
│  ├─ securities/
│  │  ├─ account-opening/
│  │  ├─ order/
│  │  └─ statement/
│  ├─ admin/
│  ├─ api/
│  └─ smoke/
└─ src/
   └─ ...
```

&nbsp;
### tests 中，檔案命名建議：

```
login.spec.ts
order-create.spec.ts
order-cancel.api.spec.ts
account-opening.e2e.spec.ts
```

&nbsp;
## 簡單判斷規則：

- UI 元件互動 → `ui`
- 純 API 驗證 → `api`
- 完整使用者流程 → `e2e`
- 跨模組資料流 → `integration`
- 部署健康檢查 → `smoke`

&nbsp;
## 最推薦的起手式：

```
.
├─ tests/
│  ├─ ui/
│  ├─ api/
│  └─ e2e/
└─ src/
   ├─ pages/
   ├─ fixtures/
   ├─ test-data/
   └─ utils/
```

這種起手式的優點是：

- `tests/` 專心放測試案例
- `src/` 專心放可重用邏輯
- 一開始不會拆太細，但後續也很好擴充
