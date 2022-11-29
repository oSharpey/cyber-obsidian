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
	- Blocking specific ports
- DMZ
	- Related with architecture - how you design your network
	- Not hardware or software
- Protocol Switching
	- Defining the rules according to your protocol
- Dynamic Packet filtering
	- Used with IDS and IPS
	- Dynamically decides which rules apply at that specific time

## Firewalls at different OSI layers
- Stateful and stateless network layer firewalls
	- Can detect TCP/UDP
- NGFW
	- Can detect application protocols

### Stateless Firewall
- Basic packet filter
- Does not remember the packets sent
- Does not examine a packet is stand-alone or part of a message stream
- Does not monitor the connection status
- Use less memory than stateful
- For example no idea if a TCP handshake is happening
- Looks at one packet and does not care about any history

### Stateful firewall
- Keeps tack of connections and data streams passing through
- Can identify packets that are part of established connection
- Better to prevent network attacks that look to exploit existing connections
- Can track connection-less protocols like UDP
- A bit slower than stateless firewalls
- Looks at one packet in the context of the history before it 

### NGFW
- Can read application data alongside IP header
- can detect application layer protocols
- Can set proxy rules
- Slower than Network layer firewalls
