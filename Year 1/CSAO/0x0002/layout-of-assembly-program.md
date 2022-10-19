
# How an assembly program is laid out

``` nasm
; Start of the program

; Any assembly program has this structure

section .data

	; All intialised data goes here
	; 

section .bss

	; All unintialised data goes here
	

section .text
	global _start
		_start:
			; Program instructions
			; More instructions
			; And some more

; End of program
```


**[[processes-in-memory|Notice that .data .bss and .text are also in the structure of a process in memory]]**




