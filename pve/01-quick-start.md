# 快速上手

## 前置條件

- 支援 Intel VT / AMD-V 的 64 位元 CPU
- 至少 2GB RAM（建議 8GB 以上）
- 一張網卡
- 可從 USB 開機的電腦或伺服器

## 最小範例：建立第一台虛擬機

### 步驟 1：登入 Web 介面

安裝完成後，瀏覽器開啟 `https://<PVE-IP>:8006`，使用 `root` 與安裝時設定的密碼登入。

### 步驟 2：下載系統映像

1. 左側點選 **local** 存儲
2. 點 **Content** → **Templates**
3. 點 **Download from URL**，輸入：
   - URL: `https://cloud.debian.org/images/cloud/bookworm/latest/debian-12-generic-amd64.qcow2`
   - 檔名：`debian-12-cloud.qcow2`
4. 點 **Query URL** 後 **Download**

### 步驟 3：建立虛擬機

1. 右上角 **Create VM**
2. **General**：名稱填 `test-vm`，下一步
3. **OS**：選 `Use CD/DVD disc image` → `local:iso/` 或略過（使用 Cloud-Init 映像時可略過）
4. **Disks**：刪除預設磁碟，改選 `Use existing disk image` → 選剛下載的 `debian-12-cloud.qcow2`
5. **CPU**：1 核心即可
6. **Memory**：512 或 1024 MB
7. **Network**：預設 `vmbr0` 即可
8. **Confirm** → **Finish**

### 步驟 4：啟動並連線

1. 在 VM 上右鍵 → **Start**
2. 右鍵 → **Console** → **noVNC** 開啟主控台

**預期結果**：虛擬機開機，可看到 Debian 登入畫面。

## 常見錯誤

| 錯誤現象 | 解決方式 |
|----------|----------|
| 無法登入 Web | 檢查防火牆是否開放 8006，或 `systemctl status pveproxy` |
| VM 無法啟動 | 確認 CPU 支援虛擬化，BIOS 中啟用 VT-x/AMD-V |
| 找不到映像 | 確認下載完成，在 **local** → **Content** 檢查 |
| 網路不通 | 檢查 `vmbr0` 是否綁定實體網卡，節點網路設定 |

## 下一步

理解 VM、存儲與網路架構 → [核心概念](02-core-concepts.md)
