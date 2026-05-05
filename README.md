# Infrastructure Knowledge Base (Home Lab)

このリポジトリは、検証環境におけるインフラ構成、設計、およびトラブルシューティングのナレッジを蓄積するためのものです。

## 🏗 システム全体図
検証環境の論理構成および物理配線。
<!--  -->

## 🌐 ネットワーク設計

### IPアドレス管理 (IPAM)
ネットワーク帯域は `1024` ベースのバイナリ計算で算出。

| 用途 | VLAN | サブネット | ゲートウェイ | 備考 |
| :--- | :--- | :--- | :--- | :--- |
| Management | 10 | 192.168.10.0/24 | .1 | ESXi/vCenter |
| Server-Segment | 20 | 192.168.20.0/24 | .1 | Linux / Windows |
| Storage (iSCSI) | 30 | 192.168.30.0/24 | N/A | Non-routed |

### ルーティング・ACL
*   **Router:** IXシリーズ / Linux Router
*   **Policy:** インターネット境界でのプロキシ経由のアクセス強制。

## 💻 仮想化基盤
*   **Hypervisor:** AlmaLinux (KVM) / Proxmox / Nutanix Community Edition
*   **Resources:**
    *   CPU: 32 vCPUs
    *   RAM: 128 GB
    *   Disk: NVMe RAID 1 (System), SATA SSD (Data)

## 🛠 構築・設定手順
各コンポーネントの詳細設定は `/docs` 配下を参照。
* [AlmaLinux 基本セットアップ](./docs/os/almalinux-setup.md)
* [Nutanix クラスタ構築手順](./docs/infra/nutanix-cluster.md)
* [Zabbix 監視テンプレート](./docs/monitor/zabbix-template.md)

## 📝 トラブルシューティング・備忘録
*   **2026-05-05:** ◯◯設定時にサブネットマスクの計算ミスによる通信不具合発生。/19等の計算には注意。
*   **リンク:** [検証済み不具合リスト](./docs/troubleshoot/log.md)

---
© 2026 Your Name / Infrastructure Engineer
