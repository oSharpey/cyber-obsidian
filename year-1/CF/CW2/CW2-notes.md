# CF Coursework 2

## Remote Enumeration

### Initial NMAP Scan of Whole Network
``` 
┌──(kali@kali-99-590)-[~]
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
- 10.1.26.30 has some interesting ports open so we will do a more detailed scan of that machine

### Nmap Scan of 10.1.26.30
- -sC to run default scripts 
- -sV to enumerate versions

``` 
┌──(kali@kali-99-590)-[~]
└─$ sudo nmap -sC -sV 10.1.26.30                                                                                                                                                                             130 ⨯
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-06 13:19 GMT
Nmap scan report for 10.1.26.30
Host is up (0.00077s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 ea0b9c1d0ffba6399a15c00e9747cb83 (RSA)
|   256 a4feb3d4863dde1b7c71fe833db89ace (ECDSA)
|_  256 c5198035f76360ad38ae75cc8fc41969 (ED25519)
80/tcp    open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
111/tcp   open  rpcbind 2-4 (RPC #100000)
| rpcinfo:
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      34230/tcp   mountd
|   100005  1,2,3      34373/udp6  mountd
|   100005  1,2,3      42366/udp   mountd
|   100005  1,2,3      46250/tcp6  mountd
|   100021  1,3,4      42944/tcp   nlockmgr
|   100021  1,3,4      44892/tcp6  nlockmgr
|   100021  1,3,4      54835/udp6  nlockmgr
|   100021  1,3,4      58101/udp   nlockmgr
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
2049/tcp  open  nfs_acl 2-3 (RPC #100227)
10000/tcp open  http    MiniServ 1.890 (Webmin httpd)
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).
MAC Address: FA:16:3E:59:F5:C8 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 36.86 seconds
```

- This shows some interesting things
- We have ssh running which could be brute forced if the password isn't strong enough
- Apache 2.4.18 which could possibly be exploited
- An nfs share we can mount to upload files to the server
- MiniServ 1.89 which can be exploited to get a shell

### Looking at the Apache Server
![[apache-default-page.png]]

- Nothing interesting on the index page so use feroxbuster to enumerate resources on the server

### Feroxbuster
- Search for html, php and txt files
```
┌──(kali㉿kali-99-590)-[~]
└─$ feroxbuster -u http://10.1.26.30 -w /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt -x html,php,txt                                                                                           1 ⨯

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.7.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.1.26.30
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/dirbuster/wordlists/directory-list-2.3-medium.txt
 👌  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405, 500]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.7.3
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 💲  Extensions            │ [html, php, txt]
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
200      GET      375l      968w    11321c http://10.1.26.30/
200      GET      375l      968w    11321c http://10.1.26.30/index.html
403      GET        9l       28w      275c http://10.1.26.30/.html
403      GET        9l       28w      275c http://10.1.26.30/.php
403      GET        9l       28w      275c http://10.1.26.30/server-status
[####################] - 1m    882184/882184  0s      found:5       errors:0
[####################] - 1m    882184/882184  7816/s  http://10.1.26.30/
```

- Nothing of interest was found - seems just like a default Apache server

## Initial Access

- From nmap we can see we have webmin 1.890 running on port 10000
- This version is vulnerable to a backdoor exploit that we can run using metasploit

```
Metasploit tip: Use the resource command to run
commands from a file
Metasploit Documentation: https://docs.metasploit.com/

msf6 > search webmin 1.89

Matching Modules
================

   #  Name                                Disclosure Date  Rank       Check  Description
   -  ----                                ---------------  ----       -----  -----------
   0  exploit/linux/http/webmin_backdoor  2019-08-10       excellent  Yes    Webmin password_change.cgi Backdoor


Interact with a module by name or index. For example info 0, use 0 or use exploit/linux/http/webmin_backdoor
```

- We can then setup the exploit with the options for the victim server


