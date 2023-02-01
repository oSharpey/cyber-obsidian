go# Lab07: UDP

### Take a tcpdump on machine m2, m7, gw
``` bash
tcpdump -v -i eth0 -w /hostlab/m2-d1.pcap
tcpdump -v -i eth0 -w /hostlab/m7-d1.pcap
tcpdump -v -i eth0 -w /hostlab/gw-d1.pcap
```

### Generate UDP traffic and send it to gwW
- Note use the hub D IP address of gwW (172.28.97.46) 
``` sh
nc -u 172.28.97.46 51966
```
- Once you try to send UDP traffic it will receive a ICMP destination unreachable packet and quit out of the nc session

### Set up listeners
``` sh
nc -l -p 51966  # On gwW
nc -l -p 57005  # on m7
```
These 2 commands are wrong as it does not specify UDP

``` sh
nc -u -l -p 51966  # On gwW
nc -u -l -p 57005  # on m7
```

### Hex values of port numbers 
Port 51966: 0xcafe
Port 57005: 0xdead

### Send UDP Traffic to M7 from M1 and from m1 to gwW
``` sh
echo "hello m7" | nc -u -q0 172.21.62.107 57005
echo "hello gwW" | nc -u -q0 172.28.97.40 51966
```

- `nc -w` &`nc -q0` : provides a timeout time 

### File transfer with UDP
```bash
# on the receiving machine 
nc -nulvp 49374 > out.txt

# on the sending machine
nc -u -q0 <ip of client> 46374 < /dir/file 

```








