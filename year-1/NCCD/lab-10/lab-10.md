- Ping anon from m1:
	- This fails
	- Firewall rule on gw means will not allow any packets in or out of eth1 (the public facing network card)

```bash
# add new firewall rule
iptables -A FORWARD -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A FORWARD -i eth0 -o eth1

```