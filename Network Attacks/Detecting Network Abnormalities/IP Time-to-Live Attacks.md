Time-to-Live attacks are primarily utilized as a means of evasion by attackers. Basically speaking the attacker will intentionally set a very low TTL on their IP packets in order to attempt to evade firewalls, IDS, and IPS systems. These work like the following. 


1. The attacker will craft an IP packet with an intentionally low TTL value (1, 2, 3 and so on).
   
2. Through each host that this packet passes through this TTL value will be decremented by one until it reaches zero.
   
3. Upon reaching zero this packet will be discarded. The attacker will try to get this packet discarded before it reaches a firewall or filtering system to avoid detection/controls.

4. When the packets expire, the routers along the path generate ICMP Time Exceeded messages and send them back to the source IP address.