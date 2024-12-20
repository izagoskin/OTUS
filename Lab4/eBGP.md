## eBGP Underlay
Вводные: 2 спайна, 3 лифа, eBGP underlay

### Leaf-1
```
hostname Leaf-1
interface Ethernet1
   no switchport
   ip address 10.0.1.2/30
interface Ethernet2
   no switchport
   ip address 10.0.2.2/30
interface Ethernet3
   no switchport
   ip address 10.0.3.2/30
interface Loopback0
   ip address 172.16.255.21/32

router bgp 65021
   neighbor 10.0.1.1 remote-as 65001
   neighbor 10.0.1.1 update-source Ethernet1
   neighbor 10.0.2.1 remote-as 65002
   neighbor 10.0.2.1 update-source Ethernet2
   address-family ipv4
      neighbor 10.0.1.1 activate
      neighbor 10.0.2.1 activate
      network 172.16.255.21/32

Leaf-1#sh ip route
 C        10.0.1.0/30 is directly connected, Ethernet1
 C        10.0.2.0/30 is directly connected, Ethernet2
 C        10.0.3.0/30 is directly connected, Ethernet3
 B E      172.16.255.1/32 [200/0] via 10.0.1.1, Ethernet1
 B E      172.16.255.2/32 [200/0] via 10.0.2.1, Ethernet2
 C        172.16.255.21/32 is directly connected, Loopback0
 B E      172.16.255.22/32 [200/0] via 10.0.1.1, Ethernet1
 B E      172.16.255.23/32 [200/0] via 10.0.1.1, Ethernet1
```
### Leaf-2

### Leaf-3

### Spine-1

### Spine-2
