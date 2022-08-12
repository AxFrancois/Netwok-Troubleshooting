# VLAN

Frequent issues :

- [VLAN mismatch](##vlan-mismatch)
- [VLAN shutown](##vlan-shutown)
- [VLAN trunk instead of access](##Trunk-and-Access)
- [ip helper address missconfiguration](##ip-helper)
- [Native VLAN error](##Native-VLAN)

## VLAN mismatch

[//]: <> (To confirm)

### Symtoms

On a Cisco switch, you can see a error message like this :

```Cisco IOS
ERROR: [VLAN mismatch] on interface Ethernet1/1, VLANs configured on the interface do not match the VLANs configured on the port.
```

Pings are blocked on the interface because the receiving switch drop the frame.

### Diagnotics

```Cisco IOS
Switch#show vlan brief
```

Comparate vlan affected to the interface between the 2 switches.

### Fixing

A very common issue is that the VLANs are not configured correctly. This is usually caused by a mismatch between the VLANs on the switches. Be always sure both switches have the same VLANs affected to a connected interface.

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
