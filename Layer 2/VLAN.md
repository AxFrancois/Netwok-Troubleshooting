# VLAN

Frequent issues :

- [VLAN mismatch](#vlan-mismatch)
- [VLAN trunk instead of access](#Trunk-and-Access)
- [Native VLAN error](#Native-VLAN)
- [VLAN mismatch on trunk](#VLAN-mismatch-on-trunk)

## VLAN mismatch

<style>body {text-align: justify}</style>

[//]: <> (Done)

A very common issue is that the VLANs are not configured correctly. Be always sure both switches have the same VLANs affected to a connected interface.

### Symtoms

On a Cisco switch, you can see a error message like this :

```Cisco IOS
%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (2), with Switch2 FastEthernet0/1 (3).
```

Pings are blocked on the interface because the receiving switch drop the frame.

### Diagnotics

Actually the CDP-4-NATIVE_VLAN_MISMATCH error message indicate the VLAN number that are mismatching and the hostname of the machine you have a mismatch with. In the first case, Switch1's fa0/1 is in the VLAN 2 but Switch2's fa0/1 is in the VLAN 3.

If for whatever reason this message doesn't appear, there is still many way to diagnose the issue.

1. Identify the ports. For this I use CDP (Cisco Discovery Protocol) `show cdp neighbors`, because LLDP or ARP will not be able to help since the layer 2 link is broken. For CDP you can also use the command `show cdp neighbors detail` to get more information about the port.

   ```Cisco IOS
   Switch1#sh cdp neighbors
   Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                   S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
   Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
   Switch2      Fas 0/1          139            S       2960        Fas 0/1

   Switch1#show cdp neighbors detail

   Device ID: Switch2
   Entry address(es):
   Platform: cisco 2960, Capabilities: Switch
   Interface: FastEthernet0/1, Port ID (outgoing port): FastEthernet0/1
   Holdtime: 139

   Version :
   Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.0(2)SE4, RELEASE SOFTWARE (fc1)
   Technical Support: http://www.cisco.com/techsupport
   Copyright (c) 1986-2013 by Cisco Systems, Inc.
   Compiled Wed 26-Jun-13 02:49 by mnguyen

   advertisement version: 2
   Duplex: full
   ```

2. Check if the VLAN is the same on both side. For this, you can use the command `show vlan` on both side. You can also use the command `show vlan brief` to get a more compact output. You can also use `show interface status` to get the VLAN number of the port.

   ```Cisco IOS
   Switch1#show vlan brief

   VLAN Name                             Status    Ports
   ---- -------------------------------- --------- -------------------------------
   1    default                          active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                   Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                   Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                   Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                   Fa0/18, Fa0/19, Fa0/20, Fa0/21
                                                   Fa0/22, Fa0/23, Fa0/24, Gig0/1
                                                   Gig0/2
   2    VLAN2                            active    Fa0/1
   1002 fddi-default                     active
   1003 token-ring-default               active
   1004 fddinet-default                  active
   1005 trnet-default                    active


   Switch2#sh vl br

   VLAN Name                             Status    Ports
   ---- -------------------------------- --------- -------------------------------
   1    default                          active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                   Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                   Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                   Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                   Fa0/18, Fa0/19, Fa0/20, Fa0/21
                                                   Fa0/22, Fa0/23, Fa0/24, Gig0/1
                                                   Gig0/2
   3    VLAN3                            active    Fa0/1
   1002 fddi-default                     active
   1003 token-ring-default               active
   1004 fddinet-default                  active
   1005 trnet-default                    active
   ```

Comparate vlan affected to the interface between the 2 switches. Here we can see that the VLAN 2 is affected to the fa0/1 on Switch1 but the VLAN 3 is affected to the fa0/1 on Switch2.

### Fixing

Simply change the vlan affected to the interface on the switch that is causing the issue. For this, you can use the command `interface fa0/1` and then `switchport access vlan <vlan-id>`. You can also use the command `switchport mode access` to be sure the interface is in access mode.

```Cisco IOS
Switch2#conf t
Switch2(config)#interface FastEthernet0/1
Switch2(config-if)#switchport mode access
Switch2(config-if)#switchport access vlan 2
```

## Trunk and Access

[//]: <> (Done)

Another fairly common mistake is misusing the trunk and access mode. The trunk mode is used to connect 2 switches together. The access mode is used to connect a switch to a device.

### Symtoms

No error message will appear when you have a trunk instead of an access. The only way to know is to check the configuration of the switch. If the trunk allow VLAN 1 or have a native VLAN configured, there is actually no symptoms at all. In this case, the only way to know is to check the configuration.

### Diagnostics

You can also use the command `show interface trunk` to get more information about the trunk. You can also use the commande `show interface <interface> switchport` to get more information about the configuration of the interface.

```Cisco IOS
Switch#sh int trunk
Port        Mode         Encapsulation  Status        Native vlan
Gig0/1      on           802.1q         trunking      1

Port        Vlans allowed on trunk
Gig0/1      1-2

Port        Vlans allowed and active in management domain
Gig0/1      1,2

Port        Vlans in spanning tree forwarding state and not pruned
Gig0/1      1,2

Switch#sh int g0/1 switchport
Name: Gig0/1
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 1 (default)
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 1-2
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
```

### Fixing

There is actually many ways to fix this issues, depending on what you want to obtain :

- You can use the command `switchport mode access` to change the mode of the interface to access.

  ```Cisco IOS
  Switch#conf t
  Switch(config)#interface GigabitEthernet0/1
  Switch(config-if)#switchport mode access
  ```

- You can also use the command `switchport trunk native vlan <vlan-id>` to change the native VLAN of the trunk.
  ```Cisco IOS
  Switch#conf t
  Switch(config)#interface GigabitEthernet0/1
  Switch(config-if)#switchport trunk native vlan 2
  ```
- You can also use the command `switchport trunk allowed vlan <vlan-id>` to change the VLAN allowed on the trunk.
  ```Cisco IOS
  Switch#conf t
  Switch(config)#interface GigabitEthernet0/1
  Switch(config-if)#switchport trunk allowed vlan 2
  ```

## Native VLAN

[//]: <> (To do)

The native VLAN is the VLAN that doesn't have a vlan tag on the frame (in the normal behavior). It is used to send untagged frame between 2 switches. It is also used to send untagged frame to a device that doesn't support VLAN. By default, the native VLAN is VLAN 1. This allow for some "weird" configuration like so :

                                                                Switch1              +-----------------+            Switch2
                                                            VLAN : 1 (native), 2, 3                       VLAN : 2, 3, 100 (native)

But this might be not what you wanted to do.

### Symtoms

If we take our previous example, theoretically everything should work and you would not notice anything. Hopefully, the CDP (Cisco Discovery Protocol) will help you to find the issue by sending you an alert.

```Cisco IOS
%CDP-4-NATIVE_VLAN_MISMATCH: Native VLAN mismatch discovered on FastEthernet0/1 (1), with Switch FastEthernet0/1 (100).
```

But because CDP is a proprietary protocol, you might not have it on your network. In this case, another protocol might warn you, the STP (Spanning Tree Protocol). The STP will detect that the native VLAN is different on the 2 switches and will block the port.

```Cisco IOS
%SPANTREE-2-RECV_PVID_ERR: Received BPDU with inconsistent peer vlan id 100 on FastEthernet0/1 VLAN1.

%SPANTREE-2-BLOCK_PVID_LOCAL: Blocking FastEthernet0/1 on VLAN0001. Inconsistent local vlan.
```

However, if for some reason, you don't have CDP or STP, you will not notice anything. The only way to know is to check the configuration of the switch.

### Diagnostics

You can use the command `show interface <interface> switchport` to get more information about the configuration of the interface.

```Cisco IOS
Switch1#sh int fa0/1 switchport
Name: Fa0/1
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 1 (default)      <--- Native VLAN is 1
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 1-1001
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none

Switch2#sh int fa0/1 switchport
Name: Fa0/1
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
Administrative Trunking Encapsulation: dot1q
Operational Trunking Encapsulation: dot1q
Negotiation of Trunking: On
Access Mode VLAN: 1 (default)
Trunking Native Mode VLAN: 100 (VLAN100)    <--- Native VLAN is 100
Voice VLAN: none
Administrative private-vlan host-association: none
Administrative private-vlan mapping: none
Administrative private-vlan trunk native VLAN: none
Administrative private-vlan trunk encapsulation: dot1q
Administrative private-vlan trunk normal VLANs: none
Administrative private-vlan trunk private VLANs: none
Operational private-vlan: none
Trunking VLANs Enabled: 2-1005
Pruning VLANs Enabled: 2-1001
Capture Mode Disabled
Capture VLANs Allowed: ALL
Protected: false
Unknown unicast blocked: disabled
Unknown multicast blocked: disabled
Appliance trust: none
```

### Fixing

One again, it depends on what you want to obtain. If you change the native VLAN so they both matches (for exemple here 100), you can use `switchport trunk native vlan <vlan-id>`

```Cisco IOS
Switch1#conf t
Switch1(config)#interface FastEthernet0/1
Switch1(config-if)#switchport trunk native vlan 100
```

On the other hand if you want to keep the config displayed as an exemple, you can disable STP and CDP on the interface.

```Cisco IOS
Switch1#conf t
Switch1(config)#interface FastEthernet0/1
Switch1(config-if)#no spanning-tree portfast
Switch1(config-if)#no cdp enable
```

## Documenation

### Cisco IOS

- [Cisco IOS LAN Switching Command Reference VLAN](https://www.cisco.com/c/en/us/td/docs/ios/lanswitch/command/reference/lsw_book/lsw_s2.html)
- [Tagging the Native VLAN](https://www.networkworld.com/article/2234512/cisco-subnet-tagging-the-native-vlan.html)

```

```
