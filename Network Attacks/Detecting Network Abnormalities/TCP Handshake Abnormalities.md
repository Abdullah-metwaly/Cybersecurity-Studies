
If this port is open, and in fact able to be connected to, the machine responds with a TCP SYN/ACK to acknowledge that the connection is valid and able to be used. However, we should consider all TCP flags.

|**Flags**|**Description**|
|---|---|
|`URG (Urgent)`|This flag is to denote urgency with the current data in stream.|
|`ACK (Acknowledgement)`|This flag acknowledges receipt of data.|
|`PSH (Push)`|This flag instructs the TCP stack to immediately deliver the received data to the application layer, and bypass buffering.|
|`RST (Reset)`|This flag is used for termination of the TCP connection (we will dive into hijacking and RST attacks soon).|
|`SYN (Synchronize)`|This flag is used to establish an initial connection with TCP.|
|`FIN (Finish)`|This flag is used to denote the finish of a TCP connection. It is used when no more data needs to be sent.|
|`ECN (Explicit Congestion Notification)`|This flag is used to denote congestion within our network, it is to let the hosts know to avoid unnecessary re-transmissions.|


1. Excessive SYN Flags - Right away one of the traffic patterns that we can notice is too many SYN flags. This is a prime example of nmap scanning. Simply put, the adversary will send TCP SYN packets to the target ports. In the case where our port is open, our machine will respond with a SYN-ACK packet to continue the handshake, which will then be met by an RST from the attackers scanner. However, we can get lost in the RSTs here as our machine will respond with RST for closed ports.
2. No Flags - On the opposite side of things, the attacker might send no flags. This is what is commonly referrred to as a NULL scan. In a NULL scan an attacker sends TCP packets with no flags. TCP connections behave like the following when a NULL packet is received.
3. Too Many ACKs - On the other hand, we might notice an excessive amount of acknowledgements between two hosts. In this case the attacker might be employing the usage of an ACK scan. In the case of an ACK scan TCP connections will behave like the following.
4. Excessive FINs - Using another part of the handshake, an attacker might utilize a FIN scan. In this case, all TCP packets will be marked with the FIN flag. We might notice the following behavior from our affected machine.
5. Just too many flags - Let's say the attacker just wanted to throw spaghetti at the wall. In that case, they might utilize a Xmas tree scan, which is when they put all TCP flags on their transmissions. Similarly, our affected host might respond like the following when all flags are set.
