# Firewalls

## What is a Firewall
- Combo of hardware and software
- Generally allows packets that pass specific security restrictions
- Can permit deny, encrypt, decrypt and proxy traffic

## Firewall Config
- If a firewall is not configured properly, it will not do any good
- Common config id default-deny
	- If it does not match the rule it will automatically deny the packet
	- Whitelisting rule
- So opt-in for default-allow
	- In essence a blacklisting rule

## Types of firewalls
- Host Based
	- Implemented on a single machine
	- usually a software implementation
- Network bases
	- protect private network from public networks
	- designed to protect a whole network

## Firewall technologies
- Access control list
	- define who can access that particular network
- Port Security
	- define control rules with port numbers
	- blocking specific ports
- DMZ
	- Related with architecture - how you design your network
	- Not hardware or software
- Protocol Switching
	- Defining the rules according to your protocol 