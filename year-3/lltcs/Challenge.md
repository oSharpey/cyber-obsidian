1) tar -xf file_x.gz
2) you get 2 files ctf and 66......
3) try to run ctf - missing a library
4) ldd ctf - missing lib5ae9b7f.so

```
$ ldd ctf
	linux-vdso.so.1 (0x00007fac273d0000)
	lib5ae9b7f.so => not found
	libstdc++.so.6 => /lib64/libstdc++.so.6 (0x00007fac27000000)
	libgcc_s.so.1 => /lib64/libgcc_s.so.1 (0x00007fac27383000)
	libc.so.6 => /lib64/libc.so.6 (0x00007fac26e0f000)
	libm.so.6 => /lib64/libm.so.6 (0x00007fac2729f000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fac273d2000)
```

5) look at 67b8601 - sees its a bitmap?? 
6) Load into a hex editor - elf file with bitmap header on top of it
7) remove the bitmap header - this is the shared library
8) link this library into the file