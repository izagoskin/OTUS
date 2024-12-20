## eBGP Underlay
Вводные: 2 спайна, 3 лифа, eBGP underlay
![alt](https://github.com/izagoskin/OTUS/blob/c0bf1a5d016c42fa695e83bd4ad5e021a7351006/Lab2/schema.jpg "Схема")
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

Leaf-1#ping 172.16.255.1
PING 172.16.255.1 (172.16.255.1) 72(100) bytes of data.
80 bytes from 172.16.255.1: icmp_seq=1 ttl=64 time=4.55 ms
80 bytes from 172.16.255.1: icmp_seq=2 ttl=64 time=3.13 ms
80 bytes from 172.16.255.1: icmp_seq=3 ttl=64 time=3.49 ms
80 bytes from 172.16.255.1: icmp_seq=4 ttl=64 time=3.85 ms
80 bytes from 172.16.255.1: icmp_seq=5 ttl=64 time=3.76 ms

Leaf-1#ping 172.16.255.2
PING 172.16.255.2 (172.16.255.2) 72(100) bytes of data.
80 bytes from 172.16.255.2: icmp_seq=1 ttl=64 time=6.38 ms
80 bytes from 172.16.255.2: icmp_seq=2 ttl=64 time=3.02 ms
80 bytes from 172.16.255.2: icmp_seq=3 ttl=64 time=3.46 ms
80 bytes from 172.16.255.2: icmp_seq=4 ttl=64 time=2.86 ms
80 bytes from 172.16.255.2: icmp_seq=5 ttl=64 time=3.37 ms

```
### Leaf-2
```
hostname Leaf-2
interface Ethernet1
   no switchport
   ip address 10.0.1.6/30
interface Ethernet2
   no switchport
   ip address 10.0.2.6/30
interface Ethernet3
   no switchport
   ip address 10.0.3.6/30
interface Loopback0
   ip address 172.16.255.22/32

router bgp 65022
   neighbor 10.0.1.5 remote-as 65001
   neighbor 10.0.1.5 update-source Ethernet1
   neighbor 10.0.2.5 remote-as 65002
   neighbor 10.0.2.5 update-source Ethernet2
   address-family ipv4
      neighbor 10.0.1.5 activate
      neighbor 10.0.2.5 activate
      network 10.0.1.4/30
      network 172.16.255.22/32

Leaf-2#sh ip route
 C        10.0.1.4/30 is directly connected, Ethernet1
 C        10.0.2.4/30 is directly connected, Ethernet2
 C        10.0.3.4/30 is directly connected, Ethernet3
 C        172.16.254.22/32 is directly connected, Loopback1
 B E      172.16.255.1/32 [200/0] via 10.0.1.5, Ethernet1
 B E      172.16.255.2/32 [200/0] via 10.0.2.5, Ethernet2
 B E      172.16.255.21/32 [200/0] via 10.0.1.5, Ethernet1
 C        172.16.255.22/32 is directly connected, Loopback0
 B E      172.16.255.23/32 [200/0] via 10.0.1.5, Ethernet1

Leaf-2#ping 172.16.255.1
PING 172.16.255.1 (172.16.255.1) 72(100) bytes of data.
80 bytes from 172.16.255.1: icmp_seq=1 ttl=64 time=5.21 ms
80 bytes from 172.16.255.1: icmp_seq=2 ttl=64 time=3.41 ms
80 bytes from 172.16.255.1: icmp_seq=3 ttl=64 time=3.03 ms
80 bytes from 172.16.255.1: icmp_seq=4 ttl=64 time=2.66 ms
80 bytes from 172.16.255.1: icmp_seq=5 ttl=64 time=2.52 ms

Leaf-2#ping 172.16.255.2
PING 172.16.255.2 (172.16.255.2) 72(100) bytes of data.
80 bytes from 172.16.255.2: icmp_seq=1 ttl=64 time=5.49 ms
80 bytes from 172.16.255.2: icmp_seq=2 ttl=64 time=4.22 ms
80 bytes from 172.16.255.2: icmp_seq=3 ttl=64 time=2.87 ms
80 bytes from 172.16.255.2: icmp_seq=4 ttl=64 time=2.96 ms
80 bytes from 172.16.255.2: icmp_seq=5 ttl=64 time=3.10 ms
```
### Leaf-3
```
hostname Leaf-3
interface Ethernet1
   no switchport
   ip address 10.0.1.10/30
interface Ethernet2
   no switchport
   ip address 10.0.2.10/30
interface Ethernet3
   no switchport
   ip address 10.0.3.10/30
interface Loopback0
   ip address 172.16.255.23/32

router bgp 65023
   neighbor 10.0.1.9 remote-as 65001
   neighbor 10.0.1.9 update-source Ethernet1
   neighbor 10.0.2.9 remote-as 65002
   neighbor 10.0.2.9 update-source Ethernet2
   address-family ipv4
      neighbor 10.0.1.9 activate
      neighbor 10.0.2.9 activate
      network 172.16.255.23/32

Leaf-3#sh ip route
 B E      10.0.1.4/30 [200/0] via 10.0.2.9, Ethernet2
 C        10.0.1.8/30 is directly connected, Ethernet1
 C        10.0.2.8/30 is directly connected, Ethernet2
 C        10.0.3.8/30 is directly connected, Ethernet3
 C        172.16.254.23/32 is directly connected, Loopback1
 B E      172.16.255.1/32 [200/0] via 10.0.1.9, Ethernet1
 B E      172.16.255.2/32 [200/0] via 10.0.2.9, Ethernet2
 B E      172.16.255.21/32 [200/0] via 10.0.1.9, Ethernet1
 B E      172.16.255.22/32 [200/0] via 10.0.1.9, Ethernet1
 C        172.16.255.23/32 is directly connected, Loopback0

Leaf-3#ping  172.16.255.1
PING 172.16.255.1 (172.16.255.1) 72(100) bytes of data.
80 bytes from 172.16.255.1: icmp_seq=1 ttl=64 time=5.45 ms
80 bytes from 172.16.255.1: icmp_seq=2 ttl=64 time=3.74 ms
80 bytes from 172.16.255.1: icmp_seq=3 ttl=64 time=2.80 ms
80 bytes from 172.16.255.1: icmp_seq=4 ttl=64 time=3.19 ms
80 bytes from 172.16.255.1: icmp_seq=5 ttl=64 time=3.49 ms

Leaf-3#ping  172.16.255.2
PING 172.16.255.2 (172.16.255.2) 72(100) bytes of data.
80 bytes from 172.16.255.2: icmp_seq=1 ttl=64 time=5.17 ms
80 bytes from 172.16.255.2: icmp_seq=2 ttl=64 time=3.04 ms
80 bytes from 172.16.255.2: icmp_seq=3 ttl=64 time=3.05 ms
80 bytes from 172.16.255.2: icmp_seq=4 ttl=64 time=3.15 ms
80 bytes from 172.16.255.2: icmp_seq=5 ttl=64 time=3.07 ms
```
### Spine-1
```
hostname Spine-1
interface Ethernet1
   no switchport
   ip address 10.0.1.1/30
interface Ethernet2
   no switchport
   ip address 10.0.1.5/30
interface Ethernet3
   no switchport
   ip address 10.0.1.9/30
interface Loopback0
   ip address 172.16.255.1/32
   isis enable underlay

router bgp 65001
   neighbor 10.0.1.2 remote-as 65021
   neighbor 10.0.1.6 remote-as 65022
   neighbor 10.0.1.10 remote-as 65023
   address-family ipv4
      neighbor 10.0.1.2 activate
      neighbor 10.0.1.6 activate
      neighbor 10.0.1.10 activate
      network 172.16.255.1/32

Spine-1#sh ip route
 C        10.0.1.0/30 is directly connected, Ethernet1
 C        10.0.1.4/30 is directly connected, Ethernet2
 C        10.0.1.8/30 is directly connected, Ethernet3
 C        172.16.254.1/32 is directly connected, Loopback1
 C        172.16.255.1/32 is directly connected, Loopback0
 B E      172.16.255.2/32 [200/0] via 10.0.1.2, Ethernet1
 B E      172.16.255.21/32 [200/0] via 10.0.1.2, Ethernet1
 B E      172.16.255.22/32 [200/0] via 10.0.1.6, Ethernet2
 B E      172.16.255.23/32 [200/0] via 10.0.1.10, Ethernet3
```
### Spine-2
```
hostname Spine-2
interface Ethernet1
   no switchport
   ip address 10.0.2.1/30
interface Ethernet2
   no switchport
   ip address 10.0.2.5/30
interface Ethernet3
   no switchport
   ip address 10.0.2.9/30
interface Loopback0
   ip address 172.16.255.2/32

router bgp 65002
   neighbor 10.0.2.2 remote-as 65021
   neighbor 10.0.2.2 update-source Ethernet1
   neighbor 10.0.2.6 remote-as 65022
   neighbor 10.0.2.6 update-source Ethernet2
   neighbor 10.0.2.10 remote-as 65023
   neighbor 10.0.2.10 update-source Ethernet3
   !
   address-family ipv4
      neighbor 10.0.2.2 activate
      neighbor 10.0.2.6 activate
      neighbor 10.0.2.10 activate
      network 172.16.255.2/32

Spine-2#sh ip route
 B E      10.0.1.4/30 [200/0] via 10.0.2.6, Ethernet2
 C        10.0.2.0/30 is directly connected, Ethernet1
 C        10.0.2.4/30 is directly connected, Ethernet2
 C        10.0.2.8/30 is directly connected, Ethernet3
 C        172.16.254.2/32 is directly connected, Loopback1
 B E      172.16.255.1/32 [200/0] via 10.0.2.2, Ethernet1
 C        172.16.255.2/32 is directly connected, Loopback0
 B E      172.16.255.21/32 [200/0] via 10.0.2.2, Ethernet1
 B E      172.16.255.22/32 [200/0] via 10.0.2.6, Ethernet2
 B E      172.16.255.23/32 [200/0] via 10.0.2.10, Ethernet3
```
