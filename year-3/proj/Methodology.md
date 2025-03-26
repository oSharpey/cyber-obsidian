# Methodology
## Design of adaptive system
[[Encryption Schemes]]
## Experimental Setup
Using the Chipwhisperer lite with CW303 ARM target board 
### Hardware and Software
- Laptop w/ Fedora 42
- STM32F chip on target programmed with MBEDTLS AES implementation for 128/192/256
- Chipwhisperer software version 6.0 with python api used to capture and analyse traces
- CW303 ARM target with STM32F arm cortex m4 chip
- Chipwhisperer lite capture board 

### Hardware Setup
Chipwhisperer lite single board solution comes as single unit already with the target connected to the capture board. All that needs to be done is to connect the 
capture board to the laptop via USB. The python API can be used to program the target board with different implementations