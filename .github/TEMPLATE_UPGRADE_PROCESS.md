# SvelteKit 範本升級流程

## 概述

本文檔定義了如何定期將 `dta-sveltekit` 範本與官方 SvelteKit 最新版本同步，確保所有通過 `setup.js` 初始化的新項目都使用最新的框架、工具鏈和最佳實踐。

## 升級頻率

- **定期檢查**：每季度（1月、4月、7月、10月）檢查一次
- **即時升級**：當 SvelteKit、Svelte 發布新的主版本（Major）時，應立即評估並升級
- **小版本更新**：ESLint、TypeScript、Vite 等工具的小版本更新可批量每季度執行一次

## **檢查步驟 (Quarterly Review)**

### 1. 建立官方基準項目

```bash
cd /tmp
rm -rf sveltekit-baseline
pnpx sv create sveltekit-baseline --template minimal --no-add-ons --no-dir-check
cd sveltekit-baseline
pnpm install
```

### 2. 對比版本與配置

在 `/tmp/sveltekit-baseline` 目錄中執行：

```bash
# 檢查版本
cat package.json | grep -E '"@sveltejs|"svelte|"vite|"typescript' | head -20

# 檢查配置
cat svelte.config.js
cat vite.config.ts
cat tsconfig.json
cat eslint.config.js
```

### 3. 對比記錄

建立臨時對比表格：

| 項目          | 現有版本 | 官方版本 | 需更新 |
| ------------- | -------- | -------- | ------ |
| @sveltejs/kit | 2.50.2   | X.X.X    | Yes/No |
| svelte        | 5.50.0   | X.X.X    | Yes/No |
| vite          | 7.3.1    | X.X.X    | Yes/No |
| typescript    | 5.9.3    | X.X.X    | Yes/No |
| ...           | ...      | ...      | ...    |

## 升級步驟 (When Updates Needed)

### 1. **更新 package.json**

根據對比表，更新 `devDependencies` 版本號。

**注意事項**：

- 保持 `^` 或 `~` 版本範圍符號一致
- 重點關注主版本升級（如 ESLint 9.x → 10.x），需檢查破壞性變更

### 2. **安裝依賴**

```bash
pnpm install
```

### 3. **檢查配置檔案相容性**

檢查以下檔案是否需要調整：

#### svelte.config.js

- 確保 `adapter-static` 配置與新版本相容
- 檢查 `vitePreprocess` 和其他預處理器配置

#### vite.config.ts

- 驗證 Vitest 配置是否相容
- 檢查 Playwright 集成是否正確

#### tsconfig.json

- 檢查新的 TypeScript 編譯選項
- 驗證 `skipLibCheck`, `strict` 等設定

#### eslint.config.js

- 如果升級到 ESLint 10.x，檢查新規則是否需要調整
- 驗證 TypeScript ESLint 外掛版本相容性

#### prettier 設定

- 檢查 `.prettierrc` 是否需要更新新規則

### 4. **執行驗證流程**

```bash
# 類型檢查
pnpm check

# 代碼格式化
pnpm format

# 代碼檢查
pnpm lint

# 執行單元測試
pnpm test:unit -- --run

# 執行端到端測試
pnpm test:e2e

# 本地開發伺服器
pnpm dev

# 構建靜態產物
pnpm build
pnpm preview
```

### 5. **測試初始化流程**

模擬新用戶的初始化過程：

```bash
cd /tmp
rm -rf test-dta-app
npx degit fet-3312/dat-sveltekit test-dta-app
cd test-dta-app
node setup.js  # 互動式初始化
pnpm install
pnpm check
pnpm dev  # 手動驗證應用運行
```

### 6. **檢查破壞性變更**

若涉及主版本升級（如 ESLint 9 → 10），需檢查官方遷移指南。

常見破壞性變更來源：

- ESLint 官方文檔：https://eslint.org/
- SvelteKit 遷移指南：https://svelte.dev/docs/migration
- Svelte 遷移指南：https://svelte.dev/docs/migration
- Vite 變更日誌：https://github.com/vitejs/vite/releases

### 7. **代碼重構（如需要）**

若 Svelte 有新特性或舊語法被棄用：

- 考慮是否應該更新示例代碼以展示最新最佳實踐
- 檢查 Svelte 5 Runes 的使用，確保代碼是最新的

## 文檔更新

升級後，更新以下文檔：

### 1. 更新 VERSIONS.md

在 `.github/docs/VERSIONS.md` 中記錄升級：

```markdown
## 2026-02-09

### Upgraded Packages

- @sveltejs/kit: 2.49.1 → 2.50.2
- svelte: 5.45.6 → 5.50.0
- vite: 7.2.6 → 7.3.1
- eslint: 9.39.1 → 10.0.0
- ... (other packages)

### Testing Status

- ✔ pnpm check
- ✔ pnpm lint
- ✔ pnpm test:unit
- ✔ pnpm test:e2e
- ✔ pnpm build
- ✔ new project initialization

### Breaking Changes

- ESLint 10.0.0: No breaking changes detected in flat config

### Notes

- All versions are stable and compatible
- No code changes required
```

### 2. 更新 README.md

在主 README 的「框架版本」章節更新版本資訊：

```markdown
## 框架版本

**最後更新**：2026-02-09

| 套件       | 版本    |
| ---------- | ------- |
| SvelteKit  | 2.50.2  |
| Svelte     | 5.50.0  |
| Vite       | 7.3.1   |
| TypeScript | 5.9.3   |
| Node.js    | >= 20.x |
| pnpm       | >= 9.x  |

完整的依賴版本見 [package.json](./package.json)。
```

## 提交與發佈

升級完成後，提交變更：

```bash
git add .
git commit -m "chore: upgrade dependencies to latest stable versions

- @sveltejs/kit: 2.49.1 → 2.50.2
- svelte: 5.45.6 → 5.50.0
- vite: 7.2.6 → 7.3.1
- eslint: 9.39.1 → 10.0.0
- ... (other packages)

All tests pass. No breaking changes detected."

git push origin main
```

若要發佈新版本，更新 package.json 的 version 欄位：

```bash
pnpm version patch  # 修復版本
pnpm version minor  # 新特性版本
pnpm version major  # 破壞性變更版本
```

## 監控与告警

### 訂閱更新通知

- **GitHub 倉庫 Watch**：關注 https://github.com/sveltejs/kit
- **npm 套件更新**：`npm outdated` 或使用依賴管理工具
- **官方文檔**：定期檢查 https://svelte.dev/docs 的遷移指南

### 自動化檢查

若使用 Dependabot 或類似工具，配置自動提交 PR：

`.github/dependabot.yml` 示例：

```yaml
version: 2
updates:
  - package-ecosystem: 'npm'
    directory: '/'
    schedule:
      interval: 'weekly'
    open-pull-requests-limit: 10
    allow:
      - dependency-type: 'all'
```

## 常見問題

### Q: 升級後 pnpm check 失敗

**A**: 檢查 svelte-check 和 TypeScript 版本相容性。運行 `pnpm install` 重新安裝依賴，或清除 `node_modules` 和 `pnpm-lock.yaml` 後重新安裝。

### Q: ESLint 升級後出現新警告

**A**: 檢查 eslint.config.js 中的規則配置，確保所有推薦規則相容。可參閱 ESLint 官方遷移指南。

### Q: 新初始化的項目無法運行

**A**: 確保 Husky hooks 正確初始化。運行 `husky install` 設置預提交 hooks。

### Q: Svelte 5 新特性如何應用到範本

**A**: 檢查官方文檔的 Runes 指南，考慮更新示例代碼以展示最新語法。確保所有改動向後相容或有遷移指南。

## 參考連結

- [SvelteKit 遷移指南](https://svelte.dev/docs/migration)
- [Svelte 5 Runes 文檔](https://svelte.dev/docs/svelte/runes)
- [ESLint 官方文檔](https://eslint.org/)
- [Vite 變更日誌](https://github.com/vitejs/vite/blob/main/CHANGELOG.md)
- [TypeScript 發布日誌](https://www.typescriptlang.org/docs/handbook/release-notes/)

## 維護負責人

本流程由 DTA 運維團隊維護。有問題或建議請聯繫。
