# Lab 05: Addressing & Subnets

## lab.conf
``` bash
##############################################
LAB_DESCRIPTION="IP4 subnets"
LAB_VERSION="lab05-ip-subnets.2022.rc2"
LAB_AUTHOR="Peter Norris"
LAB_EMAIL="pn@warwick.ac.uk"
LAB_MACHINES=m1,m2,m6,m7,m12,m13,m16,m17,gwW,gwX,gwY,gwZ,gw,dns1,dns2
##############################################

# unlike previous labs where we explicitly configured switches, this lab
# utilises standard netkit hubs to create so al host on a hub see all traffic
# on that hub
# We also tap put to the host and thus (probably) the real Internet.
# BE CAREFUL who you scan!!!!!!!

# create the subnets
gwW[0]=W
m1[0]=W
m2[0]=W

gwX[0]=X
m6[0]=X
m7[0]=X

gwY[0]=Y
m12[0]=Y
m13[0]=Y

gwZ[0]=Z
m16[0]=Z
m17[0]=Z

# link the subnets via the twin carded gateway machines
gwW[1]=D
gwX[1]=D
gwY[1]=Z
gwZ[1]=D
gw[0]=D

# provide dns - not essential for this lab but useful none-the-less
dns1[0]=D
dns2[0]=D

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
#                   Y                         |
#           172.19.78.0/23                    |     .220 +-----+
#    --+------------+-------------+--         +----------|dns1 |
#      |            |             |           |          +-----+
#      |78.112      |79.213       |79.254     |
#   +-----+      +-----+       +-----+        |
#   | m12 |      | m13 |       | gwY |        |     .221 +-----+
#   +-----+      +-----+       |     |        +----------|dns2 |
#                              +--+--+        |          +-----+
#                   Z             |.47.253    |
#            172.17.32.0/20       |           |
#    --+------------+-------------+--         |
#      |            |             |           |
#      |33.116      |44.217       |.47.254    |
#   +-----+      +-----+       +-----+        |
#   | m16 |      | m17 |       | gwZ |.58     |
#   +-----+      +-----+       |     |--------+
#                              +-----+        |
```


## Commands
#### Start capturing the traffic from the gateway machine
- This can be on any of the gateway machines in the network
- For example for subnet W you would capture traffic on gwW

``` sh
tcpdump -i eth0 -w /hostlab/.output/gwW-eth0-01.pcap
```

#### Ping between 2 machines on the same subnet
``` sh
# Ping m2 from m1
ping -c1 172.28.97.42

# Ping m1 from m2
ping -c1 172.28.97.41
```

#### Look at the IP header in RFC 791 and compare it to a particular ping
IP Header in RFC 791
```
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |Flags|      Fragment Offset    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |         Header Checksum       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                       Source Address                          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Destination Address                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

The header within the ping contains no options and no padding
- Options: specify some special features like specific routing instructions
- Padding: ensures it is a multiple of 32 bits
These parts of the header are rarely ever used

#### Change the config for subnet w
``` sh
ip addr add <new IP with Mask> dev <network card> # eg 146.227.150.64/26
ip addr del <old ip with mask> dev <network card>
ip route replace default via <ip of gateway> # change gateway for m1 & m2
```
- Note you will also need to change the routing table for all other gateways in order for it to be pinged by other machines


#### Why can the machines not go on 146.227.150.64/25
- Because .64 would not be a valid subnet. 
- /25 would give you a range of 146.227.150.0 - 146.227.150.128
- This means .64 would be in the middle of this range, so it is the wrong notation 

#### Add a new machine to the lab within subnet x
- Create a file `foo.startup`
- Create a directory called `foo`
``` sh
# Within foo.startup:
ip link set dev eth0 address 02:0f:0f:0f:0f:0f
ip addr add 172.21.62.108/27 dev eth0
ip link set up dev eth0
ip route add default via 172.21.62.126

# Within lab.conf
foo[0]=X
```

