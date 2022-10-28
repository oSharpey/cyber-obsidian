# The Cyber Lexicon
- Note: there are lots of different terms to describe perpetrators, need to be aware of this. Threat actor, hacker, criminal are all terms to describe the same thing.


## Risks, Threats and Vulnerabilities
**“A risk is the potential for a threat (a person or thing that is likely to cause damage) to exploit a vulnerability (a flaw, feature or user error) that may result in some form of negative impact.”**
- Example: the threat could be the software and the vulnerability would be a buffer overflow

### What are vulnerabilities
3 main types of vulnerabilities 
- **Flaws**
	- Programming/design error/shortcomings
- **Features**
	- Macros (like in MS Word)
	- JavaScript
- **User Error**
	- User causes vulnerability

### Zero Day Vulnerabilities
- A vulnerability that exists and has been disclosed but there is no known mitigation (no patch)
- It only becomes a zero day when someone knows about it
- Exposed with a zero day exploit
- Often sold on the dark web, sometimes for £100,000s

#### 7 Stages of Zero Day
1) The vulnerability is introduced by coders (intentionally or not)
2) A threat actor discovers the vulnerability and sells it on one of 3 markets
3) The software vendor becomes aware
4) The vendor discloses it publicly and the vulnerability gets a CVE number
5) Anti-Virus and security companies produce a malware signature for the vulnerability - completed when the AV company releases the signature
6) The software vendor releases the patch for the exploit
7) The software vendor feels like a significant number of endpoints has installed the patch

#### Markets
- Dark Web 
	- Selling zero day on dark web marketplaces 
- Extortion
	- A threat actor with the zero day informs the company they know about it and demand money from the company
- Threat hunters
	- Only legitimate market
	- Usually a company scheme

