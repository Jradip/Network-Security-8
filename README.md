# Network-Security-8

#VLAN 10 Dosen
Router-Core(config)# interface g0/1.10
Router-Core(config-subif)# encapsulation dot1Q 10
Router-Core(config-subif)# ip address 192.168.10.1 255.255.255.0
Router-Core(config-subif)# exit

#VLAN 20 MAHASISWA
Router-Core(config)# interface g0/1.20
Router-Core(config-subif)# encapsulation dot1Q 20
Router-Core(config-subif)# ip address 192.168.20.1 255.255.255.0
Router-Core(config-subif)# exit

#VLAN Guest
Router-Core(config)# interface g0/2
Router-Core(config-if)# ip address 192.168.30.1 255.255.255.0
Router-Core(config-if)# no shutdown
Router-Core(config-if)# exit

#VLAN Switch
Switch1# configure terminal
Switch1(config)# vlan 10
Switch1(config-vlan)# name DOSEN
Switch1(config-vlan)# exit

Switch1(config)# vlan 20
Switch1(config-vlan)# name MAHASISWA
Switch1(config-vlan)# exit

#Port access
Switch1(config)# interface fa0/1
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
Switch1(config-if)# exit

Switch1(config)# interface fa0/2
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 20
Switch1(config-if)# exit

#Trunk
Switch1(config)# interface fa0/24
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# exit
Switch1(config)# spanning-tree mode pvst

#Excluded
Router-Core(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10
Router-Core(config)# ip dhcp excluded-address 192.168.20.1 192.168.20.10
Router-Core(config)# ip dhcp excluded-address 192.168.30.1 192.168.30.10

#Pool Dhcp Dosen
Router-Core(config)# ip dhcp pool DOSEN
Router-Core(dhcp-config)# network 192.168.10.0 255.255.255.0
Router-Core(dhcp-config)# default-router 192.168.10.1
Router-Core(dhcp-config)# dns-server 172.18.18.10
Router-Core(dhcp-config)# exit

#Pool dhcp Mahasiswa
Router-Core(config)# ip dhcp pool MAHASISWA
Router-Core(dhcp-config)# network 192.168.20.0 255.255.255.0
Router-Core(dhcp-config)# default-router 192.168.20.1
Router-Core(dhcp-config)# dns-server 172.18.18.10
Router-Core(dhcp-config)# exit

#Pool dhcp guest
Router-Core(config)# ip dhcp pool GUEST
Router-Core(dhcp-config)# network 192.168.30.0 255.255.255.0
Router-Core(dhcp-config)# default-router 192.168.30.1
Router-Core(dhcp-config)# dns-server 172.18.18.10
Router-Core(dhcp-config)# exit

#ACL Dosen
Router-Core(config)# access-list 100 permit udp any eq 68 any eq 67
Router-Core(config)# access-list 100 permit udp any eq 67 any eq 68
Router-Core(config)# access-list 100 permit ip 192.168.10.0 0.0.0.255 any

#ACL Mahasiswa
Router-Core(config)# access-list 110 permit udp any eq 68 any eq 67
Router-Core(config)# access-list 110 permit udp any eq 67 any eq 68
Router-Core(config)# access-list 110 permit ip 192.168.20.0 0.0.0.255 host 172.16.100.10
Router-Core(config)# access-list 110 deny icmp any host 8.8.8.8

#ACL Guest
Router-Core(config)# access-list 120 permit udp any eq 68 any eq 67
Router-Core(config)# access-list 120 permit udp any eq 67 any eq 68
Router-Core(config)# access-list 120 permit tcp 192.168.30.0 0.0.0.255 host 8.8.8.8 eq 80
Router-Core(config)# access-list 120 permit tcp 192.168.30.0 0.0.0.255 host 8.8.8.8 eq 443
Router-Core(config)# access-list 120 deny icmp any host 172.16.100.10

#ACL Interface
Router-Core(config)# interface g0/1.10
Router-Core(config-if)# ip access-group 100 in
Router-Core(config-if)# exit

Router-Core(config)# interface g0/1.20
Router-Core(config-if)# ip access-group 110 in
Router-Core(config-if)# exit

Router-Core(config)# interface g0/2
Router-Core(config-if)# ip access-group 120 in
Router-Core(config-if)# exit
