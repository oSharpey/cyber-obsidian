# Lab 8 Notes

**start packet capture on gwW**

```sh
tcpdump -v -i eth0 -w /hostlab/gwW-eth0-01.pcap
```

- Setup screens on m1 and m2
- use `ss` command to see network connection stats
	- -n: dont resolve hostnames
	- -a: show both listening and non listening sockets
	- -t: display tcp sockets
	- -4: use IP4

```sh

watch -n1 ss -ant4

```

- Use watch command to update ouput (default output 2s)

- set up nc listener on port 80

- -n: dont resolve names

- -l: Listen for a connection

- -v: Verbose output

- -p: Specify source port

```sh

nc -nvlp 80

```

- generate TCP traffic from m2 to m1

```sh

nc 172.28.97.41 80 # from machine m2

```

- Quit out from m2 then restart listener on m1

- Go to watch screen

```sh

# Watch scrren on m1

# After sending hello from m2 to m1 and visa versa

State Recv-Q Send-Q Local Address:Port Peer Address:Port Process

LISTEN 0 10 0.0.0.0:3333 0.0.0.0:*

LISTEN 0 10 0.0.0.0:4444 0.0.0.0:*

LISTEN 0 10 0.0.0.0:5555 0.0.0.0:*

LISTEN 0 1 0.0.0.0:80 0.0.0.0:*

LISTEN 0 10 0.0.0.0:2222 0.0.0.0:*

ESTAB 0 0 172.28.97.41:80 172.28.97.42:45294

```

```sh

# Watch screen on m2

State Recv-Q Send-Q Local Address:Port Peer Address:Port Process

ESTAB 0 0 172.28.97.42:45294 172.28.97.41:80

```

- Set up another listener on m1

- Try to connect on port 3306 (does not connect as m1 isnt listening on 3306)

- Send content of /etc/services to m1 (-q5 stops the connections after 5 seconds)

```sh

cat /etc/services | nc 172.28.97.41 80 -q5

```

- create another tcp connection between m6 and m2

- make m6 a listener on port 123

- make m2 the client on port 5555

```sh

# Make m6 a listener

nc -nvlp 123

# Make m2 a client on 5555

nc 172.21.62.106 123 -p 5555

```

- Stop TCP dump on gwW and look at pcap

- View one TCP stream by going

- Highlight TCP packet

- Analyse in menu bar

- Follow in drop down menu

- Follow TCP stream

- To switch between relative and absolute sequence numbers

- Right click the packet

- got to protocol preferences > TCP > relative numbers