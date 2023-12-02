
>The [Domain Name System](https://www.cloudflare.com/learning/dns/what-is-dns/) (`DNS`) translates domain names (e.g., hackthebox.com) to the numerical IP addresses (e.g., 104.17.42.72). DNS is mostly `UDP/53`, but DNS will rely on `TCP/53` more heavily as time progresses. DNS has always been designed to use both UDP and TCP port 53 from the start, with UDP being the default, and falls back to using TCP when it cannot communicate on UDP, typically when the packet size is too large to push through in a single UDP packet. Since nearly all network applications use DNS, attacks against DNS servers represent one of the most prevalent and significant threats today.


```
$ nmap -p53 -Pn -sV -sC 10.10.110.213
```
#nma #dns

## DNS Zone Transfer

>>A DNS zone is a portion of the DNS namespace that a specific organization or administrator manages. Since DNS comprises multiple DNS zones, DNS servers utilize DNS zone transfers to copy a portion of their database to another DNS server. Unless a DNS server is configured correctly (limiting which IPs can perform a DNS zone transfer), anyone can ask a DNS server for a copy of its zone information since DNS zone transfers do not require any authentication. In addition, the DNS service usually runs on a UDP port; however, when performing DNS zone transfer, it uses a TCP port for reliable data transmission.
>>
>>A DNS zone is a portion of the DNS namespace that a specific organization or administrator manages. Since DNS comprises multiple DNS zones, DNS servers utilize DNS zone transfers to copy a portion of their database to another DNS server. Unless a DNS server is configured correctly (limiting which IPs can perform a DNS zone transfer), anyone can ask a DNS server for a copy of its zone information since DNS zone transfers do not require any authentication. In addition, the DNS service usually runs on a UDP port; however, when performing DNS zone transfer, it uses a TCP port for reliable data transmission.>

```
$ dig AXFR @ns1.inlanefreight.htb inlanefreight.htb
```
#dig

```
$ fierce --domain zonetransfer.me
```
#fierce

## Domain Takeovers & Subdomain Enumeration

>The domain name (e.g., `sub.target.com`) uses a CNAME record to another domain (e.g., `anotherdomain.com`). Suppose the `anotherdomain.com` expires and is available for anyone to claim the domain since the `target.com`'s DNS server has the `CNAME` record. In that case, anyone who registers `anotherdomain.com` will have complete control over `sub.target.com` until the DNS record is updated.

```
sub.target.com.   60   IN   CNAME   anotherdomain.com
```


```
$ ./subfinder -d inlanefreight.com -v
```
#subfinder

>An excellent alternative is a tool called [Subbrute](https://github.com/TheRook/subbrute). This tool allows us to use self-defined resolvers and perform pure DNS brute-forcing attacks during internal penetration tests on hosts that do not have Internet access.
```
$ git clone https://github.com/TheRook/subbrute.git >> /dev/null 2>&1
$ cd subbrute
$ echo "ns1.inlanefreight.com" > ./resolvers.txt
$ ./subbrute inlanefreight.com -s ./names.txt -r ./resolvers.txt
```
#git #github #subbrute


```
$ host support.inlanefreight.com
```
#host
>The [can-i-take-over-xyz](https://github.com/EdOverflow/can-i-take-over-xyz) repository is also an excellent reference for a subdomain takeover vulnerability. It shows whether the target services are vulnerable to a subdomain takeover and provides guidelines on assessing the vulnerability.



## DNS Spoofing

>DNS spoofing is also referred to as DNS Cache Poisoning. This attack involves alerting legitimate DNS records with false information so that they can be used to redirect online traffic to a fraudulent website. Example attack paths for the DNS Cache Poisoning are as follows:

- An attacker could intercept the communication between a user and a DNS server to route the user to a fraudulent destination instead of a legitimate one by performing a Man-in-the-Middle (`MITM`) attack.
    
- Exploiting a vulnerability found in a DNS server could yield control over the server by an attacker to modify the DNS records.


#### Local DNS Cache Poisoning

```
$ cat /etc/ettercap/etter.dns
```


```
$ ping -c 4 inlanefreight.com
```
#ping 

```
$ dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>
```
#dnsenum #dns 

```
$ dig axfr <domain.tld> @<nameserver>
$ dig any <domain.tld> @<nameserver>
$ dig ns <domain.tld> @<nameserver>
```
#dig #axfr  #ns 