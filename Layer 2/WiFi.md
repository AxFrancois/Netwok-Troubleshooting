# WiFi

Wifi is a technology both in layer 1 and layer 2. It is used to connect devices to a network without the need of a cable. It is a wireless technology that uses radio waves to transmit data. It is a very popular technology because it is easy to use and it is very fast. It is also very common to find wifi in public places like airports, hotels, restaurants, etc.

Frequent issues :

- [2.4 or 5 GHz](#2.4-or-5-ghz)
- [Frequent disconnections](#frequent-disconnections)

## 2.4 or 5 GHz

[//]: <> (Done)

2 bands of frequency are used for wifi. The 2.4 GHz band is the most common one. It is used for older devices. The 5 GHz band is used for newer devices. It is faster than the 2.4 GHz band but it is less common. However, there is many things to know when configuring a wifi network. To keep things simple, here is a table that shows the differences between the 2 bands.

| Category (More is better) | 2.4 GHz | 5 GHz |
| ------------------------- | ------- | ----- |
| Speed                     | +       | +++   |
| Range                     | +++     | +     |
| Penetrating solid objects | +++     | --    |
| Resistance to interferece | -       | +++   |
| Band availability         | -       | ++    |

## Frequent disconnections

[//]: <> (TO DO)

Disconnection from Wifi is a frequent issue, but it can be caused by many things, making it sometimes hard to diagnose.

### Symptoms

Your device is getting disconnected from the wifi network.

### Diagnosis

Use a wifi analyser to diagnose your wifi connection.
You should check the following things :

- Band usage : how many Wifi networks are using the same band as yours ?
- Signal strength : how strong is the signal of your network ?

Band Usage indicates how many Wifi networks are using the same band as yours. Here is an example of a network with a lot of networks using the same band.
![Band Usage](./Ressources/Images/WiFi%20band%20usage.png)

Signal strength is usually measured in dBm. Here is a table to help you diagnose your signal strength.

| dBm        | Signal strength | Application                   |
| ---------- | --------------- | ----------------------------- |
| > -30      | Excellent       | Everything                    |
| -30 to -67 | Good            | Streaming video, Online Games |
| -67 to -72 | Poor            | Streaming audio, Web browsing |
| < -72      | Terrible        | Nothing                       |

### Fixing

Depending on the results of your diagnosis, you can try to fix your wifi connection by doing the following things :

- Change the band of your network
- Move your device closer to the router
- Use a repeater or another access point

## Documentation

- Band usage made with Wifi Analyzer for Windows 10 : https://www.microsoft.com/store/productId/9NBLGGH33N0N
