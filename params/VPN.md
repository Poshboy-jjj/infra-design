# 概要
出先Windows端末と検証環境を接続するために、TaileScaleを用いてVPNを構成する
VPNサーバおよび出先Windows端末にTaileScaleをインストールする

# VPNサーバ設定
## TaileScale
<img width="495" height="514" alt="image" src="https://github.com/user-attachments/assets/dd973123-e2cf-4b69-a4f5-02579c5210f6" />

## VPN
```bash
[root@vpn ~]# cat /etc/sysctl.d/99-tailscale.conf
net.ipv4.ip_forward = 1
[root@vpn ~]#
```
