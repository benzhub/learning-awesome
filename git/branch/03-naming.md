# Git Branch 命名規範

## 為什麼要規範？

- 一眼看出分支用途
- 方便團隊協作與 Code Review
- 利於自動化（CI/CD 可依命名觸發不同流程）

---

## 推薦格式

```
<類型>/<ticket-id>-<簡短描述>
```

| 類型 | 說明 | 範例 |
|------|------|------|
| `feature/` | 新功能 | `feature/PROJ-123-add-dark-mode` |
| `fix/` | Bug 修復 | `fix/PROJ-456-login-timeout` |
| `hotfix/` | 生產緊急修復 | `hotfix/PROJ-789-payment-crash` |
| `release/` | 發版分支 | `release/v1.2.0` |
| `chore/` | 工具／配置 | `chore/upgrade-deps` |
| `docs/` | 文件 | `docs/api-readme` |

---

## 命名規則

1. **小寫**：`feature/add-login` 而非 `feature/Add-Login`
2. **用連字號**：`feature/user-profile` 而非 `feature/user_profile`
3. **簡短描述**：能表達目的即可，不必過長
4. **Ticket ID 可選**：有 Jira/Linear 等系統時建議加上

---

## 範例

```bash
# 新功能
git checkout -b feature/PROJ-101-checkout-flow

# Bug 修復
git checkout -b fix/PROJ-202-null-pointer-auth

# 緊急修復
git checkout -b hotfix/PROJ-303-payment-gateway

# 發版
git checkout -b release/v1.2.0

# 無 ticket 時
git checkout -b chore/update-eslint-config
```

---

## 下一步

👉 [04-strategy.md](./04-strategy.md) — Merge 與 Rebase 策略
