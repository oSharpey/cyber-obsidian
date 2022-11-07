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
var_2: dw 0x7788

**\<constant name\>**  **equ**  **\<value\>**
Capacity equ 100 
