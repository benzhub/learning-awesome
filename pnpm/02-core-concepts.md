# pnpm 核心概念

## 1. 全域 Content-Addressable Store

pnpm 在你的電腦只保存**每個版本的套件一份**，存放於全域 Store：

```
~/.local/share/pnpm/store/   (Linux/macOS)
%APPDATA%\pnpm\store\        (Windows)
```

查看 Store 位置：

```bash
pnpm store path
# /Users/you/.local/share/pnpm/store/v3
```

**運作方式：**

```
第一次安裝 react@18.0.0
  → 下載並存入 Store（~/.pnpm-store/...）
  → 在專案 node_modules 建立硬連結（hard link）

第二次在另一個專案安裝 react@18.0.0
  → Store 已有 → 直接建立硬連結，不重複下載 ✅
```

> 💡 硬連結（hard link）讓多個專案「共用」同一份實體檔案，節省大量磁碟空間。

---

## 2. node_modules 結構：嚴格隔離

### npm 的平鋪結構（問題所在）

```
node_modules/
├── react/
├── loose-envify/      ← react 的依賴，也被提升到根層
├── js-tokens/         ← loose-envify 的依賴，也被提升
└── ...（全部混在一起）
```

任何套件都能被直接 `require`，即使未宣告在 `package.json`。

### pnpm 的嚴格結構（正確做法）

```
node_modules/
├── .pnpm/                         ← 實際存放所有套件（硬連結）
│   ├── react@18.0.0/
│   │   └── node_modules/
│   │       ├── react/             ← react 本體
│   │       └── loose-envify/      ← react 的依賴，隔離在此
│   └── express@4.18.0/
│       └── node_modules/
│           └── express/
├── react -> .pnpm/react@18.0.0/node_modules/react    ← symlink
└── express -> .pnpm/express@4.18.0/node_modules/express
```

**結果：** 你只能使用 `package.json` 中宣告的套件，杜絕幽靈依賴。

---

## 3. 幽靈依賴問題（Ghost Dependencies）

```json
// package.json（只宣告了 express）
{
  "dependencies": {
    "express": "^4.18.0"
  }
}
```

```javascript
// npm 環境下可以跑（但這是錯的！）
const qs = require('qs')        // qs 是 express 的依賴，未宣告卻可使用

// pnpm 環境下會報錯 ✅（正確行為）
// Error: Cannot find module 'qs'
```

> ⚠️ 幽靈依賴在 npm 環境看似正常，但升級 express 時 qs 版本可能改變，導致難以追蹤的 bug。

---

## 4. lockfile：pnpm-lock.yaml

pnpm 使用 `pnpm-lock.yaml` 鎖定所有依賴的精確版本：

```yaml
lockfileVersion: '9.0'

importers:
  .:
    dependencies:
      react:
        specifier: ^18.0.0
        version: 18.2.0

packages:
  react@18.2.0:
    resolution: {integrity: sha512-...}
    dependencies:
      loose-envify: ^1.1.0
```

**重要規則：**
- 必須提交到 git
- CI 環境使用 `pnpm install --frozen-lockfile` 確保版本一致
- 不要手動編輯

---

## 5. Store 維護指令

```bash
pnpm store status       # 檢查 store 完整性
pnpm store prune        # 清除未被任何專案使用的套件
pnpm store path         # 顯示 store 路徑
```

> 💡 定期執行 `pnpm store prune` 回收不再使用的舊版本套件空間。

---

> 下一步：[03-common-patterns.md](./03-common-patterns.md) — 日常開發常用指令大全
