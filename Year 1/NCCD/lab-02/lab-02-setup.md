
## Lab 02 Intro
For this lab we are going to create three machines `a`, `b`, and `r`, launch them using `lstart`, and finally analyse the contents of their `.startup` files. 
Further detail in [[lab-02-commands]]

## What will the network look like?
The image below shows the layout of the network produced by the contents in the config files
![[lab-02-diagram.png]]

## lab.dep
Used to tell Netkit to start each of the machines in parallel, the file does not have to contain anything 

## lab.conf
Used to set the local network cards each machine has and which LAN each of the network cards is on. 

### `lab.conf`

```python 
##############################################
LAB_DESCRIPTION="abr - two hosts, one router. Netkit 101."
LAB_VERSION="lab01-abr.2022-23.rc1"
LAB_AUTHOR="Peter Norris"
LAB_EMAIL="pn@warwick.ac.uk"
LAB_MACHINES=a,b,r
##############################################

a[0]=lanX

r[1]=lanX
r[2]=lanY

b[0]=lanY

###############################################
# corresponding network diagram
#
#   +------+      +-----------+       +------+       
#   |  a   |      |     r     |       |  b   |        
#   |      |      |           |       |      |        
#   | eth0 |      | eth1 eth2 |       | eth0 |        
#   +--+---+      +--+-----+--+       +--+---+       
#      |.42       .41|     |.1         .2|           
#      |             |     |     lanY    |           
#      |             |   --+-------------+--         
#      |     lanX    |       10.0.0.0/27             
#    --+-------------+--                             
#      172.28.97.40/29
#
# The IP addresses are assigned elsewhere (in the startup files)
```


## .startup Files
The .startup files are used to configure each of the machines with their MAC address, IP address and routing information

### `a.startup`

``` python 
# eth0 exists because there is a a[0] entry in lab.conf
# first, we give it an easily recongnisable MAC address
# 02:xx:xx:xx:xx:xx means it is locally, not globally administered

ip link set dev eth0 address 02:a0:a0:a0:a0:a0

# IP4 private address of our choosing
# the /29 defines which other addresses we expect on the LAN
ip addr add 172.28.97.42/29 dev eth0

# the configured network card is now made active
ip link set up dev eth0

# routing information tells how to get to IP4 addresses not on our LAN
ip route add 10.0.0.0/27 via 172.28.97.41 dev eth0
```

### `b.startup`

```python
# eth0 exists because there is a b[0] entry in lab.conf
ip link set dev eth0 address 02:b0:b0:b0:b0:b0
ip addr add 10.0.0.2/27 dev eth0
ip link set up dev eth0
ip route add default via 10.0.0.1 dev eth0
```

### `r.startup`

```python
# eth1 exists because there is a r[1] entry in lab.conf
ip link set dev eth1 address 02:81:81:81:81:81
ip addr add 172.28.97.41/29 dev eth1
ip link set up dev eth1

# eth2 exists because there is a r[2] entry in lab.conf
ip link set dev eth2 address 02:82:82:82:82:82
ip addr add 10.0.0.1/27 dev eth2
ip link set up dev eth2
```