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

### Set up listeners
```
nc -l -p 51966  # On gwW
nc -l -p 5

```

