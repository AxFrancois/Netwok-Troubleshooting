# VLAN

Frequent issues :

- [VLAN mismatch](#vlan-mismatch)
- [VLAN shutown](#vlan-shutown)
- [VLAN trunk instead of access](#Trunk-and-Access)
- [ip helper address missconfiguration](#ip-helper)
- [Native VLAN error](#Native-VLAN)

## VLAN mismatch

[//]: <> (Ok)

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

## VLAN shutowns

[//]: <> (To confirm)

### Symtoms

Pings are blocked on the interface because the sending switch drop the frame.

### Diagnostics

```Cisco IOS
Switch#show vlan

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Fa0/0
2    VLAN0002                         act/lshut Fa0/1
3    VLAN0003                         sus/lshut Fa0/2
4    VLAN0004                         act/ishut Fa0/3
5    VLAN0005                         sus/ishut Fa0/4

```

- `act/lshut` : VLAN status is active but shut down locally.
- `sus/lshut` : VLAN status is suspended but shut down locally.
- `act/ishut` : VLAN status is active but shut down internally.
- `sus/ishut` : VLAN status is suspended but shut down internally.

### Fixing

- If the VLAN is shutdown locally :

```Cisco IOS

```

- If the VLAN is shutdown internally :

```Cisco IOS
Switch#no shutdown vlan 3
Switch#no shutdown vlan 4
```

## Trunk and Access

[//]: <> (To do)

## IP helper

[//]: <> (To do)

## Native VLAN

[//]: <> (To do)

## Documenation

### Cisco IOS

- [Cisco IOS LAN Switching Command Reference VLAN](https://www.cisco.com/c/en/us/td/docs/ios/lanswitch/command/reference/lsw_book/lsw_s2.html)
- [Tagging the Native VLAN](https://www.networkworld.com/article/2234512/cisco-subnet-tagging-the-native-vlan.html)
