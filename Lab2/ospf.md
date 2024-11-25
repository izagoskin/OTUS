# Underlay ospf
![alt](https://github.com/izagoskin/OTUS/blob/c0bf1a5d016c42fa695e83bd4ad5e021a7351006/Lab2/schema.jpg "Схема")

hostname Leaf-3
spanning-tree mode mstp
interface Ethernet1
   no switchport
   ip address 10.0.1.10/30
interface Ethernet2
   no switchport
   ip address 10.0.2.10/30
interface Ethernet3
   no switchport
   ip address 10.0.3.10/30
interface Ethernet4
interface Ethernet5
interface Loopback0
   ip address 172.16.255.23/32
interface Loopback1
   ip address 172.16.254.23/32
interface Management1
ip routing
router ospf 1
   router-id 172.16.255.23
   passive-interface default
   no passive-interface Ethernet1
   no passive-interface Ethernet2
   no passive-interface Ethernet3
   no passive-interface Loopback0
   no passive-interface Loopback1
   network 10.0.0.0/16 area 0.0.0.0
   network 172.16.254.0/24 area 0.0.0.0
   network 172.16.255.0/24 area 0.0.0.0
   max-lsa 12000
end
Leaf-3#
