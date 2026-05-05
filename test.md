graph TD
    %% クラウド/外部サービス
    Internet((Internet)) --- BBR[Broadband Router]
    
    subgraph IX2215["NEC IX2215 (L3 Routing Only)"]
        BBR --- GE0[GE0.0: DHCP/NAPT]
        GE0 --- ACL[ACL vlan-isolation: Limited]
        
        %% IX2215側のVLAN GW
        ACL --- GW66[GW .254]
        ACL --- GW100[GW .254]
    end

    %% 物理セグメント (Tagged VLAN)
    IX2215 === L2SW[L2 Switch (PVID Trunk)]
    L2SW === HYP_LAN[Hypervisor LAN NICs]

    subgraph Hypervisor["VM Host (KVM/ESXi)"]
        HYP_LAN --- |VLAN 66 Trunk| vSw66[vSwitch (VLAN 66)]
        HYP_LAN --- |VLAN 100 Trunk| vSw100[vSwitch (VLAN 100)]
        
        %% --- 仮想ファイアウォール (VyOS) ---
        subgraph VyOS["VyOS (Bridge FW)"]
            eth0[eth0: vBridge0]
            eth1[eth1: vBridge0]
            FW_RULES{"Stateful<br/>Inspection<br/>ZBF"}
        end
        
        vSw66 --- eth0
        FW_RULES --- eth0
        FW_RULES --- eth1
        eth1 --- vSw100
        
        %% --- 各セグメントのVM ---
        subgraph Seg_External["VLAN 66 (External Segment: 192.168.66.0/24)"]
            VM66_Proxy[VM: Proxy]
            VM66_Postfix[VM: Postfix]
            VM66_VPN[VM: VPN <br/>(WireGuard/Tailscale)]
        end
        vSw66 --- VM66_Proxy
        vSw66 --- VM66_Postfix
        vSw66 --- VM66_VPN
        
        subgraph Seg_Internal["VLAN 100 (Internal Segment: 192.168.100.0/24)"]
            VM100_Ansible[VM: Ansible]
            VM100_Zabbix[VM: Zabbix]
            VM100_HBLab[VM: HB Lab]
        end
        vSw100 --- VM100_Ansible
        vSw100 --- VM100_Zabbix
        vSw100 --- VM100_HBLab
    end

    %% 管理用VPN
    VM66_VPN -.-> |VPN Connection| AdminPC[Admin端末]
