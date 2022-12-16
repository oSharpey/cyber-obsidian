# Lab 04 DHCP & ARP

## lab.conf
``` sh
##############################################
LAB_DESCRIPTION="DHCP and ARP - five hosts, one DHCP server, one switch"
LAB_VERSION="lab04-dhcp-arp.2022.rc2"
LAB_AUTHOR="Peter Norris"
LAB_EMAIL="pn@warwick.ac.uk"
LAB_MACHINES=a0,dh,h1,h2,h3,h4,h5
##############################################

# as in lab03, a0 is an access layer switch 
# we will not give a0 an IP4 address - it only functions as a L2 LAN device.
# hosts with get their IP addresses in various ways

# 8 port access layer switch a0 - we give it 8 ethernet cards
# we will use these 6 nics to connect to hosts
a0[1]=a01
a0[2]=a02
a0[3]=a03
a0[4]=a04
a0[5]=a05
a0[6]=a06

# we could use these 2 nics to connect to other switches 
a0[30]=a0-d0
a0[31]=a0-d1

# we will connect five hosts and the DHCP server to the a0 switch
h1[0]=a01
h2[0]=a02
h3[0]=a03
h4[0]=a04
h5[0]=a05

dh[0]=a06
dh[mem]=256

###############################################
# corresponding network diagram
#
#         (192.168.97.0/26)                |        |   
#   +--------------------------------------+--------+---+       
#   |                   a0                eth30    eth31|        
#   |                                                   |        
#   | eth1     eth2     eth3     eth4     eth5     eth6 |        
#   +--+--------+--------+--------+--------+--------+---+       
#      |        |        |        |        |        |   
#      |        |        |        |        |        |                              
#      |(.41)   |(.42)   |(.?)    |.4      |.5      |.1                                  
#   +--+---+ +--+---+ +--+---+ +--+---+ +--+---+ +--+---+                              
#   | eth0 | | eth0 | | eth0 | | eth0 | | eth0 | | eth0 |                                
#   |      | |      | |      | |      | |      | |      |                                
#   |  h1  | |  h2  | |  h3  | |  h4  | |  h5  | |  dh  |                 
#   +------+ +------+ +------+ +------+ +------+ +------+               
#
# 
```


## Machines
Like Lab-03 we have the same switch with 8 ports, however this time we have 6 ports connected to machines. Machines `h1` to `h5` are configured as regular host. `dh` is configured as a DHCP server.  

Within the startup files of `h1` to `h5` there is no direct assignment of IP addresses from within the file, as shown below
```sh
###############################
#
# h2.startup
#
###############################

ip link set dev eth0 address 02:02:02:02:02:02
dhclient eth0
```

Instead there is a call to the 