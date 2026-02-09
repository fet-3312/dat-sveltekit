# SvelteKit 項目升級檢查清單

新 SvelteKit 用戶使用本升級檢查清單，確保其項目總是建立在最新的框架版本之上。

---

## 📋 快速檢查

運行以下命令檢查當前項目是否使用最新版本：

```bash
# 檢查過期的依賴
pnpm outdated

# 查看當前 Node.js 版本
node --version

# 查看當前 pnpm 版本
pnpm --version
```

---

## 🔄 升級流程（適用於現有項目）

### 步驟 1：備份當前狀態

```bash
git status  # 確保工作目錄乾淨
git commit -m "chore: backup before framework upgrade"
git checkout -b chore/upgrade-sveltekit
```

### 步驟 2：更新依賴版本

編輯 `package.json`，更新 `devDependencies` 中的版本號。

參考最新版本：[.github/VERSIONS.md](./.github/VERSIONS.md)

**常見包的最新版本**：

- `@sveltejs/kit`: ^2.50.2
- `svelte`: ^5.50.0
- `vite`: ^7.3.1
- `typescript`: ^5.9.3
- `eslint`: ^10.0.0
- `prettier`: ^3.8.1
- 其他見 [VERSIONS.md](.github/VERSIONS.md)

### 步驟 3：安裝依賴

```bash
pnpm install
```

### 步驟 4：驗證相容性

```bash
# 類型檢查
pnpm check

# 代碼檢查和格式化
pnpm format
pnpm lint

# 執行單元測試
pnpm test:unit -- --run

# 執行端到端測試
pnpm test:e2e

# 本地開發驗證
pnpm dev

# 生產構建驗證
pnpm build
pnpm preview
```

### 步驟 5：解決相容性問題

如果步驟 4 中出現問題，請檢查：

#### 類型錯誤

- 運行 `pnpm check` 取得詳細信息
- 檢查 `svelte-check` 的輸出
- 審查 TypeScript 編譯器配置

#### ESLint 錯誤

- 檢查 `eslint.config.js` 是否需要更新規則
- 參考 [ESLint 官方遷移指南](https://eslint.org/)
- 特別注意主版本升級（如 9.x → 10.x）

#### 代碼格式化問題

- 運行 `pnpm format` 自動修復
- 檢查 `.prettierrc` 配置

#### 測試失敗

- 檢查 Vitest 和 Playwright 相容性
- 查看 `vite.config.ts` 中的測試配置
- 確保文件後綴符合期望（`.svelte.ts`, `.spec.ts` 等）

### 步驟 6：提交變更

```bash
git add .
git commit -m "chore: upgrade to latest SvelteKit framework

Upgrades:
- @sveltejs/kit: X.X.X → Y.Y.Y
- svelte: X.X.X → Y.Y.Y
- vite: X.X.X → Y.Y.Y
- eslint: X.X.X → Y.Y.Y
- ... (other packages)

Testing:
- ✔ pnpm check
- ✔ pnpm lint
- ✔ pnpm test:unit
- ✔ pnpm test:e2e
- ✔ pnpm build

No breaking changes detected."

git push origin chore/upgrade-sveltekit
```

### 步驟 7：提交 Pull Request

在 GitHub 上建立 PR，允許代碼審查和 CI 驗證。

---

## ✨ 新項目初始化（推薦）

對於新項目，使用官方 `dta-sveltekit` 範本，自動獲得最新配置：

```bash
# 使用 degit 複製範本
npx degit fet-3312/dat-sveltekit my-app

# 進入項目並初始化
cd my-app
node setup.js
pnpm install
pnpm dev
```

此方法確保：

- ✅ 使用最新的 SvelteKit 版本
- ✅ 最新的 Svelte 5 配置
- ✅ 最新的工具鏈（ESLint 10, Prettier, 等）
- ✅ 上等的 TypeScript 設定
- ✅ 完整的測試框架（Vitest + Playwright）
- ✅ 靜態部署優化配置

---

## 📊 版本相容性矩陣

| SvelteKit 版本 | Svelte 版本 | Node.js | TypeScript | ESLint | Vite |
| -------------- | ----------- | ------- | ---------- | ------ | ---- |
| 2.50+          | 5.50+       | >= 20.x | 5.9+       | 10.x   | 7.3+ |
| 2.49           | 5.45+       | >= 20.x | 5.8+       | 9.x    | 7.2+ |
| 2.40+          | 5.0+        | >= 18.x | 5.0+       | 9.x    | 6.0+ |

---

## ⚠️ 常見問題與解決方案

### Q: `pnpm check` 出現 "Cannot find module" 錯誤

**A**:

```bash
# 清除快取並重新安裝
rm -rf node_modules pnpm-lock.yaml
pnpm install
pnpm check
```

### Q: ESLint 升級後報告新警告

**A**:

1. 檢查新的 ESLint 規則
2. 更新 `eslint.config.js` 設定
3. 運行 `pnpm lint` 查看詳細信息
4. 如有必要，更新代碼以符合新規則

### Q: Svelte 5 新特性如何使用

**A**:
檢查 [Svelte 5 Runes 文檔](https://svelte.dev/docs/svelte/runes)：

- 使用 `$state()` 取代 `let` 定義狀態
- 使用 `$derived` 建立派生值
- 使用 `$effect` 處理副作用

### Q: 靜態部署時應用無法在子路徑運行

**A**:

1. 檢查 `.env` 文件中的 `PUBLIC_BASE_PATH` 設定
2. 確認 `svelte.config.js` 中基礎路徑配置正確
3. 驗證 SvelteKit 正確讀取環境變數

### Q: 發現新版本中有破壞性變更

**A**:

1. 檢查官方遷移指南
2. 查看 [TEMPLATE_UPGRADE_PROCESS.md](.github/TEMPLATE_UPGRADE_PROCESS.md) 的風險章節
3. 逐步應用變更，並運行測試驗證

---

## 🚀 性能優化建議

升級後，考慮應用以下優化：

### 代碼分割優化

```typescript
// 使用動態導入減少初始包大小
import { lazy } from 'svelte'

const HeavyComponent = lazy(() => import('./HeavyComponent.svelte'))
```

### 圖片優化

使用 `@sveltejs/enhanced-img` 進行自動圖片最佳化：

```svelte
<script>
	import { Image } from '@sveltejs/enhanced-img'
</script>

<Image src="/image.jpg" alt="description" />
```

### 預加載優化

```svelte
<svelte:head>
	<link rel="preload" href="/critical-font.woff2" as="font" type="font/woff2" crossorigin />
</svelte:head>
```

---

## 📞 獲得幫助

- 【升級流程詳解】 [.github/TEMPLATE_UPGRADE_PROCESS.md](.github/TEMPLATE_UPGRADE_PROCESS.md)
- 【版本歷史】 [.github/VERSIONS.md](.github/VERSIONS.md)
- 【官方文檔】 https://svelte.dev/docs
- 【SvelteKit 文檔】 https://svelte.dev/docs/kit
- 【Svelte 5 Runes】 https://svelte.dev/docs/svelte/runes

---

## 📝 升級記錄

在此記錄本項目的升級歷史（可選）：

```
【升級日期】
【升級版本】
【驗證結果】
【遇到的問題】
【發現的新特性】
```

---

_此清單最後更新於 2026-02-09_
