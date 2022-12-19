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

Within the startup files of `h1` to `h3` there is no direct assignment of IP addresses from within the file, as shown below
```sh
###############################
#
# h2.startup
#
###############################

ip link set dev eth0 address 02:02:02:02:02:02
dhclient eth0
```

Instead there is a call to the DHCP server to assign the host an ip address. within the `dnsmasq.d` directory there is a config file that sets up the settings for the DHCP server. In the `lab.conf` file it shows that `h1` and `h2` have assigned IP addresses but `h3` does not. This is set up within the DHCP config file
```sh
# ~~~~ r a n g e s (for arbitrary mac addressses) ~~~~~
# dhcp address range for allocation (3 minute lease)
dhcp-range=192.168.97.50,192.168.97.60,255.255.255.192,3m

# ~~~  h o s t s (for known mac addressses) ~~~~~~~~~~~~~~~~~
dhcp-host=02:01:01:01:01:01,h1,192.168.97.41,5m
dhcp-host=02:02:02:02:02:02,h2,192.168.97.42,5m
```

Machines `h4` and `h5` have statically assigned IP addresses within their startup files
``` sh
###############################
#
# h5.startup
#
###############################

# configure this host's MAC address and ip address
# also prevent it using arp
ip link set dev eth0 address 02:05:05:05:05:05
ip link set dev eth0 arp off
ip addr add dev eth0 192.168.97.5/26
ip link set dev eth0 up


###############################
#
# h4.startup
#
###############################

# configure this host's ip address
ip link set dev eth0 address 02:04:04:04:04:04
ip link set dev eth0 arp off
ip addr add dev eth0 192.168.97.4/26
ip link set dev eth0 up

# put a static entry for h5 into our arp cache
ip neigh add 192.168.97.5 lladdr 02:05:05:05:05:05 dev eth0
```

The DHCP servers network card is configured in the `interfaces` file, shown below:
``` sh
# Used by ifup(8) and ifdown(8). See the interfaces(5) manpage or
# /usr/share/doc/ifupdown/examples for more information.

# The loopback network interface
auto lo 
iface lo inet loopback

# eth0 
auto eth0
iface eth0 inet static
	address 192.168.97.1
	netmask 255.255.255.192
	broadcast 192.168.97.63
	gateway 192.168.97.62
	hwaddress ether 02:d8:d8:d8:d8:d8
```
This sets up the IP, MAC, subnet mask, broadcast and gateway address


### h1
IP from the DHCP server (known mac address)
- **IP:** 192.168.97.41
- **MAC:** 02:01:01:01:01:01

### h2
IP from the DHCP server (known mac address)
- **IP:** 192.168.97.42
- **MAC:** 02:02:02:02:02:02

### h3
IP from the DHCP server (arbitrary mac address)
- **IP:** Can range from .50 to .60 (192.168.97.52 for example)
- **MAC:** 02:03:03:03:03:03

### h4
IP from h4 startup file
- **IP:** 192.168.97.4
- **MAC:** 02:04:04:04:04:04

### h5
IP from h5 startup file
- **IP:** 192.168.97.5
- **MAC:** 02:05:05:05:05:05

### dh
IP from `interfaces` file and brought up using `ifup` command
- **IP:** 192.168.97.1
- **MAC:** 02:d8:d8:d8:d8:d8


## DHCP Section
#### Ensure that the packet capture has completed and look at the PCAP file
```sh
systemctl status tcpdump20@sw0
```
- Looking at the PCAP file we can see how DHCP assigns the IP addresses to the various machines
- First a DHCP Discover message is sent to locate the IP of the DHCP Sever
- The DHCP server then responds with a DCHP Offer containing available addresses and options 
- The machine then sends the DHCP server a DHCP Request where it requests and IP address
- Finally the DHCP server responds with a DHCP Ack where the server responds with the IP configuration data
- Within the pcap file there are also some ARP request and an ICMP ping request, I assume these are to check if there are any other machines on the network that already are allocated IPs within the range that the DHCP server can offer. 

#### Change the default gateway
- Change the default gateway of the server to the IP address of the DHCP server (192.168.97.1)


## ARP Section

#### Capture packets passing through the switch (a0)
``` sh
tcpdump -i sw0 -w /hostlab/.output/a0-sw0-02.pcap
```

#### Look at the ARP cache of h3
- There are 2 ways to do this
- The old way is to use `arp -n` the new way is to use `ip neigh show`
- the `-n` in the arp command stops the addresses being resolved into host names

**arp -n**
``` bash
root@h3:~# arp -n
Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.97.41            ether   02:01:01:01:01:01   C                     eth0
192.168.97.1             ether   02:d8:d8:d8:d8:d8   C                     eth0
```

**ip neigh show**
``` bash
root@h3:~# ip neigh show
192.168.97.41 dev eth0 lladdr 02:01:01:01:01:01 STALE
192.168.97.1 dev eth0 lladdr 02:d8:d8:d8:d8:d8 REACHABLE
```


#### On h3, quickly ping the IP address of each machine in turn, together with one non-existent address.
- Since we are pinging an IP address, ARP is going to have to discover the corresponding MAC address so it can send an ethernet frame to the correct NIC.
- This can be done with a simple bash script
``` bash
for i in 1 2 3 4 5 41 42 52 101;  
do  
	ping -c1 192.168.97.${i};  
done;
```

- After this runs look at what is in h3 ARP table
``` bash
root@h3:~# ip neigh show
192.168.97.2 dev eth0  FAILED
192.168.97.3 dev eth0  FAILED
192.168.97.1 dev eth0 lladdr 02:d8:d8:d8:d8:d8 REACHABLE
192.168.97.4 dev eth0  FAILED
192.168.97.5 dev eth0  FAILED
192.168.97.42 dev eth0 lladdr 02:02:02:02:02:02 REACHABLE
192.168.97.62 dev eth0  FAILED
192.168.97.41 dev eth0 lladdr 02:01:01:01:01:01 REACHABLE
```

- As you can see multiple of these addresses failed to ping
- .101, .2 and .3 failed to ping as they do not exist (it seems .101 shows up as .62 in the ARP table as .62 would be the last address that can be assigned to a machine)
- Interesting thing is that h3 is unable to ping .4 and .5 - This is due to how it is set up in the h4 and h5 startup files, both contain this line:
``` sh
ip link set dev eth0 arp off
```

- This disables the ARP protocol for h4 and h5. So h4 and h5 will broadcast an ARP request or reply to an ARP request from another machine.
- h4 and h5 will never be able to populate its ARP 
