# ネットワーク
<img width="784" height="360" alt="image" src="https://github.com/user-attachments/assets/3acd93be-1707-4a8c-b9b5-92257503031e" />

Bridge：vSphereでいうポートグループ。VMに紐づけるオブジェクト

Bond：物理NICをボンディングで冗長化するためのインターフェース

VLAN：IP設定ができ、するとvmkernelみたいなことができる。しないと普通のポートグループみたいなことができる。ただ全部VSTみたいな挙動になるため、VLANトランクは使用不可。トランク使うならブリッジのみ。
VLANとVMは紐づけられないため、Bridgeを間に挟む必要がある
