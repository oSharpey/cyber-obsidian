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

```
$ patchelf --replace-needed lib5ae9b7f.so ./lib/lib5ae9b7f.so --set-rpath lib ctf

$ ldd ctf
	linux-vdso.so.1 (0x00007fccdc8a6000)
	./lib/lib5ae9b7f.so (0x00007fccdc400000)
	libstdc++.so.6 => /lib64/libstdc++.so.6 (0x00007fccdc000000)
	libgcc_s.so.1 => /lib64/libgcc_s.so.1 (0x00007fccdc859000)
	libc.so.6 => /lib64/libc.so.6 (0x00007fccdc668000)
	libm.so.6 => /lib64/libm.so.6 (0x00007fccdc31c000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fccdc8a8000)
```

9) now the file can be run - so load it into IDA/Ghidra/Binja
10) rename vars see what it is doing - you see you need to pass in "show_me_the_flag"
11) realise doing dynamically would be easier and load into gdb
12) break at getenv to see what environment variable is needed and set it
13) break at the rc4_decrypt to see what is needed in the envrioment variable and set it
14) continue - get flag