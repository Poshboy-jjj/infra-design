# NEC IX詳細設計

# 構成
Using 2539 out of 1048576 bytes

! NEC Portable Internetwork Core Operating System Software
! IX Series IX2215 (magellan-sec) Software, Version 10.11.6, RELEASE SOFTWARE
! Compiled May 22-Thu-2025 14:55:23 JST #2
! Last updated Mar 28-Sat-2026 08:41:58 JST
!
!
!
!
timezone +09 00
!
username admin password hash a2A01EDed3A7a12dc45bfF7fbb179B@ administrator
!
!
ip access-list vlan-isolation permit ip src 192.168.100.0/24 dest 192.168.100.0/24
ip access-list vlan-isolation permit ip src 192.168.99.0/24 dest 192.168.99.0/24
ip access-list vlan-isolation permit ip src 192.168.66.0/24 dest 192.168.66.0/24
ip access-list vlan-isolation permit tcp src any sport any dest 192.168.66.124/32 dport eq 8080
ip access-list vlan-isolation permit tcp src 192.168.66.124/32 sport eq 8080 dest any dport any
ip access-list vlan-isolation permit ip src 192.168.100.101/32 dest 192.168.66.0/24
ip access-list vlan-isolation permit ip src 192.168.66.0/24 dest 192.168.100.101/32
ip access-list vlan-isolation deny ip src any dest 192.168.0.0/16
ip access-list vlan-isolation permit ip src any dest any
ip access-list vlan66-only permit ip src 192.168.66.0/24 dest any
!
!
!
!
!
!
!
!
!
!
!
!
ssh-server ip enable
!
!
!
device GigaEthernet0
!
device GigaEthernet1
!
device GigaEthernet2
  vlan-group 1 port 1 2 3 4 5 6 7 8
!
device BRI0
  isdn switch-type hsd128k
!
device USB0
  shutdown
!
interface GigaEthernet0.0
  ip address dhcp receive-default
  ip napt enable
  ip napt inside list vlan66-only
  no shutdown
!
interface GigaEthernet1.0
  no ip address
  shutdown
!
interface GigaEthernet2.0
  no ip address
  shutdown
!
interface BRI0.0
  encapsulation ppp
  no auto-connect
  no ip address
  shutdown
!
interface USB-Serial0.0
  encapsulation ppp
  no auto-connect
  no ip address
  shutdown
!
interface GigaEthernet2:1.0
  no ip address
  shutdown
!
interface GigaEthernet2:1.1
  description vlan100
  encapsulation dot1q 100 tpid 8100
  auto-connect
  ip address 192.168.100.254/24
  ip filter vlan-isolation 1 in
  no shutdown
!
interface GigaEthernet2:1.2
  description vlan99
  encapsulation dot1q 99 tpid 8100
  auto-connect
  ip address 192.168.99.254/24
  ip filter vlan-isolation 1 in
  no shutdown
!
interface GigaEthernet2:1.3
  description vlan66
  encapsulation dot1q 66 tpid 8100
  auto-connect
  ip address 192.168.66.254/24
  ip filter vlan-isolation 1 in
  no shutdown
!
interface Loopback0.0
  no ip address
!
interface Null0.0
  no ip address
!
<img width="771" height="2395" alt="image" src="https://github.com/user-attachments/assets/1564babe-aeb0-4f29-bd5a-7ed7c96f197a" />

