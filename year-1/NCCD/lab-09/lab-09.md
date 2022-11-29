## General iptables commands
```bash
#### CONVERT DEFAULT_ACCEPT TO DEFAULT_DROP ####
iptables -P FORWARD DROP   

#### CONFIGURE RULE TO GET PACKETS FROM CLIENT TO SERVER ####
iptables -A FORWARD \
	-i eth0 \
	-o eth1 \
	-s 172.28.97.40/29 \
	! -d 172.28.97.40/29 \
	-p tcp \
	--dport 80 \
	! --sport 1:1023 \
	-j ACCEPT

#### CONFIGURE RULE TO GET PACKETS FROM SERVER TO CLIENT ####
iptables -A FORWARD \
	-i eth1 \
	-o eth0 \
	! -s 172.28.97.40/29 \
	-d 172.28.97.40/29 \
	-p tcp \
	--sport 80 \
	! --dport 1:1023 \
	-j ACCEPT


#### LIST A PARTICULAR CHAIN ####
iptables -L FORWARD


#### LIST WITH LINE NUMBERS AND MORE INFO (VERBOSE) ####
iptables -nvL FORWARD

```


## Lab09 Commands 
``` bash
## List policy table of gwW
iptables -nvL 


## Dump Traffic on m7 amd gw
tcpdump -v -i eth0 -w /hostlab/.dumps/m7-eth0-d1.pcap
tcpdump -v -i eth0 -w /hostlab/.dumps/gw-eth0-d1.pcap
tcpdump -v -i eth1 -w /hostlab/.dumps/gw-eth1-d1.pcap

## CANNOT PING 





```