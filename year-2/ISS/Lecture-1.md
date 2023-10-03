
# Cryptography

## What is Cryptography
- Securing information by transforming it into an unreadable format

## Fundamental Concepts
- **Confidentiality**
	- Keeping info secure from unauthed users
- **Integrity**
	- Data remaining unaltered 
- **Non-Repudiation**
- **Trust**
	- Confidence in a system
- **Secrets and Keys**
- **Identity**
	- Need to verify who we are communicating with (certs)
- **Certificates**
- **Randomness and Entropy**
	- Using these to not be predicted

## Why we need cryptography
### Challenges in communication
- Eavesdropping
- Message Alteration
- Impersonation
- Man-in-the-Middle (MitM) Attacks
- Replay attacks
- Password attacks
- Etc…

### Example 1 - Eavesdropping
- Unauthorised parties intercepting messages
- Compromises confidentiality

### Example 2 - Message Alteration
- Tampering with data during transmission
- Compromises Integrity

### Example 3 - Impersonation
- Pretending to be someone else to gain trust
- Compromises authenticity 

## CIA Triad
- **Confidentiality**
	- Ensuring data remains confidential and only accessible to authorised parties
- **Integrity**
	- Maintaining the accuracy and trustworthiness of data, protecting from unauthorised alterations
- **Availability**
	- Ensuring that data and resources are available and accessible when needed


## Cipher Types
- **Substitution**
	- The position of the characters remain the the same but the characters are replaced by the other characters
	- Eg Caesar Cipher
- **Transposition**
	- The characters remain the same but the position changes
	- Eg Rail Fence Cipher

## Frequency Analysis
- Utilises language models and textual patterns
- Able to decipher classical ciphers manually
- Letter frequencies in language allows to deduce the correspondence between the plaintext and ciphertext
- **A systematic examination of:**
	- Letter Frequency Analysis
	- Pattern Analysis
	- Language Specific Information
	- Frequency Distribution
	- Identifying Common Letters




# Worksheet

## Caesar Cipher
1) Decrypt the following text using the Caesar cipher and Key 13
	- **Ciphertext**: *GUR BAYL GEHR JVFQBZ VF VA XABJVAT LBH XABJ ABGUVAT*
	- **Plaintext:** *THE ONLY TRUE WISDOM IS IN KNOWING YOU KNOW NOTHING*

2) Encrypt the following text using the Caesar cipher and Key 10:
	- **Plaintext:** *An unexamined life is not worth living. He who is not a good servant will not be a good master*
	- **Ciphertext:** *Kx exohkwsxon vspo sc xyd gybdr vsfsxq. Ro gry sc xyd k qyyn cobfkxd gsvv xyd lo k qyyn wkcdob*

3) By attempting simple character shifting (manually), determine the plain-text for the following cipher-texts encrypted using a basic Caesar Cipher






