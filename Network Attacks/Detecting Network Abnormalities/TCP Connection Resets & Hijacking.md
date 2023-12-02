Unfortunately, TCP does not provide the level of protection to prevent our hosts from having their connections terminated or hijacked by an attacker. As such, we might notice that a connection gets terminated by an RST packet, or hijacked through connection hijacking. 


1. TCP Connection Termination -  They might employ a simple TCP RST Packet injection attack, or TCP connection termination in simple terms.
2. TCP Connection Hijacking - The attacker will need to block ACKs from reaching the affected machine in order to continue the hijacking. They do this either through delaying or blocking the ACK packets. As such, this attack is very commonly employed with ARP poisoning, and we might notice the following in our traffic analysis.
