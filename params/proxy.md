# Proxyサーバ詳細設計

各サーバがインターネットへ接続するためのProxyサーバ

## OS設定
### ホスト名
proxy

### OS
Almalinux 9.6(5.14.0-570.12.1.el9_6.x86_64)

### スペック
| CPU | メモリ | ディスク1 | ディスク2 |
| :--- | :--- | :--- | :--- |
|1core|2GB|30GB|10GB|

### パーティション構成
| デバイス | サイズ | マウントポイント | ファイルシステム | 備考 |
| :--- | :--- | :--- | :--- | :--- |
| /dev/sda1 | 600M | /boot/efi | vfat ||
| /dev/sda2 | 1G | /boot | xfs ||
| /dev/mapper/almalinux-root | 26.8G | / | xfs |sda3|
| /dev/mapper/almalinux-swap | 1.6G | - | swap |sda3|
| /dev/mapper/vg01-lv01 | 10G | /var/spool/squid | xfs |sdb1

### ネットワーク
| IPアドレス(CIDR表記) | デフォルトゲートウェイ | DNSサーバ | VLAN | その他 |
| :--- | :--- | :--- | :--- | :--- |
|192.168.66.124/24|192.168.66.254|8.8.8.8|66||

### SELinux
無効 
### Firewalld
無効

### 管理アカウント
| ユーザー名 | グループ | 役割 |
| :--- | :--- | :--- |
| root | - | システム管理（非推奨作業用） |
| dadmin | wheel | **共通管理ユーザー** (sudo NOPASSWD設定) |

## Squid設定
### サーバ側
- **主な設定**:
  - http_port 8080  #(クライアント側が指定するHTTP Proxyポート)
  - acl localnet src 192.168.0.0/16         # (受け付けるネットワーク)
  - acl SSL_ports port 443
  - acl Safe_ports port 80          # http
  - acl Safe_ports port 443         # https
  - cache_dir aufs /var/spool/squid 9000 16 256  #(キャッシュサイズ最大9000MB)
### クライアント側
#### dnf プロキシ設定 (`/etc/dnf/dnf.conf`)
```ini
proxy=http://192.168.66.124:8080
```

#### 環境変数 プロキシ設定(`/etc/profile.d/proxy.sh`)
```ini
export http_proxy=http://192.168.66.124:8080
export https_proxy=http://192.168.66.124:8080
export no_proxy=localhost,127.0.0.1,192.168.0.0/16
```





