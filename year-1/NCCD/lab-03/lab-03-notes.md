# Lab 03 Notes & Commands

## lab.conf
``` sh
##############################################
LAB_DESCRIPTION="bridging / switching - three hosts, one switch"
LAB_VERSION="lab03a-bridge.2022.rc1"
LAB_AUTHOR="Peter Norris"
LAB_EMAIL="pn@warwick.ac.uk"
LAB_MACHINES=a0,h1,h2,h3
##############################################

# a0 is an access layer switch 
# we will not give a0 an IP4 address - it only functions as a L2 LAN device.
# hosts with IP addresses are connected to it - they will all be on the same LAN

# 8 port access layer switch a0 - we give it 8 ethernet cards
# we will use these 6 nics to connect to hosts
a0[1]=a01
a0[2]=a02
a0[3]=a03
a0[4]=a04
a0[5]=a05
a0[6]=a06

# we will use these 2 nics to connect to other switches (in part b of the lab)
a0[30]=a0-d0
a0[31]=a0-d1

# we will only connect three hosts to the a0 switch
h1[0]=a01
h2[0]=a02
h3[0]=a03


###############################################
# corresponding network diagram
#
#         (192.168.97.0/24)                |        |   
#   +--------------------------------------+--------+---+       
#   |                   a0                eth30    eth31|        
#   |                                                   |        
#   | eth1     eth2     eth3     eth4     eth5     eth6 |        
#   +--+--------+--------+--------+--------+--------+---+       
#      |        |        |        |        |        |   
#      |        |        |                   
#      |.1      |.2      |.3                 
#   +--+---+ +--+---+ +--+---+               
#   | eth0 | | eth0 | | eth0 |                
#   |      | |      | |      |                
#   |  h1  | |  h2  | |  h3  |                
#   +------+ +------+ +------+               
#
# The IP addresses are assigned elsewhere (in the startup files)
```


## Machines 

### h1
- **IP:** 192.168.97.1
- **MAC:** 02:61:61:61:61:61

### h2
- **IP:** 192.168.97.2
- **MAC:** 02:62:62:62:62:62

### h2
- **IP:** 192.168.97.3
- **MAC:** 02:63:63:63:63:63

### a0
- Acts as a switch/bridge

## Commands
- Create a new directory to store packet captures
- Dump all traffic passing through the switch (a0)

``` sh
mkdir -p /hostlab/.output  
tcpdump -s0 -i sw0 -w /hostlab/.output/a0-sw0-01.pcap
```



