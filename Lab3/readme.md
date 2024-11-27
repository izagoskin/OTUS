# IS-IS
## Схема сети
![alt](https://github.com/izagoskin/OTUS/blob/c0bf1a5d016c42fa695e83bd4ad5e021a7351006/Lab2/schema.jpg "Схема")

|устройство | адрес|
|-----------|------|
| Spine-1   | 10.0000.0000.0001.0000.00|
| Spine-2   | 10.0000.0000.0002.0000.00|
| Spine-3   | 10.0000.0000.0003.0000.00|
| Leaf-1    | 10.0000.0000.0000.0001.00|
| Leaf-2    | 10.0000.0000.0000.0002.00|
| Leaf-3    | 10.0000.0000.0000.0003.00|

### Spine-1 config
``` interface Ethernet1
  no switchport
   ip address 10.0.1.1/30
   isis enable underlay
   isis circuit-type level-2
interface Ethernet2
   no switchport
   ip address 10.0.1.5/30
   isis enable underlay
   isis circuit-type level-2
interface Ethernet3
   no switchport
   ip address 10.0.1.9/30
   isis enable underlay
   isis circuit-type level-2
interface Loopback0
   ip address 172.16.255.1/32
   isis enable underlay
interface Loopback1
   ip address 172.16.254.1/32
   isis enable underlay
ip routing
router isis underlay
   net 10.0000.0000.0001.0000.00
   is-hostname Spine-1
   router-id ipv4 172.16.255.1
   is-type level-2
   address-family ipv4 unicast
  ```

### Spine-2 config
```
interface Ethernet1
   no switchport
   ip address 10.0.2.1/30
   isis enable underlay
   isis circuit-type level-2
interface Ethernet2
   no switchport
   ip address 10.0.2.5/30
   isis enable underlay
   isis circuit-type level-2
interface Ethernet3
   no switchport
   ip address 10.0.2.9/30
   isis enable underlay
   isis circuit-type level-2
interface Loopback0
   ip address 172.16.255.2/32
   isis enable underlay
interface Loopback1
   ip address 172.16.254.2/32
   isis enable underlay
ip routing
router isis underlay
   net 10.0000.0000.0002.0000.00
   is-hostname Spine-2
   router-id ipv4 172.16.255.2
   is-type level-2
   address-family ipv4 unicast
```

### Spine-3
```
interface Ethernet1
   no switchport
   ip address 10.0.3.1/30
   isis enable underlay
   isis circuit-type level-2
interface Ethernet2
   no switchport
   ip address 10.0.3.5/30
   isis enable underlay
   isis circuit-type level-2
interface Ethernet3
   no switchport
   ip address 10.0.3.9/30
   isis enable underlay
   isis circuit-type level-2
interface Loopback0
   ip address 172.16.255.3/32
   isis enable underlay
interface Loopback1
   ip address 172.16.254.3/32
   isis enable underlay
ip routing
router isis underlay
   net 10.0000.0000.0003.0000.00
   is-hostname Spine-3
   router-id ipv4 172.16.255.3
   is-type level-2
   address-family ipv4 unicast
```

### Leaf-1
```
interface Ethernet1
   no switchport
   ip address 10.0.1.2/30
   isis enable underlay
interface Ethernet2
   no switchport
   ip address 10.0.2.2/30
   isis enable underlay
   isis circuit-type level-2
interface Ethernet3
   no switchport
   ip address 10.0.3.2/30
   isis enable underlay
   isis circuit-type level-2
interface Loopback0
   ip address 172.16.255.21/32
   isis enable underlay
interface Loopback1
   ip address 172.16.254.21/32
   isis enable underlay
ip routing
router isis underlay
   net 10.0000.0000.0000.0001.00
   is-hostname Leaf-1
   router-id ipv4 172.16.255.21
   is-type level-2
   address-family ipv4 unicast
```

### Leaf-2
```
interface Ethernet1
   no switchport
   ip address 10.0.1.6/30
   isis enable underlay
   isis circuit-type level-2
interface Ethernet2
   no switchport
   ip address 10.0.2.6/30
   isis enable underlay
   isis circuit-type level-2
interface Ethernet3
   no switchport
   ip address 10.0.3.6/30
   isis enable underlay
   isis circuit-type level-2
interface Loopback0
   ip address 172.16.255.22/32
   isis enable underlay
interface Loopback1
   ip address 172.16.254.22/32
   isis enable underlay
ip routing
router isis underlay
   net 10.0000.0000.0000.0002.00
   is-hostname Leaf-2
   router-id ipv4 172.16.255.22
   is-type level-2
   address-family ipv4 unicast
```

### Leaf-3
```
interface Ethernet1
   no switchport
   ip address 10.0.1.10/30
   isis enable underlay
   isis circuit-type level-2
interface Ethernet2
   no switchport
   ip address 10.0.2.10/30
   isis enable underlay
   isis circuit-type level-2
interface Ethernet3
   no switchport
   ip address 10.0.3.10/30
   isis enable underlay
   isis circuit-type level-2
interface Loopback0
   ip address 172.16.255.23/32
   isis enable underlay
interface Loopback1
   ip address 172.16.254.23/32
   isis enable underlay
ip routing
router isis underlay
   net 10.0000.0000.0000.0003.00
   is-hostname Leaf-3
   router-id ipv4 172.16.255.23
   is-type level-2
   address-family ipv4 unicast
```
### Таблицы маршрутизации
#### Spine-1
``` Gateway of last resort is not set
 C        10.0.1.0/30 is directly connected, Ethernet1
 C        10.0.1.4/30 is directly connected, Ethernet2
 C        10.0.1.8/30 is directly connected, Ethernet3
 I L2     10.0.2.0/30 [115/20] via 10.0.1.2, Ethernet1
 I L2     10.0.2.4/30 [115/20] via 10.0.1.6, Ethernet2
 I L2     10.0.2.8/30 [115/20] via 10.0.1.10, Ethernet3
 I L2     10.0.3.0/30 [115/20] via 10.0.1.2, Ethernet1
 I L2     10.0.3.4/30 [115/20] via 10.0.1.6, Ethernet2
 I L2     10.0.3.8/30 [115/20] via 10.0.1.10, Ethernet3
 C        172.16.254.1/32 is directly connected, Loopback1
 I L2     172.16.254.2/32 [115/30] via 10.0.1.2, Ethernet1
                                   via 10.0.1.6, Ethernet2
                                   via 10.0.1.10, Ethernet3
 I L2     172.16.254.3/32 [115/30] via 10.0.1.2, Ethernet1
                                   via 10.0.1.6, Ethernet2
                                   via 10.0.1.10, Ethernet3
 I L2     172.16.254.21/32 [115/20] via 10.0.1.2, Ethernet1
 I L2     172.16.254.22/32 [115/20] via 10.0.1.6, Ethernet2
 I L2     172.16.254.23/32 [115/20] via 10.0.1.10, Ethernet3
 C        172.16.255.1/32 is directly connected, Loopback0
 I L2     172.16.255.2/32 [115/30] via 10.0.1.2, Ethernet1
                                   via 10.0.1.6, Ethernet2
                                   via 10.0.1.10, Ethernet3
 I L2     172.16.255.3/32 [115/30] via 10.0.1.2, Ethernet1
                                   via 10.0.1.6, Ethernet2
                                   via 10.0.1.10, Ethernet3
 I L2     172.16.255.21/32 [115/20] via 10.0.1.2, Ethernet1
 I L2     172.16.255.22/32 [115/20] via 10.0.1.6, Ethernet2
 I L2     172.16.255.23/32 [115/20] via 10.0.1.10, Ethernet3
```
#### Spine-2
```
Gateway of last resort is not set
 I L2     10.0.1.0/30 [115/20] via 10.0.2.2, Ethernet1
 I L2     10.0.1.4/30 [115/20] via 10.0.2.6, Ethernet2
 I L2     10.0.1.8/30 [115/20] via 10.0.2.10, Ethernet3
 C        10.0.2.0/30 is directly connected, Ethernet1
 C        10.0.2.4/30 is directly connected, Ethernet2
 C        10.0.2.8/30 is directly connected, Ethernet3
 I L2     10.0.3.0/30 [115/20] via 10.0.2.2, Ethernet1
 I L2     10.0.3.4/30 [115/20] via 10.0.2.6, Ethernet2
 I L2     10.0.3.8/30 [115/20] via 10.0.2.10, Ethernet3
 I L2     172.16.254.1/32 [115/30] via 10.0.2.2, Ethernet1
                                   via 10.0.2.6, Ethernet2
                                   via 10.0.2.10, Ethernet3
 C        172.16.254.2/32 is directly connected, Loopback1
 I L2     172.16.254.3/32 [115/30] via 10.0.2.2, Ethernet1
                                   via 10.0.2.6, Ethernet2
                                   via 10.0.2.10, Ethernet3
 I L2     172.16.254.21/32 [115/20] via 10.0.2.2, Ethernet1
 I L2     172.16.254.22/32 [115/20] via 10.0.2.6, Ethernet2
 I L2     172.16.254.23/32 [115/20] via 10.0.2.10, Ethernet3
 I L2     172.16.255.1/32 [115/30] via 10.0.2.2, Ethernet1
                                   via 10.0.2.6, Ethernet2
                                   via 10.0.2.10, Ethernet3
 C        172.16.255.2/32 is directly connected, Loopback0
 I L2     172.16.255.3/32 [115/30] via 10.0.2.2, Ethernet1
                                   via 10.0.2.6, Ethernet2
                                   via 10.0.2.10, Ethernet3
 I L2     172.16.255.21/32 [115/20] via 10.0.2.2, Ethernet1
 I L2     172.16.255.22/32 [115/20] via 10.0.2.6, Ethernet2
 I L2     172.16.255.23/32 [115/20] via 10.0.2.10, Ethernet3
```

#### Spine-3
```
Gateway of last resort is not set
 I L2     10.0.1.0/30 [115/20] via 10.0.3.2, Ethernet1
 I L2     10.0.1.4/30 [115/20] via 10.0.3.6, Ethernet2
 I L2     10.0.1.8/30 [115/20] via 10.0.3.10, Ethernet3
 I L2     10.0.2.0/30 [115/20] via 10.0.3.2, Ethernet1
 I L2     10.0.2.4/30 [115/20] via 10.0.3.6, Ethernet2
 I L2     10.0.2.8/30 [115/20] via 10.0.3.10, Ethernet3
 C        10.0.3.0/30 is directly connected, Ethernet1
 C        10.0.3.4/30 is directly connected, Ethernet2
 C        10.0.3.8/30 is directly connected, Ethernet3
 I L2     172.16.254.1/32 [115/30] via 10.0.3.2, Ethernet1
                                   via 10.0.3.6, Ethernet2
                                   via 10.0.3.10, Ethernet3
 I L2     172.16.254.2/32 [115/30] via 10.0.3.2, Ethernet1
                                   via 10.0.3.6, Ethernet2
                                   via 10.0.3.10, Ethernet3
 C        172.16.254.3/32 is directly connected, Loopback1
 I L2     172.16.254.21/32 [115/20] via 10.0.3.2, Ethernet1
 I L2     172.16.254.22/32 [115/20] via 10.0.3.6, Ethernet2
 I L2     172.16.254.23/32 [115/20] via 10.0.3.10, Ethernet3
 I L2     172.16.255.1/32 [115/30] via 10.0.3.2, Ethernet1
                                   via 10.0.3.6, Ethernet2
                                   via 10.0.3.10, Ethernet3
 I L2     172.16.255.2/32 [115/30] via 10.0.3.2, Ethernet1
                                   via 10.0.3.6, Ethernet2
                                   via 10.0.3.10, Ethernet3
 C        172.16.255.3/32 is directly connected, Loopback0
 I L2     172.16.255.21/32 [115/20] via 10.0.3.2, Ethernet1
 I L2     172.16.255.22/32 [115/20] via 10.0.3.6, Ethernet2
 I L2     172.16.255.23/32 [115/20] via 10.0.3.10, Ethernet3
```

#### Leaf-1
```
Gateway of last resort is not set
 C        10.0.1.0/30 is directly connected, Ethernet1
 I L2     10.0.1.4/30 [115/20] via 10.0.1.1, Ethernet1
 I L2     10.0.1.8/30 [115/20] via 10.0.1.1, Ethernet1
 C        10.0.2.0/30 is directly connected, Ethernet2
 I L2     10.0.2.4/30 [115/20] via 10.0.2.1, Ethernet2
 I L2     10.0.2.8/30 [115/20] via 10.0.2.1, Ethernet2
 C        10.0.3.0/30 is directly connected, Ethernet3
 I L2     10.0.3.4/30 [115/20] via 10.0.3.1, Ethernet3
 I L2     10.0.3.8/30 [115/20] via 10.0.3.1, Ethernet3
 I L2     172.16.254.1/32 [115/20] via 10.0.1.1, Ethernet1
 I L2     172.16.254.2/32 [115/20] via 10.0.2.1, Ethernet2
 I L2     172.16.254.3/32 [115/20] via 10.0.3.1, Ethernet3
 C        172.16.254.21/32 is directly connected, Loopback1
 I L2     172.16.254.22/32 [115/30] via 10.0.1.1, Ethernet1
                                    via 10.0.2.1, Ethernet2
                                    via 10.0.3.1, Ethernet3
 I L2     172.16.254.23/32 [115/30] via 10.0.1.1, Ethernet1
                                    via 10.0.2.1, Ethernet2
                                    via 10.0.3.1, Ethernet3
 I L2     172.16.255.1/32 [115/20] via 10.0.1.1, Ethernet1
 I L2     172.16.255.2/32 [115/20] via 10.0.2.1, Ethernet2
 I L2     172.16.255.3/32 [115/20] via 10.0.3.1, Ethernet3
 C        172.16.255.21/32 is directly connected, Loopback0
 I L2     172.16.255.22/32 [115/30] via 10.0.1.1, Ethernet1
                                    via 10.0.2.1, Ethernet2
                                    via 10.0.3.1, Ethernet3
 I L2     172.16.255.23/32 [115/30] via 10.0.1.1, Ethernet1
                                    via 10.0.2.1, Ethernet2
                                    via 10.0.3.1, Ethernet3
```

#### Leaf-2
```
Gateway of last resort is not set
 I L2     10.0.1.0/30 [115/20] via 10.0.1.5, Ethernet1
 C        10.0.1.4/30 is directly connected, Ethernet1
 I L2     10.0.1.8/30 [115/20] via 10.0.1.5, Ethernet1
 I L2     10.0.2.0/30 [115/20] via 10.0.2.5, Ethernet2
 C        10.0.2.4/30 is directly connected, Ethernet2
 I L2     10.0.2.8/30 [115/20] via 10.0.2.5, Ethernet2
 I L2     10.0.3.0/30 [115/20] via 10.0.3.5, Ethernet3
 C        10.0.3.4/30 is directly connected, Ethernet3
 I L2     10.0.3.8/30 [115/20] via 10.0.3.5, Ethernet3
 I L2     172.16.254.1/32 [115/20] via 10.0.1.5, Ethernet1
 I L2     172.16.254.2/32 [115/20] via 10.0.2.5, Ethernet2
 I L2     172.16.254.3/32 [115/20] via 10.0.3.5, Ethernet3
 I L2     172.16.254.21/32 [115/30] via 10.0.1.5, Ethernet1
                                    via 10.0.2.5, Ethernet2
                                    via 10.0.3.5, Ethernet3
 C        172.16.254.22/32 is directly connected, Loopback1
 I L2     172.16.254.23/32 [115/30] via 10.0.1.5, Ethernet1
                                    via 10.0.2.5, Ethernet2
                                    via 10.0.3.5, Ethernet3
 I L2     172.16.255.1/32 [115/20] via 10.0.1.5, Ethernet1
 I L2     172.16.255.2/32 [115/20] via 10.0.2.5, Ethernet2
 I L2     172.16.255.3/32 [115/20] via 10.0.3.5, Ethernet3
 I L2     172.16.255.21/32 [115/30] via 10.0.1.5, Ethernet1
                                    via 10.0.2.5, Ethernet2
                                    via 10.0.3.5, Ethernet3
 C        172.16.255.22/32 is directly connected, Loopback0
 I L2     172.16.255.23/32 [115/30] via 10.0.1.5, Ethernet1
                                    via 10.0.2.5, Ethernet2
                                    via 10.0.3.5, Ethernet3
```

#### Leaf-3
```
Gateway of last resort is not set

 I L2     10.0.1.0/30 [115/20] via 10.0.1.9, Ethernet1
 I L2     10.0.1.4/30 [115/20] via 10.0.1.9, Ethernet1
 C        10.0.1.8/30 is directly connected, Ethernet1
 I L2     10.0.2.0/30 [115/20] via 10.0.2.9, Ethernet2
 I L2     10.0.2.4/30 [115/20] via 10.0.2.9, Ethernet2
 C        10.0.2.8/30 is directly connected, Ethernet2
 I L2     10.0.3.0/30 [115/20] via 10.0.3.9, Ethernet3
 I L2     10.0.3.4/30 [115/20] via 10.0.3.9, Ethernet3
 C        10.0.3.8/30 is directly connected, Ethernet3
 I L2     172.16.254.1/32 [115/20] via 10.0.1.9, Ethernet1
 I L2     172.16.254.2/32 [115/20] via 10.0.2.9, Ethernet2
 I L2     172.16.254.3/32 [115/20] via 10.0.3.9, Ethernet3
 I L2     172.16.254.21/32 [115/30] via 10.0.1.9, Ethernet1
                                    via 10.0.2.9, Ethernet2
                                    via 10.0.3.9, Ethernet3
 I L2     172.16.254.22/32 [115/30] via 10.0.1.9, Ethernet1
                                    via 10.0.2.9, Ethernet2
                                    via 10.0.3.9, Ethernet3
 C        172.16.254.23/32 is directly connected, Loopback1
 I L2     172.16.255.1/32 [115/20] via 10.0.1.9, Ethernet1
 I L2     172.16.255.2/32 [115/20] via 10.0.2.9, Ethernet2
 I L2     172.16.255.3/32 [115/20] via 10.0.3.9, Ethernet3
 I L2     172.16.255.21/32 [115/30] via 10.0.1.9, Ethernet1
                                    via 10.0.2.9, Ethernet2
                                    via 10.0.3.9, Ethernet3
 I L2     172.16.255.22/32 [115/30] via 10.0.1.9, Ethernet1
                                    via 10.0.2.9, Ethernet2
                                    via 10.0.3.9, Ethernet3
 C        172.16.255.23/32 is directly connected, Loopback0
```

### Ping
```
Spine-1#ping 172.16.255.21
PING 172.16.255.21 (172.16.255.21) 72(100) bytes of data.
80 bytes from 172.16.255.21: icmp_seq=1 ttl=64 time=12.6 ms
80 bytes from 172.16.255.21: icmp_seq=2 ttl=64 time=7.24 ms
80 bytes from 172.16.255.21: icmp_seq=3 ttl=64 time=4.74 ms
80 bytes from 172.16.255.21: icmp_seq=4 ttl=64 time=4.39 ms
80 bytes from 172.16.255.21: icmp_seq=5 ttl=64 time=4.95 ms

--- 172.16.255.21 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 44ms
rtt min/avg/max/mdev = 4.393/6.807/12.696/3.111 ms, pipe 2, ipg/ewma 11.237/9.603 ms
Spine-1#
PING 172.16.255.22 (172.16.255.22) 72(100) bytes of data.
80 bytes from 172.16.255.22: icmp_seq=1 ttl=64 time=9.32 ms
80 bytes from 172.16.255.22: icmp_seq=2 ttl=64 time=3.51 ms
80 bytes from 172.16.255.22: icmp_seq=3 ttl=64 time=4.17 ms
80 bytes from 172.16.255.22: icmp_seq=4 ttl=64 time=3.52 ms
80 bytes from 172.16.255.22: icmp_seq=5 ttl=64 time=3.65 ms

--- 172.16.255.22 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 33ms
rtt min/avg/max/mdev = 3.510/4.837/9.323/2.256 ms, ipg/ewma 8.431/7.000 ms
Spine-1#
PING 172.16.255.23 (172.16.255.23) 72(100) bytes of data.
80 bytes from 172.16.255.23: icmp_seq=1 ttl=64 time=10.0 ms
80 bytes from 172.16.255.23: icmp_seq=2 ttl=64 time=7.63 ms
80 bytes from 172.16.255.23: icmp_seq=3 ttl=64 time=3.64 ms
80 bytes from 172.16.255.23: icmp_seq=4 ttl=64 time=5.16 ms
80 bytes from 172.16.255.23: icmp_seq=5 ttl=64 time=3.88 ms

--- 172.16.255.23 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 38ms
rtt min/avg/max/mdev = 3.646/6.081/10.070/2.446 ms, ipg/ewma 9.601/7.942 ms
Spine-1#
PING 172.16.255.2 (172.16.255.2) 72(100) bytes of data.
80 bytes from 172.16.255.2: icmp_seq=1 ttl=63 time=12.6 ms
80 bytes from 172.16.255.2: icmp_seq=2 ttl=63 time=8.22 ms
80 bytes from 172.16.255.2: icmp_seq=3 ttl=63 time=7.15 ms
80 bytes from 172.16.255.2: icmp_seq=4 ttl=63 time=9.58 ms
80 bytes from 172.16.255.2: icmp_seq=5 ttl=63 time=8.42 ms

--- 172.16.255.2 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 45ms
rtt min/avg/max/mdev = 7.156/9.213/12.684/1.902 ms, pipe 2, ipg/ewma 11.428/10.909 ms
Spine-1#
PING 172.16.255.3 (172.16.255.3) 72(100) bytes of data.
80 bytes from 172.16.255.3: icmp_seq=1 ttl=63 time=16.9 ms
80 bytes from 172.16.255.3: icmp_seq=2 ttl=63 time=10.9 ms
80 bytes from 172.16.255.3: icmp_seq=3 ttl=63 time=7.54 ms
80 bytes from 172.16.255.3: icmp_seq=4 ttl=63 time=8.65 ms
80 bytes from 172.16.255.3: icmp_seq=5 ttl=63 time=8.02 ms

--- 172.16.255.3 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 55ms
rtt min/avg/max/mdev = 7.547/10.422/16.955/3.466 ms, pipe 2, ipg/ewma 13.962/13.526 ms
Spine-1#
```
