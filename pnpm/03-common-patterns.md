# 常見使用模式

## 1. 依賴管理

### 安裝依賴

```bash
pnpm install                     # 根據 lockfile 安裝所有依賴
pnpm add react react-dom         # 安裝到 dependencies
pnpm add -D vitest               # 安裝到 devDependencies
pnpm add -O sharp                # 安裝到 optionalDependencies
pnpm add react@17                # 安裝特定版本
```

### 更新依賴

```bash
pnpm update                      # 更新所有套件（遵守 semver 範圍）
pnpm update react                # 更新單一套件
pnpm update --latest react       # 強制更新到最新版（忽略 semver）
pnpm outdated                    # 列出所有過時套件
```

### 移除依賴

```bash
pnpm remove react                # 從 dependencies 移除
pnpm remove -D typescript        # 從 devDependencies 移除
```

---

## 2. 執行腳本

```bash
pnpm dev                         # 執行 package.json 中的 dev 腳本
pnpm run build                   # 同上，明確加 run
pnpm test                        # 執行 test 腳本
pnpm exec tsc                    # 執行 node_modules/.bin 中的指令
pnpm dlx create-next-app@latest  # 臨時執行套件（等同 npx，不安裝到本地）
```

> 💡 `pnpm dlx` 是 `npx` 的對應指令，每次都從 Store 取用最新版本，不污染專案依賴。

---

## 3. CI/CD 環境

```bash
# CI 環境標準安裝（不允許更新 lockfile）
pnpm install --frozen-lockfile

# 加速 CI：搭配快取
# GitHub Actions 範例：
```

```yaml
# .github/workflows/ci.yml
- uses: pnpm/action-setup@v4
  with:
    version: 10

- uses: actions/setup-node@v4
  with:
    node-version: 20
    cache: 'pnpm'

- run: pnpm install --frozen-lockfile
- run: pnpm build
```

---

## 4. 檢視依賴資訊

```bash
pnpm list                        # 列出所有已安裝套件
pnpm list --depth 2              # 列出到第二層依賴
pnpm why react                   # 查看 react 為何被安裝（被誰依賴）
pnpm licenses list               # 列出所有套件授權
```

---

## 5. 發布套件

```bash
pnpm pack                        # 打包成 .tgz（不發布，用於測試）
pnpm publish                     # 發布到 npm registry
pnpm publish --access public     # 發布 scoped 套件（需要 public 權限）
pnpm publish --dry-run           # 模擬發布，不實際上傳
```

---

## 6. 常用工作流程範例

### 加入新功能時

```bash
pnpm add zod                     # 加入 runtime 依賴
pnpm add -D @types/node          # 加入型別定義
pnpm install                     # 確保所有人同步
```

### 接手舊專案

```bash
git clone <repo>
cd <repo>
pnpm install                     # 自動讀取 pnpm-lock.yaml
pnpm dev                         # 啟動開發伺服器
```

### 清理重裝

```bash
rm -rf node_modules
pnpm install                     # 從 Store 重建（超快）
```

> 💡 因為 Store 已有快取，重建 `node_modules` 幾乎不需要網路，速度極快。

---

> 下一步：[04-advanced.md](./04-advanced.md) — Workspace Monorepo 進階架構
