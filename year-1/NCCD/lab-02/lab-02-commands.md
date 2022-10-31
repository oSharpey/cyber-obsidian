
## Lab 02
- From the router machine, ping both machine a and machine b with the command  `ping -c<number> <ip address>` 
- You will get a response from both machines as the router can see both LANX and LANY

- Now from machine a try to ping machine b using the same command, notice how the ping will fail. This is because there is no route added to get from a to b in the routing table
- Add a new route to let a route to b using the command `ip route add 10.0.0.0/27 via 172.28.97.41`

- Now ping the eth2 card in the router `ping -c4 10.0.0.1`, the ping will complete with no packet loss as machine a can now see the 10.0.0.0/27 subnet
- Now ping machine b using `ping -c5 10.0.0.2`, this ping will fail and you will not receive a packet back.
- The packet is successfully being sent from machine a to machine b, however as there is no route from machine b to a, machine b cannot see machine a

- Add a route from machine b to a using the command `ip route add 172.28.97.40/29 via 10.0.0.1` 
- You can now ping machine b from machine a and the ping will complete 

