> We might discern additional aberrant behaviors within the ARP requests and replies. It is common knowledge that poisoning and spoofing form the core of most ARP-based `denial-of-service (DoS)` and `man-in-the-middle (MITM)` attacks. However, adversaries could also exploit ARP for information gathering. Thankfully, we possess the skills to detect and evaluate these tactics following similar procedures.


Some typical red flags indicative of ARP scanning are:

1. Broadcast ARP requests sent to sequential IP addresses (.1,.2,.3,...)
2. Broadcast ARP requests sent to non-existent host
3. Potentially, an unusual volume of ARP traffic originating from a malicious or compromised host

![ARP Scanning](https://academy.hackthebox.com/storage/modules/229/ARP_Scan_1.png)

It's possible to detect that indeed ARP requests are being propagated by a single host to all IP addresses in a sequential manner. This pattern is symptomatic of ARP scanning and is a common feature of widely-used scanners such as `Nmap`.

Furthermore, we may discern that active hosts respond to these requests via their ARP replies. This could signal the successful execution of the information-gathering tactic by the attacker.



## Identifying Denial-of-Service

> An attacker can exploit ARP scanning to compile a list of live hosts. Upon acquiring this list, the attacker might alter their strategy to deny service to all these machines. Essentially, they will strive to contaminate an entire subnet and manipulate as many ARP caches as possible. This strategy is also plausible for an attacker seeking to establish a man-in-the-middle position.


![ARP DoS](https://academy.hackthebox.com/storage/modules/229/ARP_DoS_1.png)

		Promptly, we might note that the attacker's ARP traffic may shift its focus towards declaring new physical addresses for all live IP addresses. The intent here is to corrupt the router's ARP cache.



### Responding To ARP Attacks


1. `Tracing and Identification`: First and foremost, the attacker's machine is a physical entity located somewhere. If we manage to locate it, we could potentially halt its activities. On occasions, we might discover that the machine orchestrating the attack is itself compromised and under remote control.

2. `Containment`: To stymie any further exfiltration of information by the attacker, we might contemplate disconnecting or isolating the impacted area at the switch or router level. This action could effectively terminate a DoS or MITM attack at its source.

