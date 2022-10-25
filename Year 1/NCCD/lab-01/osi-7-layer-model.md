# The OSI 7 Layer Model
- This is a ***model*** not a ***mandate***
- Made up of  7 layers (sometimes not all are used hence statement above)

**Layer 7:  Application**
- Human Interaction layer
- Handles protocols such as HTTP(s), SMTP(s), NTP, FTP

**Layer 6: Presentation**
- Handles how data is represented to the user (ASN.1)

**Layer 5: Session**
- Handles conversations (logins sessions, SSH, logical Start/End)
- Cookies could technically fall into this category

**Layer 4: Transport**
- Handles process addressing
	- Port numbers (16 bit for TCP/UDP)
	- Flow control (making sure the sender is overwhelming the receiver by sending packets faster than it can consume)
	- Error recovery

**Layer 3: Network**
- Deals with WAN
	- Handles IP4 and IP4 addresses (32 bit)
	- Deals with routers & gateways 

**Layer 2: Link**
- Deals with LAN (*the things you are immediately adjacent to*)
- Focusing on the Ethernet protocol
- Looking at MAC addresses 
	- 48 bit 
	- Gets you across the LAN
	- Focus on switches and hubs

**Layer 1: Physical**
- Not going to look at in detail
- Deals with volts, amps, pinout, cable length, frequency, modulation (all the real work hardware bits)


## Example Frame
- The diagram below shows how the layer frames can be organised
- Notice that not all layers are used as not all are needed (No need for Presentation(6) or Session(5) in this instance)
- It essentially acts as envelopes that get put into bigger and bigger envelopes 
- Each layer opens their envelope and passes it up the stack

*"The ethernet layer opens the ethernet envelope and passes the ethernet payload (all the IP4 info) to the IP4 layer and so on up the stack"* - Peter Norris 2022

![[osi-layer-envelope.png]]

