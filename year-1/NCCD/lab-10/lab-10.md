- Ping anon from m1:
	- This fails
	- Firewall rule on gw means will not allow any packets in or out of eth1 (the public facing network card)
	- also anon has no routing instructions 

```bash
# add new firewall rule
iptables -A FORWARD -m state --ctstate RELATED,ESTABLISHED -j ACCEPT
iptables -A FORWARD -i eth0 -o eth1 -s 0.0.0.0/0 -d 0.0.0.0/0 -j ACCEPT
```

