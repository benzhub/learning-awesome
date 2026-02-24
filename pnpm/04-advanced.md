# 進階用法：Workspace Monorepo

## 1. 什麼是 Workspace？

pnpm Workspace 讓你在**單一 git repo** 中管理多個套件，共享依賴安裝，
適合前後端共存、Design System、共用工具庫等場景。

```
my-monorepo/
├── pnpm-workspace.yaml      ← 定義 workspace 範圍
├── package.json             ← 根目錄 package.json
├── apps/
│   ├── web/                 ← 前端應用
│   └── api/                 ← 後端應用
└── packages/
    ├── ui/                  ← 共用 UI 元件庫
    └── utils/               ← 共用工具函式
```

---

## 2. 初始化 Workspace

### 建立 `pnpm-workspace.yaml`

```yaml
# pnpm-workspace.yaml
packages:
  - 'apps/*'
  - 'packages/*'
  - '!**/test/**'    # 排除測試目錄
```

### 根目錄 `package.json`

```json
{
  "name": "my-monorepo",
  "private": true,
  "scripts": {
    "dev": "pnpm -r --parallel run dev",
    "build": "pnpm -r run build",
    "test": "pnpm -r run test"
  }
}
```

---

## 3. 內部套件互相依賴

使用 `workspace:` 協議引用同 repo 內的套件：

```json
// apps/web/package.json
{
  "name": "@myorg/web",
  "dependencies": {
    "@myorg/ui": "workspace:*",      // 永遠用最新版
    "@myorg/utils": "workspace:^1.0.0"
  }
}
```

`workspace:*` 在發布時會自動替換為實際版本號。

---

## 4. Filter 指令（核心功能）

Filter 讓你精確控制要對哪些套件執行指令：

```bash
# 對單一套件執行
pnpm --filter @myorg/web build
pnpm --filter @myorg/ui dev

# 對路徑下所有套件執行
pnpm --filter "./apps/*" build
pnpm --filter "./packages/*" test

# 對某套件及其所有依賴執行（向上追溯）
pnpm --filter "...@myorg/utils" test
# 若 web 和 api 都依賴 utils，則三個都會被測試

# 對某套件及所有依賴它的套件執行（向下追溯）
pnpm --filter "@myorg/utils..." build

# 只對有變更的套件執行（CI 加速）
pnpm --filter "[HEAD~1]" build
```

---

## 5. 遞迴指令（對所有套件）

```bash
pnpm -r install                      # 所有套件安裝依賴
pnpm -r run build                    # 所有套件執行 build（依依賴順序）
pnpm -r --parallel run dev           # 所有套件平行執行 dev
pnpm -r --stream run test            # 所有套件測試，輸出帶套件名稱前綴
```

---

## 6. 在特定套件安裝依賴

```bash
# 安裝到指定套件
pnpm --filter @myorg/web add react
pnpm --filter @myorg/api add -D jest

# 安裝到根目錄（共用工具）
pnpm add -w -D prettier eslint
pnpm add -w -D turbo              # -w = --workspace-root
```

---

## 7. 進階 .npmrc 設定

```ini
# .npmrc（根目錄）

# Monorepo 中嚴格控制依賴提升
hoist=false
public-hoist-pattern[]=*eslint*
public-hoist-pattern[]=*prettier*
public-hoist-pattern[]=*types*

# 自動安裝 peer 依賴
auto-install-peers=true

# 私有 registry（企業內網）
@myorg:registry=https://npm.internal.company.com/
```

---

> 下一步：[05-best-practices.md](./05-best-practices.md) — 最佳實踐與避坑指南
