# Router and Switch

- [Difference between switch and hub (collison domain)](#collision-domain)
- [Difference between switch and router](#router-vs-switch)
- [No power](#no-power)
- [Fan or PSU failure](#fan-or-psu-failure)
- [LED indicators](#led-indicators) <!--CCNA t1 203-->
- [Cisco IOS update](#cisco-ios-update)

## Collision domain

[//]: <> (Ok)

A collision occurs when two devices try to send data at the same time on the same link. This is bad if this happen, because the 2 packets that collided will be lost. The collision domain is the set of devices that can communicate with each other without the risk of collision. 2 network appliances are particulary source of collision : the hub and the wifi hotspot. See this as 2 people talking at the same time : neither of them will be heard and all you have is unintelligible noise. There is thankfully 3 solutions to this problem : CSMA/CD, CSMA/CA, and switches.

### CSMA/CD

CSMA/CD (Carrier Sense Multiple Access with Collision Detection) is a method that allows a device to detect if there is a collision before and while sending a packet. It works by detecting if there is a signal on the wire. If there is a signal, the device will wait the end of the transmission before trying to send the packet. If there is no signal, the device will send the packet. If the device detect that there is a collision, the device will send a jam signal and wait a random amount of time before trying to send the packet again. This method is used by Ethernet.

### CSMA/CA

CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance) is a method that allows a device to detect if there is a collision before sending a packet. When a device wants to send a packet, it will first detect if there is a signal on the wire. If there is a signal, the device will wait the end of the transmission before trying to send the packet. If there is no signal, the device will send a RTS (Request to Send) signal, and then wait for a CTS (Clear to Send) signal. If the device receive a CTS signal, it will send the packet.

### Switch

Network switches are devices that allow to connect multiple devices to a network. They are particulary useful because they allow to create a collision domain for each port (instead of a single collision domain for every port for a hub). This allow more stable and faster networks, and that's why hub have been replaced by switches in every enterprise network. Hub are still sometimes used in home networks because they are cheaper and easier to use.

## Router vs Switch

[//]: <> (Ok)

### Switch

A switch is a network device that link multiple devices together, forming a network : it's like an "internet power strip". A switch is what we call a layer 2 device : it's a device that only work on the data link layer, it create LANs (Local Area Network). Switch are represented by this symbol :

![](./Ressources/Images/switch.svg)

### Router

A router is a network device that allow to connect multiple networks together : it's like the electric endpoint of your house.
A router is what we call a layer 3 device : it's a device that work on the network layer, it interconnect LANs. Routers are specifically made to implement [Routing Protocols](), QoS (Quality of Services), VPN tunnels, MPLS, NAT... Routers are represented by this symbol :

![](./Ressources/Images/router.svg)

### MLS (Multi-Layer Switch)

A Multi-Layer Switch is a switch that can also work as a router. It's a layer 2 and 3 device, combining the best of both word. It's represented by this symbol :

![](./Ressources/Images/mls.svg)

| Category       | Switch | Router              | MLS                     |
| -------------- | ------ | ------------------- | ----------------------- |
| Layer          | 2      | 3 (sometimes 4 too) | 2 & 3 (sometimes 4 too) |
| Number of port | A lot  | Not much            | A lot                   |
| Price          | Cheap  | Expensive           | Very Expensive          |

> ✔️ Switch can be managed or unmanaged. Managed switch have a CLI (Command Line Interface) and sometimes a web interface that allow to configure them. Unmanaged switch are cheaper and easier to use, but you can't implement things like VLANs or NAC (Network Access Control).

> ⚠️ Routers and switches manufacturer might ask you to buy certain liscences to enable certain features. Always check the datasheet of the device you want to buy to see if it has the features you need.

> ✔️ Some devices might have expansion slots that allow to add more features to the device. For example, you can add more ethernet ports, serial ports or wifi antennas to a router or a switch.

## No power

[//]: <> (Ok)

PSU (Power Supply Unit) are the devices that provide power to the network devices. If the PSU is not working, the device will obviously not work. Depending on you contry, power outage can also be a frequent problem, especially if the device is not UPS (Uninterruptible Power Supply) protected. Lightning strike can also damage the PSU and the device.

> ⚠️ In all situation where you are working on a device, **always make sure that the device and the case is correctly grounded**. PSU, servers and network devices sometimes uses massive capacitors that can store a lot of energy, and if you are not grounded, you can get a shock when you touch the device, resulting is bruns and sometime death.

## Fan or PSU failure

[//]: <> (To do)
