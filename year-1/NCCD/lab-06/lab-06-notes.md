# Lab 06: IP Routing

## 06a
### lab.conf
``` sh
##############################################
LAB_DESCRIPTION="IP4 routing 'half lab'. Run lab06b for the other half"
LAB_VERSION="lab06a-ip-route.2022.rc2"
LAB_AUTHOR="Peter Norris"
LAB_EMAIL="pn@warwick.ac.uk"
LAB_MACHINES=m1,m2,m6,m7,gwW,gwX,gw
##############################################

# this is the W, X and some of D subnets from lab05

# create the subnets
gwW[0]=W
m1[0]=W
m2[0]=W

gwX[0]=X
m6[0]=X
m7[0]=X

# link the subnets via the twin carded gateway machines
gwW[1]=D
gwX[1]=D
gw[0]=D

# this will connect gw1 to a virtual nic in the host and thus out to the internet
gw[1]=tap01,192.168.100.1,192.168.100.2

#
#                   W
#            172.28.97.40/29
#    --+------------+-------------+--              D
#      |            |             |           10.227.150.0/24
#      |.41         |.42          |.46        +-----------------
#   +-----+      +-----+       +-----+        |
#   | m1  |      | m2  |       | gwW |.55     |
#   +-----+      +-----+       |     |--------+               192.168.100.0/24
#                              +-----+        |           --+-----------------+-- -
#                   X                         |             |.2             .1|
#            172.21.62.96/27                  |     .254 +--+--+          +---+---+
#    --+------------+-------------+--         +----------| gw  |          |  host |
#      |            |             |           |          +-----+          +---+---+
#      |.106        |.107         |.126       |                               |
#   +-----+      +-----+       +-----+        |                               +-- >>> Internet >>>
#   | m6  |      | m7  |       | gwX |.56     |
#   +-----+      +-----+       |     |--------+
#                              +-----+        |
#                                             |
# ^^^^ above the line is lab06a ^^^^          | 
#                                             |
```

### Commands
#### Capture traffic on m2, m7 and gw
```sh
tcpdump -i eth0 -w /hostlab/.output/m1-eth0-01.pcap
tcpdump -i eth0 -w /hostlab/.output/m2-eth0-01.pcap
tcpdump -i eth0 -w /hostlab/.output/gw-eth0-01.pcap
```

#### Pings
- All pings, despite containing the full IP addresses for each packet, use the destination MAC addresses of the nearest gateway to work. 
- When you ping machine m7 from m2. m2's ARP table would map m7's IP address to 


#### Traceroute vs Ping
##### Ping
```sh
ping -c1 172.21.62.106 -R
traceroute -n 172.21.62.106
```
- `ping -R` means "Record Route" - It uses the RECORD_ROUTE option in the IP options/parameters. This means that it keeps track of every IP of the interface that the packet leaves on

``` sh
--- 172.21.62.106 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.706/0.706/0.706/0.000 ms
root@m1:~# ping -c1 172.21.62.106 -R
PING 172.21.62.106 (172.21.62.106) 56(124) bytes of data.
64 bytes from 172.21.62.106: icmp_seq=1 ttl=62 time=0.408 ms
RR: 172.28.97.41
	10.227.150.55
	172.21.62.126
	172.21.62.106
	172.21.62.106
	10.227.150.56
	172.28.97.46
	172.28.97.41

--- 172.21.62.106 ping statistics ---
```

- As displayed above on the route to `172.21.62.106`  -  `172.28.97.46` is not displayed as it entered on that interface on gwW, the .55 address is shown as it left that interface on gwW

##### Traceroute
- Shows route a packet takes (IPs of the interface a packet enters on)
- Uses time-to-live (TTL) to figure out the route of the packet
- Traceroute is slower, but more reliable 

