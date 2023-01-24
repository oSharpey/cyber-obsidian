# Viva Commands

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


## Lab 5 & 6 (Layer 3)
