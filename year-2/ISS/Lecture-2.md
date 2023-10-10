# Hash Functions

- Hashing is based on a one-way mathematical function that is relatively easy to compute, but significantly harder to reverse
- It takes a variable-length block of data and produces a fixed length hash value, called the hash
- Every time the data is altered, the hash value also changes

## Features of Hash Function
- The input can be any length 
- The output has a fixed length 
- H(x) is relatively easy to compute for a given x 
- H(x) is one-way and not reversible 
- H(x) is collision-free, meaning that two different input values will result in different hash values
## Hashing
- Can be used to detect changes 
- Vulnerable to Man in the Middle
## Origin authentication
- A keyed-hash message authentication code (HMAC) is used
- HMAC uses an additional secret key as input to the hash function
## HMAC Hashing algorithm
- Uses any cryptographic algorithm that combines a cryptographic hash function with a secret key 
- Only the sender and the receiver know the secret key 
- Only parties who have access to that secret key can compute the digest of an HMAC function
- Indicates originator of the message

# Lab - Hash Functions

| Plaintext | MD5 | Sha1   | Sha256 |
| -------- | ------- |----------|-------- |
| password  | 5f4dcc3b5aa765d61d8327deb882cf99 | 468df36d08e878d73b90af1d33740db9e63b0245| |
| _blankfile_ | d41d8cd98f00b204e9800998ecf8427e   | | |
| alice   | 6384e2b2184bcbf58eccf10ca7a6563c    | | |


password : 5f4dcc3b5aa765d61d8327deb882cf99
\<blankfile\> : 
alice : 






