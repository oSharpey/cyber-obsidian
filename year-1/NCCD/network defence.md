# Network Defence

## Defence in depth
### What is it?
- A security strategy they involves implementung

### How to implement it
- Segment the network - Break the network into smaller zones with their own security controls
- Firewalls - Used to control access to a network by allowing to only authorised traffic
- Encryption - Use of encrypted protocols

### How does it work
- First layer - Perimeter firewall, controls access to the organisations network. Need to secure the first access point of the network
- Second layer - Segment the network into different zones - DMZ etc
- IDS/IPS used to monitor network traffic and block suspicious activity
- Use proper authentication methods to control access
- Have an incident team and disaster recovery plan

### Some common mistakes
- Over-reliance on a single security control
- Not segmenting the network
- Not testing the security controls - Directionality is important
- Not monitoring the network
- Not keeping software up to date
- Not considering the human factor - humans are the weakest link

## Network Segmentation
### Common types of network segmentation
- DMZ - a network segment that contains public facing services, typically located at the networks perimeter. Perimeter firewall protects this
- Internal DMZ - Used to segment less trusted internal services
- Enterprise Zone - Network segment that contains the most sensitive data and systems. Usually the end user is here
- Management Zone - A admin control zone to monitor the network. 

### How to segment a network
- Identify the types of data and systems that need to be protected
- Identify the types of users and devices that will be accessing the network (or different subnets)
- Identify the network services and devices that will be in each segment
- Decide which network devices and services will be in each segment and how they will be connected
- Implement the segment
- Test and monitor

### Common Mistakes
- Not considering the needs of the business
- Not using a consistent approach 
- Not properly securing the DMZ (perimeter firewall)
- Not properly isolating the segments