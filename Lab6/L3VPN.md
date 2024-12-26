## L3VPN

Вводные: 3 leaf, 2 spine, 2 host
         Vlan 20 ip 10.10.20.0/24 - leaf-1
         Vlan 30 ip 10.10.30.0/24 - leaf-2
На спайнах никаких дополнительных настроек не проводилось

### Схема сети

### Leaf-1
```
hostname Leaf-1
vlan 10
   name test-vxlan
vlan 20
vrf instance VRF
interface Ethernet1
   no switchport
   ip address 10.0.1.2/30
   ip ospf area 0.0.0.0
interface Ethernet2
   no switchport
   ip address 10.0.2.2/30
   ip ospf area 0.0.0.0
interface Ethernet3
   switchport access vlan 20
interface Loopback0
   ip address 172.16.255.21/32
   ip ospf area 0.0.0.0
interface Loopback1
   ip address 172.16.254.21/32
interface Vlan20
   vrf VRF
   ip address 10.10.20.1/24
interface Vxlan1
   vxlan source-interface Loopback1
   vxlan udp-port 4789
   vxlan vlan 10 vni 10010
   vxlan vrf VRF vni 10001
   vxlan learn-restrict any
ip routing
ip routing vrf VRF
router bgp 65000
   router-id 172.16.255.21
   neighbor SPINE_EVPN peer group
   neighbor 172.16.255.1 remote-as 65000
   neighbor 172.16.255.1 update-source Loopback0
   neighbor 172.16.255.1 send-community extended
   neighbor 172.16.255.2 remote-as 65000
   neighbor 172.16.255.2 update-source Loopback0
   neighbor 172.16.255.2 send-community extended
   vlan 10
      rd auto
      route-target both 10:10010
      redistribute learned
   address-family evpn
      neighbor 172.16.255.1 activate
      neighbor 172.16.255.2 activate
   address-family ipv4
      network 172.16.254.21/32
   vrf VRF
      rd 172.16.254.21:1
      route-target import evpn 1:10001
      route-target export evpn 1:10001
      redistribute connected
router ospf 1
   router-id 172.16.255.21
   max-lsa 12000

Leaf-1#sh vxlan vtep 
Remote VTEPS for Vxlan1:

VTEP                Tunnel Type(s)
------------------- --------------
172.16.254.22       flood, unicast

Total number of remote VTEPS:  1

Leaf-1#sh ip route vrf VRF 
VRF: VRF
 C        10.10.4.0/30 is directly connected, Ethernet4
 C        10.10.20.0/24 is directly connected, Vlan20
 B I      10.10.30.0/24 [200/0] via VTEP 172.16.254.22 VNI 10001 router-mac 50:01:00:4b:62:77 local-interface Vx1

Leaf-1#sh bgp evpn route-type ip-prefix ipv4
BGP routing table information for VRF default
Router identifier 172.16.255.21, local AS number 65000
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 172.16.254.21:1 ip-prefix 10.10.4.0/30
                                 -                     -       -       0       i
 * >      RD: 172.16.254.21:1 ip-prefix 10.10.20.0/24
                                 -                     -       -       0       i
 * >      RD: 172.16.254.22:1 ip-prefix 10.10.30.0/24
                                 172.16.254.22         -       100     0       i Or-ID: 172.16.255.22 C-LST: 172 
 *        RD: 172.16.254.22:1 ip-prefix 10.10.30.0/24
                                 172.16.254.22         -       100     0       i Or-ID: 172.16.255.22 C-LST: 172 
```

### Leaf-2
```
service routing protocols model multi-agent
hostname Leaf-2
vlan 10,30
vrf instance VRF
interface Ethernet1
   no switchport
   ip address 10.0.1.6/30
   ip ospf area 0.0.0.0
interface Ethernet2
   no switchport
   ip address 10.0.2.6/30
   ip ospf area 0.0.0.0
interface Ethernet3
   switchport access vlan 30
interface Loopback0
   ip address 172.16.255.22/32
   ip ospf area 0.0.0.0
interface Loopback1
   ip address 172.16.254.22/32
interface Vlan30
   vrf VRF
   ip address 10.10.30.1/24
interface Vxlan1
   vxlan source-interface Loopback1
   vxlan udp-port 4789
   vxlan vlan 10 vni 10010
   vxlan vrf VRF vni 10001
   vxlan learn-restrict any
ip routing
ip routing vrf VRF
router bgp 65000
   router-id 172.16.255.22
   neighbor SPINE_EVPN peer group
   neighbor 172.16.255.1 remote-as 65000
   neighbor 172.16.255.1 update-source Loopback0
   neighbor 172.16.255.1 send-community extended
   neighbor 172.16.255.2 remote-as 65000
   neighbor 172.16.255.2 update-source Loopback0
   neighbor 172.16.255.2 send-community extended
   vlan 10
      rd auto
      route-target both 10:10010
      redistribute learned
   address-family evpn
      neighbor 172.16.255.1 activate
      neighbor 172.16.255.2 activate
   address-family ipv4
      network 172.16.254.22/32
   vrf VRF
      rd 172.16.254.22:1
      route-target import evpn 1:10001
      route-target export evpn 1:10001
      redistribute connected
router ospf 1
   router-id 172.16.255.22
   max-lsa 12000

Leaf-2#sh vxlan vtep
Remote VTEPS for Vxlan1:

VTEP                Tunnel Type(s)
------------------- --------------
172.16.254.21       flood, unicast

Total number of remote VTEPS:  1

Leaf-2#sh ip route vrf VRF

VRF: VRF
Codes: C - connected, S - static, K - kernel, 
       O - OSPF, IA - OSPF inter area, E1 - OSPF external type 1,
       E2 - OSPF external type 2, N1 - OSPF NSSA external type 1,
       N2 - OSPF NSSA external type2, B - Other BGP Routes,
       B I - iBGP, B E - eBGP, R - RIP, I L1 - IS-IS level 1,
       I L2 - IS-IS level 2, O3 - OSPFv3, A B - BGP Aggregate,
       A O - OSPF Summary, NG - Nexthop Group Static Route,
       V - VXLAN Control Service, M - Martian,
       DH - DHCP client installed default route,
       DP - Dynamic Policy Route, L - VRF Leaked,
       G  - gRIBI, RC - Route Cache Route

Gateway of last resort is not set

 B I      10.10.4.0/30 [200/0] via VTEP 172.16.254.21 VNI 10001 router-mac 50:01:00:e5:e3:6a local-interface Vxl1
 B I      10.10.20.0/24 [200/0] via VTEP 172.16.254.21 VNI 10001 router-mac 50:01:00:e5:e3:6a local-interface Vx1
 C        10.10.30.0/24 is directly connected, Vlan30

Leaf-2#sh bgp evpn route-type ip-prefix ipv4
BGP routing table information for VRF default
Router identifier 172.16.255.22, local AS number 65000
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 172.16.254.21:1 ip-prefix 10.10.4.0/30
                                 172.16.254.21         -       100     0       i Or-ID: 172.16.255.21 C-LST: 172 
 *        RD: 172.16.254.21:1 ip-prefix 10.10.4.0/30
                                 172.16.254.21         -       100     0       i Or-ID: 172.16.255.21 C-LST: 172 
 * >      RD: 172.16.254.21:1 ip-prefix 10.10.20.0/24
                                 172.16.254.21         -       100     0       i Or-ID: 172.16.255.21 C-LST: 172 
 *        RD: 172.16.254.21:1 ip-prefix 10.10.20.0/24
                                 172.16.254.21         -       100     0       i Or-ID: 172.16.255.21 C-LST: 172 
 * >      RD: 172.16.254.22:1 ip-prefix 10.10.30.0/24
                                 -                     -       -       0       i
```

### Host-1
```
VPCS> sh ip

NAME        : VPCS[1]
IP/MASK     : 10.10.20.2/24
GATEWAY     : 10.10.20.1
DNS         : 
MAC         : 00:50:79:66:68:04
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500

VPCS> ping 10.10.30.2            

84 bytes from 10.10.30.2 icmp_seq=1 ttl=62 time=38.515 ms
84 bytes from 10.10.30.2 icmp_seq=2 ttl=62 time=17.504 ms
84 bytes from 10.10.30.2 icmp_seq=3 ttl=62 time=18.101 ms
84 bytes from 10.10.30.2 icmp_seq=4 ttl=62 time=16.995 ms
84 bytes from 10.10.30.2 icmp_seq=5 ttl=62 time=17.057 ms

```

### Host-2
```
VPCS> sh ip

NAME        : VPCS[1]
IP/MASK     : 10.10.30.2/24
GATEWAY     : 10.10.30.1
DNS         : 
MAC         : 00:50:79:66:68:08
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500

VPCS> ping 10.10.20.2

84 bytes from 10.10.20.2 icmp_seq=1 ttl=62 time=16.402 ms
84 bytes from 10.10.20.2 icmp_seq=2 ttl=62 time=16.540 ms
84 bytes from 10.10.20.2 icmp_seq=3 ttl=62 time=16.444 ms
84 bytes from 10.10.20.2 icmp_seq=4 ttl=62 time=17.649 ms
84 bytes from 10.10.20.2 icmp_seq=5 ttl=62 time=17.281 ms
```
