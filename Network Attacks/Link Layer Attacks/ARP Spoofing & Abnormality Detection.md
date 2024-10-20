
The `Address Resolution Protocol (ARP)` has been a longstanding utility exploited by attackers to launch man-in-the-middle and denial-of-service attacks, among others. Given this prevalence, ARP forms a focal point when we undertake traffic analysis, often being the first protocol we scrutinize. Many ARP-based attacks are broadcasted, not directed specifically at hosts, making them more readily detectable through our packet sniffing techniques.


![ARP Protocol](https://academy.hackthebox.com/storage/modules/229/ARP-protocol.png)

### Avoid for this attack: 

1. `Static ARP Entries`: By disallowing easy rewrites and poisoning of the ARP cache, we can stymie these attacks. This, however, necessitates increased maintenance and oversight in our network environment.

2. `Switch and Router Port Security`: Implementing network profile controls and other measures can ensure that only authorized devices can connect to specific ports on our network devices, effectively blocking machines attempting ARP spoofing/poisoning.

#### Detection for this attack
> A key red flag we need to monitor is any anomaly in traffic emanating from a specific host. For instance, one host incessantly broadcasting ARP requests and replies to another host could be a telltale sign of ARP spoofing.

Wireshark detection for ARP request and reply

1. `Opcode == 1`: This represents all types of ARP Requests
2. `Opcode == 2`: This signifies all types of ARP Replies
3. `arp.duplicate-address-detected && arp.opcode == 2`

![ARP Cache Poisoning](https://academy.hackthebox.com/storage/modules/229/ARP-spoofing-poisoning.png)

## Identifying The Original IP Addresses

>A crucial question we need to pose is, what were the initial IP addresses of these devices? Understanding this aids us in determining which device altered its IP address through MAC spoofing. After all, if this attack was exclusively performed via ARP, the victim machine's IP address should remain consistent. Conversely, the attacker's machine might possess a different historical IP address.

- `(arp.opcode) && ((eth.src == 08:00:27:53:0c:ba) || (eth.dst == 08:00:27:53:0c:ba))`


>Right off the bat, we might notice some inconsistencies with TCP connections. If TCP connections are consistently dropping, it's an indication that the attacker is not forwarding traffic between the victim and the router.
>
>>If the attacker is, in fact, forwarding the traffic and is operating as a man-in-the-middle, we might observe identical or nearly symmetrical transmissions from the victim to the attacker and from the attacker to the router.

