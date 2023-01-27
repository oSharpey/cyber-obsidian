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

#### CANNOT PING THROUGH GWX AS GWX FIREWALL ONLY CONFGIGURED FORWARD CHAIN TO ALLOW TCP AND UDP PACKETS, WILL DROP ALL ICMP PACKETS ####


## Rule to permit IMCP traffic on gwX from eth0 to eth1
iptables -A FORWARD -i eth0 -o eth1 -s 0.0.0.0/0 -d 0.0.0.0/0 -p icmp -j ACCPET

## Rule to permit IMCP traffic on gwX from eth1 to eth0
iptables -A FORWARD -i eth1 -o eth0 -s 0.0.0.0/0 -d 0.0.0.0/0 -p icmp -j ACCPET

### THE RULES ABOVE ALLOW ICMP PACKETS THROUGH GWX ###

## Delete these rules and replace with 

iptables -A FORWARD \ 
	-i eth0 -o eth1 \
	-s 172.21.62.96/27 -d 172.28.97.40/29 \
	-p icmp --icmp-type echo-request \
	-j ACCEPT

iptables -A FORWARD \
	-i eth1 -o eth0 -s \
	172.28.97.40/29 -d 172.21.62.96/27 \
	-p icmp --icmp-type echo-reply \
	-j ACCEPT

# Set up udp nc listener on m6
nc -u -nvlp 53
# Connect on m1
nc -u 172.21.62.106 53

# Only accept UDP traffic from LANx on port 53 and to LANx on potr 53
iptables -A FORWARD \
	-i eth0 -o eth1 \
	-s 172.28.97.40/29 -d 0.0.0.0/0 \
	-p udp \
	--dport 53 \
	-j ACCEPT

iptables -A FORWARD \
	-i eth1 -o eth0 \
	-s 0.0.0.0/0 -d 172.21.62.96/27 \
	-p udp \
	--sport 53 \
	-j ACCEPT



#only allow tcp web traffic
iptables -A FORWARD \
	-i eth1 -o eth0 \
	! -s 172.28.97.40/29 -d 172.28.97.40/29 \
	-p tcp \
	--sport 80 \
	-j ACCEPT

iptables -A FORWARD \
	-i eth0 -o eth1 \
	-s 172.28.97.40/29 ! -d 172.28.97.40/29 \
	-p tcp \
	--dport 80 \
	-j ACCEPT
```