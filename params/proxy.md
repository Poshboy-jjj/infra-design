# Proxyサーバ詳細設計 (squid-server)

各サーバがインターネットへ接続するためのProxyサーバ

## 基本情報
| 項目 | 設定値 | 備考 |
| :--- | :--- | :--- |
| ホスト名 | proxy | |
| OS | AlmaLinux 9.6 | |
| IPアドレス | 192.168.66.124 | VLAN66 |
| プロキシポート | 8080 | |

## 管理アカウント
| ユーザー名 | グループ | 役割 |
| :--- | :--- | :--- |
| root | - | システム管理（非推奨作業用） |
| dadmin | wheel | **共通管理ユーザー** (sudo NOPASSWD設定) |

## ソフトウェア構成
### Squid (Proxy Service)
- **バージョン**: dnf標準
- **主な設定**:
  - http_port 8080  #(クライアント側が指定するHTTP Proxyポート)
  - acl localnet src 192.168.0.0/16         # (受け付けるネットワーク)
  - acl SSL_ports port 443
  - acl Safe_ports port 80          # http
  - acl Safe_ports port 443         # https
  - cache_dir aufs /var/spool/squid 9000 16 256  #(キャッシュサイズ最大9000MB)
 
## サーバ固有設定
### SELinux
無効 
### Firewalld
無効
### パーティション構成
| デバイス | サイズ | マウントポイント | ファイルシステム | 備考 |
| :--- | :--- | :--- | :--- ||
| /dev/sda1 | 600M | /boot/efi | vfat ||
| /dev/sda2 | 1G | /boot | xfs ||
| /dev/mapper/almalinux-root | 26.8G | / | xfs |sda3|
| /dev/mapper/almalinux-swap | 1.6G | - | swap |sda3|
| /dev/mapper/vg01-lv01 | 10G | /var/spool/squid | xfs |sdb1


## 関連ホスト設定（接続元）
各クライアント側の設定

### dnf プロキシ設定 (`/etc/dnf/dnf.conf`)
```ini
proxy=http://192.168.66.124:8080
```

### 環境変数 プロキシ設定(`/etc/profile.d/proxy.sh`)
```ini
export http_proxy=http://192.168.66.124:8080
export https_proxy=http://192.168.66.124:8080
export no_proxy=localhost,127.0.0.1,192.168.0.0/16
```
