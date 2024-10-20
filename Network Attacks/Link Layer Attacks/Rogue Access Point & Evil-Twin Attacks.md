
> Addressing rogue access points and evil-twin attacks can seem like a gargantuan task due to their often elusive nature. Nevertheless, with the appropriate strategies in place, these illegitimate access points can be detected and managed effectively. In the realm of malevolent access points, rogue and evil-twin attacks invariably surface as significant concerns.


![Rogue AP](https://academy.hackthebox.com/storage/modules/229/rogueap.png)


> A rogue access point primarily serves as a tool to circumvent perimeter controls in place. An adversary might install such an access point to sidestep network controls and segmentation barriers, which could, in many cases, take the form of hotspots or tethered connections. These rogue points have even been known to infiltrate air-gapped networks. Their primary function is to provide unauthorized access to restricted sections of a network. The critical point to remember here is that rogue access points are directly connected to the network.


## Evil-Twin

>An evil-twin on the other hand is spun up by an attacker for many other different purposes. The key here, is that in most cases these access points are not connected to our network. Instead, they are standalone access points, which might have a web server or something else to act as a man-in-the-middle for wireless clients.



![Evil-Twin](https://academy.hackthebox.com/storage/modules/229/evil-twin.png)



### Airodump-ng Detection

>Right away, we could utilize the ESSID filter for Airodump-ng to detect Evil-Twin style access points.


```
$ sudo airodump-ng -c 4 --essid HTB-Wireless wlan0 -w raw

CH  4 ][ Elapsed: 1 min ][ 2023-07-13 16:06    
 BSSID              PWR RXQ  Beacons    #Data, #/s  CH   MB   ENC CIPHER  AUTH ESSID
 F8:14:FE:4D:E6:F2   -7 100      470      155    0   4   54   OPN              HTB-Wireless
 F8:14:FE:4D:E6:F1   -5  96      682        0    0   4  324   WPA2 CCMP   PSK  HTB-Wireless
```


A hostile portal attack is used by attackers in order extract credentials from users among other nefarious actions.


## Finding a Fallen User

Despite comprehensive security awareness training, some users may fall prey to attacks like these. Fortunately, in the case of open network style evil-twin attacks, we can view most higher-level traffic in an unencrypted format. To filter exclusively for the evil-twin access point, we would employ the following filter.

- (wlan.fc.type == 00) and (wlan.fc.type_subtype == 8)