# Structure of an Assembly Program
**Main Parts**
- section .data
- section .bss
- section .text


## section .data 

### Datatypes
|Type|Length|Name|
|-|-|-|
|db|8 bits|Bytes|
|dw|16 bits|Word|
|dd|32 bits|Double Word|
|dq|64 bits|Quadword

### Initialising Variables and Constants in .data
**\<variable name\>**  **\<type>**  **\<value\>**
``` nasm
var_2: dw 0x7788 
```


**\<constant name\>**  **equ**  **\<value\>**

``` nasm
Capacity equ 100 
```


## section .bss

### Datatypes
|Type|Length|Name|
|-|-|-|
|resb|8 bits|Bytes|
|resw|16 bits|Word|
|resd|32 bits|Double Word|
|resq|64 bits|Quadword

### Initialising Variables and Constants in .bss
**\<variable name\>**  **\<type>**  **\<number\>**
``` nasm 
dArray resd 20 ;declares space for an array of 20 double words
```


## section .txt
- **Contains**
	- global
	- \_start
	- main and other function implementations 

