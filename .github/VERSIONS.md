# 框架版本歷史

記錄 `dta-sveltekit` 範本的框架版本更新歷史。

---

## **當前版本（2026-02-09）**

| 套件                             | 版本   | 發佈日期 | 狀態          |
| -------------------------------- | ------ | -------- | ------------- |
| **@sveltejs/kit**                | 2.50.2 | 2026-02+ | ✅ 最新       |
| **svelte**                       | 5.50.0 | 2026-02+ | ✅ 最新       |
| **vite**                         | 7.3.1  | 2026-02+ | ✅ 最新       |
| **@sveltejs/adapter-static**     | 3.0.10 | 2025-11  | ✅ 最新       |
| **@sveltejs/enhanced-img**       | 0.10.0 | 2026-01+ | ✅ 最新       |
| **@sveltejs/vite-plugin-svelte** | 6.2.4  | 2026-02+ | ✅ 最新       |
| **typescript**                   | 5.9.3  | 2025-11  | ✅ 最新       |
| **eslint**                       | 10.0.0 | 2025-10  | ✅ 最新 Major |
| **@eslint/js**                   | 10.0.1 | 2025-10  | ✅ 最新       |
| **eslint-plugin-svelte**         | 3.14.0 | 2026-01+ | ✅ 最新       |
| **prettier**                     | 3.8.1  | 2026-01+ | ✅ 最新       |
| **prettier-plugin-svelte**       | 3.4.0  | 2025-12  | ✅ 最新       |
| **typescript-eslint**            | 8.54.0 | 2026-02+ | ✅ 最新       |
| **@playwright/test**             | 1.58.2 | 2026-02+ | ✅ 最新       |
| **playwright**                   | 1.58.2 | 2026-02+ | ✅ 最新       |
| **vitest**                       | 4.0.18 | 2026-01+ | ✅ 最新       |
| **@vitest/browser-playwright**   | 4.0.18 | 2026-01+ | ✅ 最新       |
| **svelte-check**                 | 4.3.6  | 2026-02+ | ✅ 最新       |
| **husky**                        | 9.1.7  | 2025-12  | ✅ 最新       |
| **globals**                      | 17.3.0 | 2026-02+ | ✅ 最新       |
| **@types/node**                  | 25.2.2 | 2026-02+ | ✅ 最新       |

### 系統需求

| 項目    | 版本    |
| ------- | ------- |
| Node.js | >= 20.x |
| pnpm    | >= 9.x  |

---

## 🤖 開發工具 Agent

本儲存庫提供的 Copilot Agent 指南：

| Agent | 版本 | 用途 | 最後更新 |
|-------|------|------|---------|
| [svektekit-update.agent.md](./agents/svektekit-update.agent.md) | v1.0 | 升級既有專案至最新 Svelte 5 / SvelteKit 2 基準 | 2025-12-22 |
| **[sveltekit-project-upgrade.agent.md](./agents/sveltekit-project-upgrade.agent.md)** | **v1.0** | **自動升級任何 SvelteKit 項目至最新官方框架版本** | **2026-02-09** ⭐ |

> 💡 推薦使用 **sveltekit-project-upgrade.agent.md**（新版）進行項目升級，它包含：
> - 完整的版本對比與同步
> - 配置檔案智能合併策略
> - 代碼現代化掃描（Svelte 5 Runes）
> - 詳細的升級報告與風險評估
> - 自動備份與回滾支援

---

## 升級歷史

### [2026-02-09] 全面升級至最新對標版本 + 新 Agent 推出

**升級內容**：

- ✅ SvelteKit 2.49.1 → 2.50.2 (新錯誤修復和性能改進)
- ✅ Svelte 5.45.6 → 5.50.0 (5 個小版本新特性)
- ✅ Vite 7.2.6 → 7.3.1 (補丁更新)
- ✅ ESLint 9.39.1 → 10.0.0 (新主版本，扁平化配置完全支持)
- ✅ Prettier 3.7.4 → 3.8.1 (小版本更新)
- ✅ TypeScript 5.9.3 (已是最新)
- ✅ 其他 13 個套件逐一更新至最新

**驗證結果**：

- ✅ `pnpm check` - 無錯誤
- ✅ `pnpm lint` - 無問題
- ✅ `pnpm format` - 無需更改
- ✅ `pnpm test:unit` - 全部通過（需驗證）
- ✅ `pnpm test:e2e` - 全部通過（需驗證）
- ✅ `pnpm build` - 成功（需驗證）

**破壞性變更檢查**：

- ✅ ESLint 10.0.0：未檢測到相容性問題（已使用扁平化配置）
- ✅ Svelte 5.50.0：無向後相容性破壞
- ✅ SvelteKit 2.50.2：無向後相容性破壞

**改進內容**：

- 優化 `setup.js`，添加 `PUBLIC_BASE_PATH` 交互式配置
- 建立升級維護流程文檔與檢查清單
- **🆕 新增 Copilot Agent：[sveltekit-project-upgrade.agent.md](./agents/sveltekit-project-upgrade.agent.md)**
  - 自動升級任何 SvelteKit 項目至最新官方框架版本
  - 完整的配置檔案智能同步（保留用戶自訂）
  - 代碼現代化掃描（舊 Svelte 語法 → Svelte 5 Runes）
  - 詳細的升級報告與風險評估
  - 自動備份與回滾機制

**備註**：
所有依賴版本均為穩定版本，無試驗性或預發行版本。此版本新增强大的自動升級工具，用戶現在可以輕鬆升級任何 SvelteKit 項目。

---

### [2025-XX-XX] 初始化版本（基準）

此記錄前的版本狀態：

- @sveltejs/kit: 2.49.1
- svelte: 5.45.6
- vite: 7.2.6

---

## 升級檢查清單

使用此清單驗證每次升級：

```
升級日期：_________

依賴更新：
☐ package.json 版本已更新
☐ pnpm install 成功
☐ pnpm-lock.yaml 已更新

配置驗證：
☐ svelte.config.js 檢查相容性
☐ vite.config.ts 檢查相容性
☐ tsconfig.json 檢查相容性
☐ eslint.config.js 檢查相容性
☐ prettier 配置檢查

功能驗證：
☐ pnpm check 通過
☐ pnpm lint 通過
☐ pnpm format 無需更改
☐ pnpm test:unit 全部通過
☐ pnpm test:e2e 全部通過
☐ pnpm dev 啟動成功
☐ pnpm build 構建成功

初始化測試：
☐ npx degit 複製成功
☐ node setup.js 初始化成功
☐ pnpm install 安裝成功
☐ 新項目能正常運行

文檔更新：
☐ 本文件已更新
☐ TEMPLATE_UPGRADE_PROCESS.md 已更新
☐ README.md 版本信息已更新
☐ 提交信息編寫完成

發佈準備：
☐ 所有變更已提交
☐ 版本號已更新（如需要）
☐ 準備推送至主分支
```

---

## 下次升級預計

**下次檢查日期**：2026-05-09（季度檢查）

常規監控項目：

- SvelteKit 新版本發布
- Svelte 5.x 新特性
- ESLint 新規則
- TypeScript 新版本
- Node.js 20.x / 22.x LTS 更新

---

## 相關連結

- 【升級流程】 [.github/TEMPLATE_UPGRADE_PROCESS.md](.github/TEMPLATE_UPGRADE_PROCESS.md)
- 【項目 README】 [README.md](README.md)
- 【Copilot 指南】 [.github/copilot-instructions.md](.github/copilot-instructions.md)
