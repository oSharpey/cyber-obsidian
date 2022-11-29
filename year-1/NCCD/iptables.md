# iptables

## What is iptables
- manage Linux firewalls
- manage packet filtering ans NAT rules
- Contains 4 built in tables
- each table contains a chain
- each chain can have one or more rules

## Tables
- Filter table
- NAT table
- Mangle table
- Raw table

### Filter Table
- Default table
	- INPUT chain: Incoming to firewall. 
		- Packet not generated on device coming from other source and the device is receiving the packer
	- OUTPUT chain: Outgoing from firewall. 
		- Device generated the packet and is send through one of the interfaces
	- FORWARD Chain: packet for other network card or routed packets

### NAT Table
- Handle Nat Related rules
- Output chain: NAT for locally generated packers

### Mangle table
- specialised packet alteration like QoS bits and TCP header

### Raw table
- Used for configuration exceptions

## iptables rules
- contains criteria and targets for packets
	- Target - Accept, reject, drop etc
- If a rule matched, iptables execute the rile and slip other rules
- If a rule does not match it moves to the next rule
- So, rules are first come fist serve

## Target Values for Firewall Rules
- ACCEPT - Firewall lets packet through
- DROP - Firewall drops the packet without error message
- REJECT - DROP and sends an error message
- There are more target values. some values are specific to particular tables.


**ORDER OF THE RULES MATTERS**
First thing that matches will hit


Network Firewall - Put rules on forward chain
Host Firewall - Put rules on input and output chain

consider things in a particular order
layer 2 then layer 3 then layer 4

## Commands

```bash
iptables -P FORWARD DROP   # Convert defaut_accept to default_drop

iptables -A FORWARD \
	-i eth0 \
	-o eth1 \
	-s 172.28.97.40/29 \
	! -d 172.28.97.40/29 \
	-p tcp \
	--dport 80 \
	! --sport 1:1023 \



```