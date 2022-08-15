# MAC Address

MAC Address, wich stand for Media Access Control, is a unique identifier for each device on the network. It's a 48 bits address that is used for the layer 2 communication. It is physically linked to the network card of the device, tho it still can be modified by some software in an action named mac spoofing.

Each constructor have a range of allowed MAC addresses, usually the 24th first bits (or 6 first hexadecimal digits) are used to identify the manufacturer of the device. This can be usefull when for exemple you want to create security polocy depending on the manufacturer of the device. You can use website such as https://macvendors.com/ to get the manufacturer of the device.

Frequent issues :

- [MAC Duplication](##MAC-Duplication)

## MAC Duplication

[//]: <> (To confirm)

In a layer 2 segment, each device must have a unique MAC address. The probability of a device having the same MAC address is very low (1/2^48 = 1 in 281 trillion), so this issue should never happen, right ? Right ? Well actually it can happen, and it's when I'm working with virtual machines. When you carelessly clone a virtual machine, the MAC address is the same as the original one.

### Symtoms

The machine is unreachable, traffic is lost

### Diagnostics

```Cisco IOS
Switch#show mac address-table
```

### Fixing
