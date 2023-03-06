# CF Coursework 2

## Remote Enumeration

**Inital NMAP Scan of Whole Network**
``` bash
┌──(kali㉿kali-99-590)-[~]
└─$ sudo nmap 10.1.26.0/24
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-06 13:08 GMT
Nmap scan report for 10.1.26.1
Host is up (0.0019s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
MAC Address: FA:16:3E:54:7D:F0 (Unknown)

Nmap scan report for 10.1.26.2
Host is up (0.0020s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
53/tcp open  domain
MAC Address: FA:16:3E:B6:7A:98 (Unknown)

Nmap scan report for 10.1.26.3
Host is up (0.0020s latency).
All 1000 scanned ports on 10.1.26.3 are in ignored states.
Not shown: 1000 closed tcp ports (reset)
MAC Address: FA:16:3E:DB:FA:60 (Unknown)

Nmap scan report for 10.1.26.4
Host is up (0.0019s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
53/tcp open  domain
MAC Address: FA:16:3E:90:8D:AD (Unknown)

Nmap scan report for 10.1.26.5
Host is up (0.0019s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
53/tcp open  domain
MAC Address: FA:16:3E:77:33:1D (Unknown)

Nmap scan report for 10.1.26.30
Host is up (0.0019s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
111/tcp   open  rpcbind
2049/tcp  open  nfs
10000/tcp open  snet-sensor-mgmt
MAC Address: FA:16:3E:59:F5:C8 (Unknown)

Nmap scan report for kali-99-590 (10.1.26.20)
Host is up (0.0000060s latency).
Not shown: 998 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
3389/tcp open  ms-wbt-server

Nmap done: 256 IP addresses (7 hosts up) scanned in 2.54 seconds
```

- Nothing too interesting from 10.1.26.1 - 10.1.26.5
- 10.