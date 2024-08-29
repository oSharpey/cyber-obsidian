# Cobalt Mining

1) You get a exe file called challenge.exe
2) putting this into VT shows it to be a colbalt strike stager
3) Spin up a windows vm download x64dbg/windbg
4) Find where virtualalloc/heapalloc is called 
5) Break on that and you should see just after a binary being loaded into memory (starts with MZ)
6) once whole binary had been loaded dump it from memory
7) Edit the hex dump so it starts with MZ
8) Use a tool like sentinalOne's beacon parser to get beacon config which contains flag.