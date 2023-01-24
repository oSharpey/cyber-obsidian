# Viva Commands

## Lab 3

#### Dump all traffic passing through the switch (a0)
``` sh
tcpdump -s0 -i sw0 -w /hostlab/.output/a0-sw0-01.pcap
```

#### shows the ip and mac for the eth0 card
``` sh

ip addr show dev eth0    # shows the ip and mac for the eth0 card
```


