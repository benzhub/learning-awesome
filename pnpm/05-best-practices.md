# 最佳實踐與避坑指南

## 1. 一定要做的事

### 提交 pnpm-lock.yaml

```bash
# .gitignore 中不要加這行！
# pnpm-lock.yaml    ← 錯誤，不應該忽略

# 正確做法：提交到 git
git add pnpm-lock.yaml
git commit -m "chore: update lockfile"
```

**原因：** lockfile 確保所有開發者和 CI 環境安裝完全相同的版本。

### CI 使用 --frozen-lockfile

```bash
# CI 腳本
pnpm install --frozen-lockfile   # 若 lockfile 不符則報錯，不自動更新
```

---

## 2. 常見錯誤與解決方案

| 問題 | 原因 | 解決方案 |
|------|------|---------|
| `Cannot find module 'xxx'` | 使用了幽靈依賴 | 在 `package.json` 明確加入該套件 |
| symlink 損壞 | store 不一致 | `pnpm store prune && pnpm install` |
| peer deps 警告大量出現 | peer 未安裝 | `.npmrc` 加 `auto-install-peers=true` |
| `ERR_PNPM_OUTDATED_LOCKFILE` | lockfile 與 package.json 不同步 | 執行 `pnpm install` 更新 lockfile |
| Monorepo 中套件找不到 | workspace 協議設定錯誤 | 確認 `pnpm-workspace.yaml` 路徑正確 |

---

## 3. node_modules 依賴提升策略

### 嚴格模式（推薦）

```ini
# .npmrc
hoist=false
```

所有依賴完全隔離，是最安全的設定，但可能導致某些舊工具不相容。

### 半嚴格模式（pnpm 預設）

```ini
# .npmrc（預設值，不需手動設定）
hoist-pattern[]=*
public-hoist-pattern[]=*types*
public-hoist-pattern[]=*eslint*
```

### 相容舊工具模式（不推薦，但有時必要）

```ini
# .npmrc
shamefully-hoist=true    # 行為類似 npm，所有套件都提升到根目錄
```

> ⚠️ `shamefully-hoist=true` 會讓 pnpm 的嚴格性保障失效，只在無法避免時使用。

---

## 4. Store 定期維護

```bash
# 每隔一段時間清理未使用的舊版本套件
pnpm store prune

# 驗證 store 完整性
pnpm store status
```

---

## 5. Monorepo 最佳實踐

```bash
# 共用工具安裝到根目錄
pnpm add -w -D prettier eslint typescript

# 應用特定依賴安裝到各自套件
pnpm --filter @myorg/web add react
pnpm --filter @myorg/api add express

# 版本統一：避免同一套件在不同子套件有不同版本
# 使用 pnpm update -r <pkg> 統一更新
pnpm update -r typescript
```

### workspace 內部依賴最佳寫法

```json
{
  "dependencies": {
    "@myorg/utils": "workspace:*"   // 推薦：永遠指向最新本地版本
  }
}
```

---

## 6. 團隊協作規範

**建議加入 README 的說明：**

```markdown
## 開發環境要求

- Node.js >= 20
- pnpm >= 9（安裝：`npm i -g pnpm`）

## 安裝依賴

\`\`\`bash
pnpm install
\`\`\`

## 禁止使用 npm install 或 yarn install
本專案使用 pnpm 管理依賴，請勿混用其他套件管理工具。
```

**加入 `.npmrc` 防止意外使用 npm：**

```ini
engine-strict=true
```

**`package.json` 指定 pnpm 版本：**

```json
{
  "engines": {
    "node": ">=20",
    "pnpm": ">=9"
  },
  "packageManager": "pnpm@9.0.0"
}
```

---

## 快速回顧

- ✅ 永遠提交 `pnpm-lock.yaml`
- ✅ CI 使用 `--frozen-lockfile`
- ✅ 所有依賴宣告在 `package.json`，不依賴幽靈依賴
- ✅ Monorepo 共用工具裝在根目錄（`-w`），應用依賴裝在各自套件
- ✅ 定期 `pnpm store prune` 清理空間
- ❌ 不混用 npm/yarn/pnpm
- ❌ 不手動編輯 `pnpm-lock.yaml`
- ❌ 不在 `.gitignore` 忽略 lockfile
