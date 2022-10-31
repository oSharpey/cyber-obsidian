# Data Representation 
- 2 types of integer 
	- Unsigned 
	- Signed - used for storing negative numbers, uses two's compliment 


### Endian
- **Little-Endian** - Where the lowest memory address stores the least significant byte & the largest address stores the most significant byte
- **Big-Endian** - The exact opposite of little-endian
- x86_64 Uses little-endian for example, whereas network traffic uses big endian 

***Storing the Value 0x12345678 in big endian***
![[beaddressing.png]]

***Storing the Value 0x12345678 in little endian***
![[leaddressing.png]]


### Character Storage 
- In low level systems, characters are represented in a numeric way (hex bytes)
- Characters are mapped to their numeric equivalent using character sets, eg. ASCII
- An example ASCII Table is shown below from https://en.wikipedia.org/wiki/ASCII

![[ascii-table.png]]


### Widths and their names
- **Byte** - 8 Bits (smallest addressable space)
- **Word** - 16 bits
- **Doubleword** - 32 bits
- **Quadword** - 64 bits 

![[widths-names.png]]


### Bit Numbering 
- MSB (Most Significant Bit ) - If this is changed it has a huge effect on the overall value of the number 
- LSB (Least Significant Bit) - Does not have a large effect if changed 

| MSB |   |   |     |   |   |   | LSB |
|:---:|---|---|-----|---|---|---|:---:|
|  0  | 1 | 1 |  0  | 1 | 0 | 0 |  1  |
|  7  |   |   | BIT |   |   |   |  0  |
