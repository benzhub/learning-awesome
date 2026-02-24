# 核心概念

## 概念一：虛擬機（VM）與容器（LXC）

Proxmox VE 支援兩種虛擬化：

| 類型 | 技術 | 特點 |
|------|------|------|
| **VM** | KVM/QEMU | 完整作業系統、支援 Windows、隔離性高 |
| **LXC** | Linux 容器 | 輕量、共用核心、僅 Linux、啟動快 |

> 💡 關鍵洞察：需要跑 Windows 或完整隔離用 VM；純 Linux 服務、高密度部署用 LXC。

```bash
# 命令列建立 VM
qm create 100 --name my-vm --memory 1024 --cores 2

# 命令列建立 LXC 容器
pct create 200 local:vztmpl/debian-12-standard_12.2-1_amd64.tar.zst --hostname my-ct --memory 512
```

## 概念二：存儲（Storage）

存儲是 VM/容器磁碟的存放位置，分為：

- **local**：本機目錄，單節點使用
- **local-lvm**：本機 LVM，支援快照、克隆
- **NFS / CIFS**：網路共享，可跨節點
- **ZFS / Ceph**：進階，支援快照、複製、HA

```bash
# 查看存儲
pvesm status

# 查看存儲內容
pvesm list local
```

> 💡 關鍵洞察：要做線上遷移或 HA，VM 磁碟需放在共享存儲（NFS、Ceph 等）。

## 概念三：網路（vmbr0）

預設使用 **橋接模式**：`vmbr0` 橋接實體網卡，VM 如同接在同一台虛擬交換器。

```
[實體網卡 enp0s3] ──► [vmbr0 橋接] ──► [VM1] [VM2] [VM3]
```

```bash
# 查看網路設定
cat /etc/network/interfaces
```

## 概念四：節點與集群

- **單節點**：一台 PVE 主機，適合入門
- **集群**：多台節點組成，配置透過 pmxcfs 即時同步，可線上遷移、HA

```
[節點1] ◄── Corosync ──► [節點2] ◄──► [節點3]
   │                         │
   └── 共用配置 (pmxcfs) ─────┘
```

> 💡 關鍵洞察：集群無主從之分，任一節點都可管理整個集群。

## 概念關係圖

```
存儲 ──► 提供磁碟給 ──► VM/容器
  │
  └── 共享存儲 ──► 支援 ──► 線上遷移、HA

網路(vmbr0) ──► 連接 ──► VM/容器
```

## 下一步

日常操作：備份、遷移、克隆 → [常見模式](03-common-patterns.md)
