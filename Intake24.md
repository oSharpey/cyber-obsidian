# Executive Protection Services 2

1) You get a exe file called challenge.exe
2) putting this into VT shows it to be a colbalt strike stager
3) Spin up a windows vm download x64dbg/windbg
4) Find where virtualalloc/heapalloc is called 
5) Break on that and you should see just after a binary being loaded into memory (starts with MZ)
6) once whole binary had been loaded dump it from memory
7) Edit the hex dump to ensure it starts with MZ
8) Use a tool like sentinalOne's beacon parser to get beacon config which contains flag.


Intake24{why_n0T_str1k3_b4cK}

# Executive Protection Services 1
Help someone has encrypted our clients filesytem with some custom written encryption - can you help get the client their files back?

Encryption:
1) take the sha with hash of the rootfs.gz 
2) store master key as a global and calculated with sha3 - that is used as a key and iv
3) hash together master key and sha hash of rootfs.gz store at rootfs.gz+256
4) hash part of master key with md5 and use it as enc_state1 and enc_state2
5) expand key from original IV 
6) encrypt enc_state1 and enc_state2 to use as xoring key
7) xor 16 bytes
8) add on a key value (0x4)
9) re_encrypt_key
	