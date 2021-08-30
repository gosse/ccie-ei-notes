## 1.6 Multicast

* LHR - Last Hop Router
* FHR - First Hop Router
* Sender/Source
* Receiver
* SSM - Source Specific Multicast
* ASM - Any Source Multicast 
* PIM-SM - Sparse Mode
* PIM-DM - Dense Mode
* Sparse-Dense Mode
* IGMP - Internet Group Management Protocol
* MLD - Multicast Listener Discovery

### 1.6.a Layer 2 multicast
#### 1.6.a i IGMPv2, IGMPv3

* IGMP is the Internet Group Management protocol, used for host to LHR communication. It is how hosts communicate they want to join multicast groups. 
* IGMPv2 standard for most things, IGMPv3 brings some new features, like Source-Specific Multicast (SSM), immediate join, and immediate leave. IMGPv2 enabled by default on Cisco routers. 
* Receiver sends Membership Report message to LHR, which then sends a PIM join to other routers up the tree. 
* LHR can do IGMPv2 to IGMPv3 mappings if the client doesn't support IGMPv3 


#### 1.6.a ii IGMP Snooping, PIM Snooping

* RFC 4541
* IGMP Snooping
  * IGMP snooping is Layer 2 switches watching IGMP *membership report* messages and keeping track which hosts are subscribed to which feeds for flooding traffic. 
  * If not enabled, BUM (broadcast, unknown unicast, multicast) traffic is flooded on all ports in the VLAN 
  * Enabled by Default 
  * `Switch(config)# [no] ip igmp snooping`
  * IGMP Snooping Querier 
    * IGMP snooping querier is used in a VLAN where PIM and IGMP are not configured because multicast traffic does not need to be routed. 
      * `Switch(config)# int vlan [VLAN ID]`
      * `Switch(config)# ip igmp snooping querier `
* PIM Snooping 
  * Requires IGMP snooping 
  * Used when a switch is between routers 
  * Forwards multicast to ports based on snooping PIM messages 
  * `Switch(config)# [no] ip pim snooping `


#### 1.6.a iii IGMP Querier

* A single router on a shared segment is elected to be the active querier. 
  * Router with lowest IP address become the querier 
  * This is not the same as the IGMP snooping querier.

#### 1.6.a iv IGMP Filter


* Filter IGMP joins by port 
* IOS-XE
  * `ip igmp filter #`
    * `#` is an igmp profile 
    * IGMP profiles are a bit like route-maps 

      Switch(config)# ip igmp profile 3
      Switch(config-igmp-profile)# permit
      Switch(config-igmp-profile)# range 229.9.9.0
      Switch(config-igmp-profile)# range 231.0.0.0 232.0.0.0
      Switch(config-igmp-profile)# exit
      Switch(config)# interface GigabitEthernet0/1
      Switch(config-if)# ip igmp filter 1

* IOS Classic
  * Do it with snooping
  * `ip igmp snooping access-group <acl> [vlan <vlan_id>]`
  * `ip access-list standard no-mcast-for-you`
  * `deny ip 229.0.0.0 0.0.0.255`
  * `permit ip any any`

#### 1.6.a v MLD
### 1.6.b Reverse path forwarding check
### 1.6.c PIM
#### 1.6.c i Sparse Mode
#### 1.6.c ii Static RP, BSR, AutoRP
#### 1.6.c iii Group to RP Mapping
#### 1.6.c iv Bidirectional PIM
#### 1.6.c v Source-Specific Multicast
#### 1.6.c vi Multicast boundary, RP announcement filter
#### 1.6.c vii PIMv6 Anycast RP
#### 1.6.c viii IPv4 Anycast RP using MSDP
#### 1.6.c ix Multicast multipath

