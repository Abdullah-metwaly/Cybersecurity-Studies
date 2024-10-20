



An attacker might conduct these packet crafting attacks towards the source and destination IP addresses for many different reasons or desired outcomes. Here are a few that we can look for:

1. `Decoy Scanning` - Simply put, when an attacker wants to gather information, they might change their source address to be the same as another legitimate host, or in some cases entirely different from any real host. This is to attempt to evade IDS/Firewall controls, and it can be easily observed.
		In the case of decoy scanning, we will notice some strange behavior.
	1. `Initial Fragmentation from a fake address`
	2. `Some TCP traffic from the legitimate source address`
		As such, another simple way that we can prevent this attack beyond just detecting it through our traffic analysis efforts is the following.
	1. `Have our IDS/IPS/Firewall act as the destination host would` - In the sense that reconstructing the packets gives a clear indication of malicious activity.    
	2. `Watch for connections started by one host, and taken over by another` - The attacker after all has to reveal their true source address in order to see that a port is open. This is strange behavior and we can define our rules to prevent it.


2. `Random Source Attack DDoS` - Through random source crafting an attacker might be able to send tons of traffic to the same port on the victim host. This in many cases, is used to exhaust resources of our network controls or on the destination host. in which many hosts will ping one host which does not exist, and the pinged host will ping back all others and get no reply.

	![[random-source.png]]
	In many real world cases, like a web server, we may have many different users utilizing the same port. However, these requests are contrary of our indicators. Such that they will have different lengths and the base ports will not exhibit this behavior.

3. `LAND Attacks` - LAND Attacks operate similarly to Random Source denial-of-service attacks in the nature that the source address is set to the same as the destination hosts. In doing so the attacker might be able to exhaust network resources or cause crashes on the target host.
		LAND attacks operate through an attacker spoofing the source IP address to be the same as the destination. These denial-of-service attacks work through sheer volume of traffic and port re-use. Essentially, if all base ports are occupied, it makes real connections much more difficult to establish to our affected host.
   
4. `SMURF Attacks` - Similar to LAND and Random Source attacks, SMURF attacks work through the attacker sending large amounts of ICMP packets to many different hosts. However, in this case the source address is set to the victim machines, and all of the hosts which receive this ICMP packet respond with an ICMP reply causing resource exhaustion on the crafted source address (victim).
		SMURF Attacks are a notable distributed denial-of-service attack, in the nature that they operate through causing random hosts to ping the victim host back. Simply put, an attacker conducts these like the following
		-----------------------------------------------------------------------------------------------------
		One of the things we can look for in our traffic behavior is an excessive amount of ICMP replies from a single host to our affected host. Sometimes attackers will include fragmentation and data on these ICMP requests to make the traffic volume larger.


![[smurf.webp]]

In this link [SMURF](https://techofide.com/blogs/what-is-smurf-attack-what-is-the-denial-of-service-attack-practical-ddos-attack-step-by-step-guide/) more explanation on the SMURF attack 




6. `Initialization Vector Generation` - In older wireless networks such as wired equivalent privacy, an attacker might capture, decrypt, craft, and re-inject a packet with a modified source and destination IP address in order to generate initialization vectors to build a decryption table for a statistical attack. These can be seen in nature by noticing an excessive amount of repeated packets between ho