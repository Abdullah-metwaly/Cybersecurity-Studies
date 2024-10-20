Tunneling is a technique employed by adversaries in order to exfiltrate data from one location to another. There are many different kinds of tunneling, and each different kind uses a different protocol. Commonly, attackers may utilize proxies to bypass our network controls, or protocols that our systems and controls allow.


We noticed fragmentation occurring within our ICMP traffic as it is above, this would indicate a large amount of data being transferred via ICMP. However a suspicious ICMP request might have a large data length like 38000 bytes. 


![[ICMP-tunneling.png]]



## Preventing ICMP Tunneling

In order to prevent ICMP tunneling from occurring we can conduct the following actions.

1. `Block ICMP Requests` - Simply, if ICMP is not allowed, attackers will not be able to utilize it.

2. `Inspect ICMP Requests and Replies for Data` - Stripping data, or inspecting data for malicious content on these requests and replies can allow us better insight into our environment, and the ability to prevent this data exfiltration.
