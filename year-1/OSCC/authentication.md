# Authentication & Authorisation


## Authentication
### What is authentication?
- Determine the users identity before revealing sensitive info
- Could use: password, fingerprint, etc - The application later handles authentication
- Authentication identifies who the person or system is

### Uses in the OS
- By a server - uses the auth to know who's accessing the system
- By a client - To ensure the server is who it claims to be
- Use of a username and password
- Use of authentication certificates

### 3 Common factors
#### - Something you know
- This could be a password - its something only you know in your memory
- Base level of authentication
- Hashing - never stores plaintext password
- Issues with password: guessability (can be brute forced)

#### - Something you have
- Token: smart card, authentication code, ubikey
- Can be physical (ubikey) or digital/logical (one time code)
- Only as secure as the token itself - card/key can be lost

#### - Something you are
- Biometrics (fingerprint, iris scanner)
- Humans are unique
- Humans are also changeable - for example injuring your hand and losing a fingerprint


### Linux password process
- Read in username & password
- Lookup password in /etc/passwd file - if it isnt there, fail
- Otherwise, read UID, GID & shell info
- Lookup salt and hash from /etc/shadow, using the salt, hash the password and compare to the saved hash
- If it matches fork the process, exec shell, etc
- if fail, just fail - no info about why

### PAM
- Implements additional authentication in Linux
- Provides admins the ability to use multiple auth mechanisms into an existing system

## Authorisation
### What is authorisation
- Determines what a user can and cannot access
- For example linux file permissions

