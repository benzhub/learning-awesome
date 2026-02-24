# Proxmox VE — 開源虛擬化平台

## 這是什麼

Proxmox VE（Proxmox Virtual Environment）是基於 Debian 的開源虛擬化平台，整合 **KVM 虛擬機** 與 **LXC 容器**，提供 Web 管理介面、集群、備份與 HA 高可用。

與 VMware ESXi、Hyper-V 相比：完全開源、無授權費用、支援超融合（Ceph、ZFS），適合自建伺服器與中小型企業。

## 適用場景

- ✅ 適合：自建 NAS、家庭實驗室、開發測試環境
- ✅ 適合：中小企業虛擬化、私有雲部署
- ✅ 適合：需要 HA、備份、集群的生產環境
- ❌ 不適合：僅需單一輕量容器（可考慮 Docker）

## 學習地圖

1. [快速上手](01-quick-start.md) — 安裝並建立第一台虛擬機
2. [核心概念](02-core-concepts.md) — VM、容器、存儲、集群
3. [常見模式](03-common-patterns.md) — 備份、遷移、克隆
4. [進階用法](04-advanced.md) — 集群、HA、Ceph
5. [最佳實踐](05-best-practices.md) — 避坑與優化

## 安裝

```bash
# 1. 下載 ISO：https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso
# 2. 製作 USB 開機碟（Linux 範例）
dd bs=1M conv=fdatasync if=./proxmox-ve_*.iso of=/dev/sdX

# 3. 從 USB 開機，依安裝精靈完成設定
# 4. 安裝完成後訪問 https://<IP>:8006
```

> ⚠️ 安裝會覆寫所選磁碟，請先備份重要資料。

## 延伸閱讀

- [Proxmox VE 官方文件](https://pve.proxmox.com/pve-docs/pve-admin-guide.html)
- [PVE 中文手冊](https://pve-doc-cn.readthedocs.io/zh-cn/latest/)
