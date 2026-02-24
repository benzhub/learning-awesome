# 快速上手 pnpm

## 安裝 pnpm

### 方式一：透過 npm 安裝（推薦）

```bash
npm install -g pnpm
```

### 方式二：透過 Homebrew（macOS）

```bash
brew install pnpm
```

### 方式三：官方安裝腳本

```bash
curl -fsSL https://get.pnpm.io/install.sh | sh -
```

驗證安裝：

```bash
pnpm --version
# 輸出：10.x.x
```

> 💡 可設定短別名，減少輸入：在 `.zshrc` 或 `.bashrc` 加入 `alias pn=pnpm`

---

## 初始化第一個專案

```bash
mkdir my-project && cd my-project
pnpm init          # 建立 package.json
```

安裝套件：

```bash
pnpm add react react-dom          # 安裝到 dependencies
pnpm add -D typescript            # 安裝到 devDependencies
pnpm add -g nodemon               # 全域安裝
```

移除套件：

```bash
pnpm remove react                 # 移除套件
```

---

## 基礎指令對照表（npm → pnpm）

| 你熟悉的 npm 指令 | 對應的 pnpm 指令 |
|------------------|----------------|
| `npm install` | `pnpm install` |
| `npm i <pkg>` | `pnpm add <pkg>` |
| `npm i -D <pkg>` | `pnpm add -D <pkg>` |
| `npm uninstall <pkg>` | `pnpm remove <pkg>` |
| `npm run <script>` | `pnpm <script>` 或 `pnpm run <script>` |
| `npx <cmd>` | `pnpm dlx <cmd>` |
| `npm update` | `pnpm update` |

---

## 執行第一個腳本

在 `package.json` 中加入腳本：

```json
{
  "scripts": {
    "dev": "node index.js",
    "build": "tsc"
  }
}
```

執行腳本（不需要 `run`）：

```bash
pnpm dev        # 等同 npm run dev
pnpm build
```

---

## 第一個完整流程

```bash
# 1. 建立專案
mkdir hello-pnpm && cd hello-pnpm
pnpm init

# 2. 安裝依賴
pnpm add express
pnpm add -D @types/express typescript

# 3. 建立入口檔案
echo 'console.log("Hello pnpm!")' > index.js

# 4. 執行
node index.js
# 輸出：Hello pnpm!
```

安裝完成後你會看到：

```
node_modules/
├── .pnpm/          ← pnpm 管理的實際檔案
└── express -> .pnpm/express@x.x.x/...  ← 符號連結
pnpm-lock.yaml      ← 鎖定版本（務必提交至 git）
```

> ⚠️ `pnpm-lock.yaml` 必須提交到版本控制，確保團隊安裝結果一致。

---

> 下一步：[02-core-concepts.md](./02-core-concepts.md) — 理解 pnpm 核心原理
