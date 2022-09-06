# Address Resolution Protocol

<style>body {text-align: justify}</style>

ARP, wich stand for Address Resolution Protocol, is a protocol used to resolve the MAC address of a device from its IP address. It's a layer 3 protocol, but it's used in layer 2 communication.

It works by sending a layer 2 broadcast message to the network (destination MAC address FF:FF:FF:FF:FF:FF), asking for the MAC address of a specific IP address. The device with the IP address will respond with its MAC address to the source.

ARP is rarely the cause of a problem, but it's important to understand how it works because it can help to detect a [IPv4 Duplication](IPv4.md/#ip-duplicate) issue.

Frequent issues :

- [ARP Caching issue](#arp-caching-issue)
- [ARP for duplicate IP](#arp-for-duplicate-ip)

## ARP Caching issue

[//]: <> (To do)

As we said, the ARP uses a broadcast message to resolve the MAC address of a device. A broadcast message creates a lot of traffic on the network, so it's not a good idea to send it every time we need to communicate with a device. To avoid this, the ARP uses a cache to store the MAC address of the devices it already resolved. This cache is stored in the ARP table of the device. Every IP device, from a router to a PC, has its own ARP table. The entries in the cache have a limited lifetime, after which the device will send a new ARP request to resolve the MAC address if it needs to communicate with the device.

### Symptoms

### Diagnosis

You can take a look at the ARP table of your device to check for any incorrect entry.

- Cisco IOS

```Cisco IOS
Router#show ip arp
```

- Linux

```Bash
$ arp -a
```

- Windows

```Shell
C:\Users\MyUser>arp -a
Interface : 192.168.1.18 --- 0x11
Adresse Internet      Adresse physique      Type
192.168.1.1           00-90-93-39-66-d3     dynamique
224.0.0.2             01-00-5e-00-00-02     statique
224.0.0.22            01-00-5e-00-00-16     statique
224.0.0.251           01-00-5e-00-00-fb     statique
224.0.0.252           01-00-5e-00-00-fc     statique
224.0.1.60            01-00-5e-00-01-3c     statique
224.0.1.187           01-00-5e-00-01-bb     statique
226.1.1.1             01-00-5e-01-01-01     statique
230.230.230.230       01-00-5e-66-e6-e6     statique
239.255.132.178       01-00-5e-7f-84-b2     statique
239.255.255.250       01-00-5e-7f-ff-fa     statique
239.255.255.253       01-00-5e-7f-ff-fd     statique
255.255.255.255       ff-ff-ff-ff-ff-ff     statique
```

> 📍 Note : the ARP table contains certains entries that are not related to the devices on the network, as my network is 192.168.0.0/24. These entries are used for multicast communication, like explained in [IPv4 Specials IP](IPv4.md/#using-special-ips) (224.0.0.0/4). Their MAC addresses are also not real MAC addresses, they are called Multicast MAC addresses, like explained in [Specials MAC addresses](../Layer%202/MAC.md/#S).

### Fixing

Depending on your issue, you can add or delete a specific entry in the ARP table, or you can flush the whole ARP table.

- Cisco IOS

```Cisco IOS
Router#arp 192.168.1.15 AA:BB:CC:DD:EE:FF
Router#no arp 192.168.1.15 AA:BB:CC:DD:EE:FF
Router#clear ip arp
```

- Linux

```Bash
$ arp -s 192.168.1.15 AA:BB:CC:DD:EE:FF
$ arp -d 192.168.1.15 AA:BB:CC:DD:EE:FF
$ ip neighbor flush all
```

- Windows

```Shell

```

## ARP for duplicate IP

[//]: <> (To do)
