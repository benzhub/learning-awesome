# 進階用法

## 集群建立

**何時需要**：多台 PVE 主機需集中管理、線上遷移或 HA。

```bash
# 在第一台節點建立集群
pvecm create my-cluster

# 在第二台節點加入（需輸入第一台 root 密碼）
pvecm add <第一台節點IP>

# 檢查狀態
pvecm status
```

**Web 操作**：資料中心 → Cluster → Create Cluster / Join Cluster。

> ⚠️ 集群建立後節點名稱不可改，加入前確認主機名與網路正確。

---

## HA 高可用

**何時需要**：VM 需在節點故障時自動遷移或重啟。

**前置**：已建立集群，VM 磁碟在共享存儲。

```bash
# 建立 HA 資源（VM 100）
ha-manager add vm:100 --maxrestart 3 --maxrelocate 5

# 或 Web：資料中心 → HA → Add → 選 VM
```

**概念**：HA 管理器監控 VM 狀態，節點故障時在其它節點啟動 VM。需配合 Fencing（隔離）確保腦裂時正確處理。

---

## Ceph 超融合存儲

**何時需要**：多節點共享存儲、高可用、自我修復。

**前置**：至少 3 節點、每節點多塊磁碟、專用網路。

```bash
# 透過 Web 精靈：資料中心 → Ceph → Create Ceph cluster
# 或 CLI 初始化
pveceph init --network 10.0.0.0/24

# 建立 OSD（每顆資料碟）
pveceph osd create /dev/sdX

# 建立 Pool
pveceph pool create vm-data --add_storages
```

**注意**：Ceph 不建議與硬體 RAID 混用，每顆碟獨立交給 Ceph 管理。

---

## 存儲複製（Storage Replication）

**何時需要**：兩地備援、異地容災。

```bash
# 建立複製任務
pvesr create-local-job 100 local-zfs remote-zfs --schedule "0 2 * * *"
```

需兩端都有對應存儲，並設定好連線與認證。

---

## API 與自動化

**何時需要**：腳本、CI/CD、外部系統整合。

```bash
# 取得 Ticket（需先登入取得 ticket 與 CSRF token）
# 或使用 API Token：資料中心 → Permissions → API Tokens

curl -k -d "username=root@pam&password=xxx" \
  https://<PVE-IP>:8006/api2/json/access/ticket
```

**範例**：列出所有 VM

```bash
curl -k -H "Authorization: PVEAPIToken=user@realm!tokenid=secret" \
  https://<PVE-IP>:8006/api2/json/nodes/<node>/qemu
```

## 下一步

避坑與優化建議 → [最佳實踐](05-best-practices.md)
