
>When we begin to look for network anomalies, we should always consider the IP layer. Simply put, the IP layer functions in its ability to transfer packets from one hop to another. This layer uses source and destination IP addresses for inter-host communications. When we examine this traffic, we can identify the IP addresses as they exist within the IP header of the packet.

1. `Length - IP header length`: This field contains the overall length of the IP header.
2. `Total Length - IP Datagram/Packet Length`: This field specifies the entire length of the IP packet, including any relevant data.
3. `Fragment Offset`: In many cases when a packet is large enough to be divided, the fragmentation offset will be set to provide instructions to reassemble the packet upon delivery to the destination host.
4. `Source and Destination IP Addresses`: These fields contain the origination (source) and destination IP addresses for the two communicating hosts.



![[IPheader.jpg]]

## Commonly Abused Fields

Innately, attackers might craft these packets to cause communication issues. Traditionally, an attacker might attempt to evade IDS controls through packet malformation or modification. As such, diving into each one of these fields and understanding how we can detect their misuse will equip us with the tools to succeed in our traffic analysis efforts.



Commonly, attackers might abuse this field for the following purposes:

1. `IPS/IDS Evasion`  - Well, for short, an attacker could split their nmap or other enumeration techniques to be fragmented, and as such it could bypass the detection.
   
2. `Firewall Evasion` - An attacker could evade a firewall's controls through fragmentation. If the firewall does not reassemble these packets before delivery.
   
3. `Firewall/IPS/IDS Resource Exhaustion` - An attacker were to craft their attack to fragment packets to a very small MTU (10, 15, 20, and so on), the network control might not reassemble these packets due to resource constraints.

4. `Denial of Service` - For old hosts, an attacker might utilize fragmentation to send IP packets exceeding 65535 bytes through ping or other commands. In doing so, the destination host will reassemble this malicious packet and experience countless different issues. As such, the resultant condition is successful denial-of-service from the attacker.


>In this case, the destination host would respond with RST flags for ports which do not have an active service running on them (aka closed ports). This pattern is a clear indication of a fragmented scan.

