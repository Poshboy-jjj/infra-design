# Ansibleサーバ詳細設計

構成管理サーバ

## OS設定
### ホスト名
ansible

### OS
Almalinux 9.6(5.14.0-570.12.1.el9_6.x86_64)

### パーティション構成
| デバイス | サイズ | マウントポイント | ファイルシステム | 備考 |
| :--- | :--- | :--- | :--- | :--- |
| /dev/sda1 | 600M | /boot/efi | vfat ||
| /dev/sda2 | 1G | /boot | xfs ||
| /dev/mapper/almalinux-root | 26.8G | / | xfs |sda3|
| /dev/mapper/almalinux-swap | 1.6G | - | swap |sda3|

### ネットワーク
| IPアドレス(CIDR表記) | デフォルトゲートウェイ | DNSサーバ | VLAN | その他 |
| :--- | :--- | :--- | :--- | :--- |
|192.168.100.111/24|192.168.100.254||100||

### SELinux
無効 
### Firewalld
無効

### 管理アカウント
| ユーザー名 | グループ | 役割 |
| :--- | :--- | :--- |
| root | - | システム管理（非推奨作業用） |
| dadmin | wheel | **共通管理ユーザー** (sudo NOPASSWD設定) |

## Github設定
