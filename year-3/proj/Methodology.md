# Methodology
## Design of adaptive system
### AES Cipher
### https://sbrc2016.ufba.br/downloads/SessoesTecnicas/151977.pdf
Scheme 2 - Encryption key size:
- Level 0: Unencrypted;
- Level 1: 128-bit key;
- Level 2: 192-bit key;
- Level 3: 256-bit key

### Different Algos
### BASED ON https://lup.lub.lu.se/luur/download?func=downloadFile&recordOId=8936871&fileOId=8936873
| **Protection level** | **Cryptographic Algorithm**      |
| -------------------- | -------------------------------- |
| HIGH                 | AES256                           |
| MEDIUM               | AES128 (or something else maybe) |
| LOW                  | SPECK128                         |
| NONE                 | None                             |

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
capture board to the laptop via USB. The python API can be used to program the target board with different implementations. 
### Target Software Setup
Target software can be complied and programmed with the python API - MBEDTLS is used as the AES implementation with the simpleserial protocol used to trigger encryption and set the key. MBEDTLS is optimised with t-tables
### Capturing Traces
Use the python API to send various plaintexts and capture the traces from that, number of traces can vary from 50 to 10000. For the type of attack 2000 will be more than enough and can be cut down after the fact for the sake of speed


***All the code is in jupyter notebooks 
## Attack
### CPA attack
Basic steps:
- Get all data, power traces and corresponding plaintexts sent
- Create a power leakage model based off hamming weights of intermediate values - XORing key with plaintext output from the SBOX. This loops through all traces
- Rank the correlation coefficients to determine what is the most likely key
Higher hamming weights in general linearly scale to higher power consumption, so a higher correlation between the HW and power consumption corresponds to a more likely key 

Correlation uses Pearsons Correlation Coefficient:
 $$ r=\frac{cov(X,Y)}{\sigma _{x}\sigma _{y}}$$$$cov(X,Y) = \sum_{n=1}^{N}(Y_{n}-\bar{Y})(X_{n}-\bar{X})$$ $$\sigma _{X}=\sqrt{\sum_{n=1}^{N}(X_{n}-\bar{X})^{2}}$$
 where X is the set of captured power traces and Y is the guess at the internal state (the power model/guessed hamming weights), $cov(X,Y)$ is the covariance between the two datasets - identifying a potential linear relationship between the two and sigma is the standard deviation. In totality the PCC measures the linear correlation between two sets of data. 

### Ranking successful attacks
Use:
- measurements to disclosure (average minimum number of traces to get full key )
- Success rate - different papers calculate this different ways need to find one that works well





