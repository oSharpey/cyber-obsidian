# Encryption Systems

## HIGHT Cipher
### https://ieeexplore.ieee.org/document/8717426
HIGHT cipher, shown in Fig. 5, has the following features:
- is generalized feistel network,
- has block size of 64-bit and key size of 128-bit,
- includes initial round, 32 rounds and final round; where each round consists of simple operations: exclusive-or (XOR), bitwise rotations and mod 28 additions.
- key schedule has two phases: Whitening Keys (WKs) generation and SubKeys (SKs) generation.

two-round implementation, which is a fast implementation of HIGHT cipher with variable key (i.e. key generation). This implementation instantiates two rounds of the HIGHT algorithm in hardware for fast encryption.

one-round implementation, which is a configurable low-power implementation of HIGHT cipher. This implementation has single round instantiated with fixed key (i.e. no key scheduling). Fixed key implementation reduces power consumption compared with variable key [10]. It also varies the number of iterations (i.e. loops) applied on the single round to achieve different power and energy consumptions.
- control block activates the desired implementation with appropriate configuration based on power-level. Table I illustrates encryption configurations. The columns of the table are:
    - first column provides the configuration name. The configuration H2 is the two-round implementation, whereas H1_n is one-round implementation with n iterations.
    - second column shows number of rounds implemented in hardware. Higher number of rounds improves throughput and performance as shown in various studies including [9].
    - number of rounds used in the implementation. While HIGHT algorithm is designed for 32 round, we decided to reduce the number of rounds to meet power/energy constraints. While there is risk in reducing number of iterations, it is better than inhibiting encryption entirely or shutting down components. The number of iterations is decreased for only one round implementation.

![[Pasted image 20250318153627.png]]


## AES Cipher
### https://sbrc2016.ufba.br/downloads/SessoesTecnicas/151977.pdf
Scheme 2 - Encryption key size:
- Level 0: Unencrypted;
- Level 1: 128-bit key;
- Level 2: 192-bit key;
- Level 3: 256-bit key

## Different Algos
### https://lup.lub.lu.se/luur/download?func=downloadFile&recordOId=8936871&fileOId=8936873
| **Protection level** | **Cryptographic Algorithm** |
| -------------------- | --------------------------- |
| HIGH                 | AES256 with SHA3-256        |
| MEDIUM               | SPECK128 with BLAKE2s       |
| LOW                  | BLAKE2s keyed               |
| NONE                 | None                        |
### https://www.sciencedirect.com/science/article/pii/S2542660524001604

|Security level|Encryption algorithm|
|---|---|
|L1|LBlock, Skipjack|
|L2|PRINCE|
|L3|RECTANGLE|
|L4|Piccolo|
|L5|PRESENT, HIGHT, SIMON|
|L6|XTEA, AES|
