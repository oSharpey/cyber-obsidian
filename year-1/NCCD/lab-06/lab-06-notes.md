# Lab 06: IP Routing

## 06a
### lab.conf
```
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
- All pings 

