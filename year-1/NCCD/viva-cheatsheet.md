# Viva Cheatsheet

## Lab 3 & 4 (Layer 2)

``` sh
# Dump all traffic passing through a network card
tcpdump -i <network card> -w /hostlab/.output/a0-sw0-01.pcap
```

```sh
# SHOW IP AND MAC FOR A NETWORK CARD
ip addr show dev <Network card>  # eg ip addr show dev eth0 
```

```sh
# CLEAR ARP TABLE ON THE MACHINE
ip -s -s neigh flush all   # deletes the whole a machines ARP table
```

```sh
# COMMAND TO SET AGING TIME
ip link set sw0 type bridge ageing_time 2000    # Set ageing time to 20s 
```

```sh
# SHOW THE AGING TIME OF THE CACHE
ip -s -j -p link show dev sw0 | grep "ageing_time" # Show the ageing time of the switches cache
```

```sh
# SHOW THE ARP TABLE
ip neigh show #OR
arp -n
```

First a DHCP Discover message is sent to locate the IP of the DHCP Sever
- The DHCP server then responds with a DCHP Offer containing available addresses and options 
- The machine then sends the DHCP server a DHCP Request where it requests and IP address
- Finally the DHCP server responds with a DHCP Ack where the server responds with the IP configuration data
- Within the pcap file there are also some ARP request and an ICMP ping request, I assume these are to check if there are any other machines on the network that already are allocated IPs within the range that the DHCP server can offer. 

## Lab 5 & 6 (Layer 3)

```sh
# Change the ip address of the network card
ip addr add <new IP with Mask> dev <network card> # eg 146.227.150.64/26
ip addr del <old ip with mask> dev <network card>
ip route replace default via <ip of gateway> # change gateway for m1 & m2
```

#### Why can the machines not go on 146.227.150.64/25
- Because .64 would not be a valid subnet. 
- /25 would give you a range of 146.227.150.0 - 146.227.150.128
- This means .64 would be in the middle of this range, so it is the wrong notation 

```sh
# Tracing a ping
ping -c1 172.21.62.106 -R 
traceroute -n 172.21.62.106
```
`ping -R` means "Record Route" - It uses the RECORD_ROUTE option in the IP options/parameters. This means that it keeps track of every IP of the interface that the packet leaves on

Traceroute hows route a packet takes (IPs of the interface a packet enters on)
`-n` means that it wont try to resolve hostnames

`ping -R` also displays the return route back from .106 traceroute does not do this



## Lab 7 & 8 (Layer 4)

```sh
# Set up UDP listeners and senders
nc -u 172.28.97.46 51966
nc -nvulp 51966
```

```sh
# using timeouts 
echo "hello m7" | nc -u -w 3 172.21.62.107 57005
echo "hello gwW" | nc -u -q0 172.28.97.40 51966
```

```sh
# UDP file transfer
nc -nulvp 49374 > out.txt # on the receiving machine 
nc -u -q0 <ip of client> 46374 < /dir/file # on the sending machine
```

```sh
# look at udp/tcp connetctions
watch -n1 netstat -an4 # netstat is depricated
watch -n1 ss -anut4
```