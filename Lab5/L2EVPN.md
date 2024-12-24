## L2EVPN

Вводные: 2 спайна, 3 лифа, 2 хоста. Значимые лиф1 и лиф2
         undelay OSPF, overlay iBGP
         шаблоны в BGP не использовались сознательно, как и VRF

### Leaf-1
```
service routing protocols model multi-agent
hostname Leaf-1
vlan 10
   name test-vxlan
interface Ethernet1
   no switchport
   ip address 10.0.1.2/30
   ip ospf area 0.0.0.0
interface Ethernet2
   no switchport
   ip address 10.0.2.2/30
   ip ospf area 0.0.0.0
interface Ethernet3
   switchport access vlan 10
interface Loopback0
   ip address 172.16.255.21/32
   ip ospf area 0.0.0.0
interface Loopback1
   ip address 172.16.254.21/32
interface Vxlan1
   vxlan source-interface Loopback1
   vxlan udp-port 4789
   vxlan vlan 10 vni 10010
   vxlan learn-restrict any
ip routing
router bgp 65000
   router-id 172.16.255.21
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
router ospf 1
   router-id 172.16.255.21
   max-lsa 12000

Leaf-1#sh ip route
 C        10.0.1.0/30 is directly connected, Ethernet1
 O        10.0.1.4/30 [110/20] via 10.0.1.1, Ethernet1
 O        10.0.1.8/30 [110/20] via 10.0.1.1, Ethernet1
 C        10.0.2.0/30 is directly connected, Ethernet2
 O        10.0.2.4/30 [110/20] via 10.0.2.1, Ethernet2
 O        10.0.2.8/30 [110/20] via 10.0.2.1, Ethernet2
 O        10.0.3.8/30 [110/30] via 10.0.1.1, Ethernet1
                               via 10.0.2.1, Ethernet2
 C        172.16.254.21/32 is directly connected, Loopback1
 B I      172.16.254.22/32 [200/0] via 10.0.1.1, Ethernet1
                                   via 10.0.2.1, Ethernet2
 B I      172.16.254.23/32 [200/0] via 10.0.1.1, Ethernet1
                                   via 10.0.2.1, Ethernet2
 O        172.16.255.1/32 [110/20] via 10.0.1.1, Ethernet1
 O        172.16.255.2/32 [110/20] via 10.0.2.1, Ethernet2
 C        172.16.255.21/32 is directly connected, Loopback0
 O        172.16.255.22/32 [110/30] via 10.0.1.1, Ethernet1
                                    via 10.0.2.1, Ethernet2
 O        172.16.255.23/32 [110/30] via 10.0.1.1, Ethernet1
                                    via 10.0.2.1, Ethernet2

Leaf-1#sh interfaces vx1
Vxlan1 is up, line protocol is up (connected)
  Hardware is Vxlan
  Source interface is Loopback1 and is active with 172.16.254.21
  Listening on UDP port 4789
  Replication/Flood Mode is headend with Flood List Source: EVPN
  Remote MAC learning via EVPN
  VNI mapping to VLANs
  Static VLAN to VNI mapping is 
    [10, 10010]      
  Note: All Dynamic VLANs used by VCS are internal VLANs.
        Use 'show vxlan vni' for details.
  Static VRF to VNI mapping is not configured
  Headend replication flood vtep list is:
    10 172.16.254.22  

Leaf-1#sh vxlan vni 
VNI to VLAN Mapping for Vxlan1
VNI         VLAN       Source       Interface       802.1Q Tag
----------- ---------- ------------ --------------- ----------
10010       10         static       Ethernet3       untagged  
                                    Vxlan1          10

Leaf-1#sh bgp evpn summary 
BGP summary information for VRF default
Router identifier 172.16.255.21, local AS number 65000
Neighbor Status Codes: m - Under maintenance
  Neighbor     V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  172.16.255.1 4 65000            154       176    0    0 01:48:44 Estab   1      1
  172.16.255.2 4 65000            153       175    0    0 01:48:32 Estab   1      1

Leaf-1#sh bgp evpn
BGP routing table information for VRF default
Router identifier 172.16.255.21, local AS number 65000
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 172.16.255.21:10 mac-ip 0050.7966.6804
                                 -                     -       -       0       i
 * >Ec    RD: 172.16.255.22:10 mac-ip 0050.7966.6808
                                 172.16.254.22         -       100     0       i Or-ID: 172.16.255.22 C-LST: 172 
 *  ec    RD: 172.16.255.22:10 mac-ip 0050.7966.6808
                                 172.16.254.22         -       100     0       i Or-ID: 172.16.255.22 C-LST: 172 
 * >      RD: 172.16.255.21:10 imet 172.16.254.21
                                 -                     -       -       0       i
 * >Ec    RD: 172.16.255.22:10 imet 172.16.254.22
                                 172.16.254.22         -       100     0       i Or-ID: 172.16.255.22 C-LST: 172 
 *  ec    RD: 172.16.255.22:10 imet 172.16.254.22
                                 172.16.254.22         -       100     0       i Or-ID: 172.16.255.22 C-LST: 172

```

### Leaf-2
```
service routing protocols model multi-agent
hostname Leaf-2
vlan 10
interface Ethernet1
   no switchport
   ip address 10.0.1.6/30
   ip ospf area 0.0.0.0
interface Ethernet2
   no switchport
   ip address 10.0.2.6/30
   ip ospf area 0.0.0.0
interface Ethernet3
   switchport access vlan 10
interface Loopback0
   ip address 172.16.255.22/32
   ip ospf area 0.0.0.0
interface Loopback1
   ip address 172.16.254.22/32
interface Vxlan1
   vxlan source-interface Loopback1
   vxlan udp-port 4789
   vxlan vlan 10 vni 10010
   vxlan learn-restrict any
ip routing
router bgp 65000
   router-id 172.16.255.22
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
router ospf 1
   router-id 172.16.255.22
   max-lsa 12000

Leaf-2#sh int vxlan 1
Vxlan1 is up, line protocol is up (connected)
  Hardware is Vxlan
  Source interface is Loopback1 and is active with 172.16.254.22
  Listening on UDP port 4789
  Replication/Flood Mode is headend with Flood List Source: EVPN
  Remote MAC learning via EVPN
  VNI mapping to VLANs
  Static VLAN to VNI mapping is 
    [10, 10010]      
  Note: All Dynamic VLANs used by VCS are internal VLANs.
        Use 'show vxlan vni' for details.
  Static VRF to VNI mapping is not configured
  Headend replication flood vtep list is:
    10 172.16.254.21  
  Shared Router MAC is 0000.0000.0000

Leaf-2#sh vxlan vni 
VNI to VLAN Mapping for Vxlan1
VNI         VLAN       Source       Interface       802.1Q Tag
----------- ---------- ------------ --------------- ----------
10010       10         static       Ethernet3       untagged  
                                    Vxlan1          10        

Leaf-2#sh bgp evpn 
BGP routing table information for VRF default
Router identifier 172.16.255.22, local AS number 65000
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >Ec    RD: 172.16.255.21:10 mac-ip 0050.7966.6804
                                 172.16.254.21         -       100     0       i Or-ID: 172.16.255.21 C-LST: 172 
 *  ec    RD: 172.16.255.21:10 mac-ip 0050.7966.6804
                                 172.16.254.21         -       100     0       i Or-ID: 172.16.255.21 C-LST: 172 
 * >      RD: 172.16.255.22:10 mac-ip 0050.7966.6808
                                 -                     -       -       0       i
 * >Ec    RD: 172.16.255.21:10 imet 172.16.254.21
                                 172.16.254.21         -       100     0       i Or-ID: 172.16.255.21 C-LST: 172 
 *  ec    RD: 172.16.255.21:10 imet 172.16.254.21
                                 172.16.254.21         -       100     0       i Or-ID: 172.16.255.21 C-LST: 172 
 * >      RD: 172.16.255.22:10 imet 172.16.254.22
                                 -                     -       -       0       i
```

### Spine-1
```
service routing protocols model multi-agent
hostname Spine-1
interface Ethernet1
   no switchport
   ip address 10.0.1.1/30
   ip ospf area 0.0.0.0
interface Ethernet2
   no switchport
   ip address 10.0.1.5/30
   ip ospf area 0.0.0.0
interface Ethernet3
   no switchport
   ip address 10.0.1.9/30
   ip ospf area 0.0.0.0
interface Loopback0
   ip address 172.16.255.1/32
   ip ospf area 0.0.0.0
ip routing
router bgp 65000
   router-id 172.16.255.1
   neighbor 172.16.255.21 remote-as 65000
   neighbor 172.16.255.21 update-source Loopback0
   neighbor 172.16.255.21 route-reflector-client
   neighbor 172.16.255.21 send-community extended
   neighbor 172.16.255.22 remote-as 65000
   neighbor 172.16.255.22 update-source Loopback0
   neighbor 172.16.255.22 route-reflector-client
   neighbor 172.16.255.22 send-community extended
   neighbor 172.16.255.23 remote-as 65000
   neighbor 172.16.255.23 update-source Loopback0
   neighbor 172.16.255.23 route-reflector-client
   neighbor 172.16.255.23 send-community extended
   address-family evpn
      neighbor 172.16.255.21 activate
      neighbor 172.16.255.22 activate
      neighbor 172.16.255.23 activate
router ospf 1
   router-id 172.16.255.1
   max-lsa 12000

Spine-1#sh bgp evpn 
BGP routing table information for VRF default
Router identifier 172.16.255.1, local AS number 65000
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 172.16.255.21:10 mac-ip 0050.7966.6804
                                 172.16.254.21         -       100     0       i
 * >      RD: 172.16.255.22:10 mac-ip 0050.7966.6808
                                 172.16.254.22         -       100     0       i
 * >      RD: 172.16.255.21:10 imet 172.16.254.21
                                 172.16.254.21         -       100     0       i
 * >      RD: 172.16.255.22:10 imet 172.16.254.22
                                 172.16.254.22         -       100     0       i
```

### Spine-2
```
service routing protocols model multi-agent
hostname Spine-2
interface Ethernet1
   no switchport
   ip address 10.0.2.1/30
   ip ospf area 0.0.0.0
interface Ethernet2
   no switchport
   ip address 10.0.2.5/30
   ip ospf area 0.0.0.0
interface Ethernet3
   no switchport
   ip address 10.0.2.9/30
   ip ospf area 0.0.0.0
interface Loopback0
   ip address 172.16.255.2/32
   ip ospf area 0.0.0.0
interface Loopback1
   ip address 172.16.254.2/32
ip routing
router bgp 65000
   router-id 172.16.255.2
   neighbor 172.16.255.21 remote-as 65000
   neighbor 172.16.255.21 update-source Loopback0
   neighbor 172.16.255.21 route-reflector-client
   neighbor 172.16.255.21 send-community extended
   neighbor 172.16.255.22 remote-as 65000
   neighbor 172.16.255.22 update-source Loopback0
   neighbor 172.16.255.22 route-reflector-client
   neighbor 172.16.255.22 send-community extended
   neighbor 172.16.255.23 remote-as 65000
   neighbor 172.16.255.23 update-source Loopback0
   neighbor 172.16.255.23 route-reflector-client
   neighbor 172.16.255.23 send-community extended
   address-family evpn
      neighbor 172.16.255.21 activate
      neighbor 172.16.255.22 activate
      neighbor 172.16.255.23 activate
router ospf 1
   router-id 172.16.255.2
   max-lsa 12000

Spine-2#sh bgp evpn 
BGP routing table information for VRF default
Router identifier 172.16.255.2, local AS number 65000
Route status codes: * - valid, > - active, S - Stale, E - ECMP head, e - ECMP
                    c - Contributing to ECMP, % - Pending BGP convergence
Origin codes: i - IGP, e - EGP, ? - incomplete
AS Path Attributes: Or-ID - Originator ID, C-LST - Cluster List, LL Nexthop - Link Local Nexthop

          Network                Next Hop              Metric  LocPref Weight  Path
 * >      RD: 172.16.255.21:10 mac-ip 0050.7966.6804
                                 172.16.254.21         -       100     0       i
 * >      RD: 172.16.255.22:10 mac-ip 0050.7966.6808
                                 172.16.254.22         -       100     0       i
 * >      RD: 172.16.255.21:10 imet 172.16.254.21
                                 172.16.254.21         -       100     0       i
 * >      RD: 172.16.255.22:10 imet 172.16.254.22
                                 172.16.254.22         -       100     0       i
```

### VPC-1
```
VPCS> sh ip
NAME        : VPCS[1]
IP/MASK     : 10.10.3.1/24
GATEWAY     : 255.255.255.0
DNS         : 
MAC         : 00:50:79:66:68:04
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500
VPCS> ping 10.10.3.3
84 bytes from 10.10.3.3 icmp_seq=1 ttl=64 time=24.966 ms
84 bytes from 10.10.3.3 icmp_seq=2 ttl=64 time=18.904 ms
84 bytes from 10.10.3.3 icmp_seq=3 ttl=64 time=19.183 ms
84 bytes from 10.10.3.3 icmp_seq=4 ttl=64 time=17.439 ms
84 bytes from 10.10.3.3 icmp_seq=5 ttl=64 time=16.621 ms
```

### VPC-3
```
VPCS> sh ip
NAME        : VPCS[1]
IP/MASK     : 10.10.3.3/24
GATEWAY     : 255.255.255.0
DNS         : 
MAC         : 00:50:79:66:68:08
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500
VPCS> ping 10.10.3.1
84 bytes from 10.10.3.1 icmp_seq=1 ttl=64 time=15.485 ms
84 bytes from 10.10.3.1 icmp_seq=2 ttl=64 time=17.698 ms
84 bytes from 10.10.3.1 icmp_seq=3 ttl=64 time=16.635 ms
84 bytes from 10.10.3.1 icmp_seq=4 ttl=64 time=14.986 ms
84 bytes from 10.10.3.1 icmp_seq=5 ttl=64 time=17.490 ms
VPCS> 
```

