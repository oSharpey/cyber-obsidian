### Take a tcpdump on machine m2, m7, gw
``` bash
tcpdump -v -i eth0 -w /hostlab/m2-d1.pcap
tcpdump -v -i eth0 -w /hostlab/m7-d1.pcap
tcpdump -v -i eth0 -w /hostlab/gw-d1.pcap
```

### Generate UDP traffic and send it to gwW
- Note use the hub D IP address of gwW (172.28.97.46) 
``` 
nc -u 172.28.97.46 51966
```
- Once you try to send UDP traffic it will receive a ICMP destination unreachable packet and quit out of the nc session

### Set up listeners
```
nc -l -p 51966  # On gwW
nc -l -p 57005  # on m7
```
These 2 commands are wrong as it does not specify UDP

```
nc -u -l -p 51966  # On gwW
nc -u -l -p 57005  # on m7
```

### Hex values of port numbers 
Port 51966: 0xcafe
Port 57005: 0xdead

### Send UDP Traffic to M7 from M1 and from m1 to gwW
```
echo "hello m7" | nc -q0 172.21.62.107 57005
echo "hello gwW" | nc -q0 172.28.97.40 51966
```








