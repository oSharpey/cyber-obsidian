Bitwarden

Usage scenarios
- Sharing login information with trusted contacts
- Automatically auto-filling a passwords and card information into a website 
- Configuring TFA for the password manager to protect your account

Threat Scenarios
- A user falling for a phisihing attack and having the password manager autofill passwords and/or card information
- Having passwords synced over multiple devices increases the possibility of having a password compromised - more devices you can leave unlocked 
- A user choosing a weak easy to remember master password that is easily found in a wordlist


Assessing difficulty of use 
- Sharing login information with trusted contacts: De-motivators
	- system: Bitwardens sending system is complex to use, lots of complex options, not efficient 
	- external: User will be more inclined to use a less secure but more efficient method of sharing passwords ie sending them in plaintext
- Automatically autofilling passwords/card information
	- system: Autofilling sometimes does not always fill in the correct fields causing user to have to manually enter in the card/login information
	- system: Bitwarden hides autofill under a keyboard shortcut and is not shown to user as an option. User has to perform multiple clicks to get to the option of filling a password
	- External: build on point above, users have less technical ability, poorer eyesight or impared motor functions may find it difficult to navigate to the autofill option in the browser extension or press the keyboard shortcut
	- External: Competition offers ability to autofill password from the login form, ie firefox actively prompts user to autofill using built in password manager bypassing bitwarden
	- System: bitwarden often saves the wrong url for the website, especially if the website sign up url is different to login, casues autofil to not work and user has to manually search for login in password manager
- Configuring TFA for the password manager to protect your account
	- system: logging in with TFA is time consuming and takes more effort than not using it
	- external: lack of awareness for te benifits of using TFA
	- external: Cost, users may be deterred if they see it may come with added cost, buying a physical key like ubikey or paying for premium
	- System: Too much choice in options for TFA

Assessing Ease of use
-  A user falling for a phishing attack and having the password manager autofill passwords and/or card information
	- System: No warnings on use of the auto fill feature causing user to have blind trust in the security of the website and password manager
	- External: sophistication of phishing attacks may make it difficult for a lay user to identify the threat causing thier data to be compromised
- Having passwords synced over multiple devices increases the possibility of having a password compromised - more devices you can leave unlocked 
	- System: bitwarden does have an automatic logout feature, users stay logged into the password manager until the browser is closed, if you are actively using multiple devices its very easy to accidentally stay logged in leaving you exposed
	- System: Bitwarden does have an option to log out after a certain period of time. Not on be default however if on user may get frustrated at having to log in multiple times a day if the password manager is used regularly causing user to disable the feature
	- External: Convenience with having passwords synced over multiple devices
- A user choosing a weak easy to remember master password that is easily found in a wordlist
	- System: At signup bitwarden does not provide users with details on how to choose a secure password, it only states 12 characters minimum and has a sliding scale of weak to excellent, user is allowed to enter password helloworld1234! which is ranked as good
	- External: users choose path of least resistance, causes user to use a password that is easily rememberd by them and is not as complex as it should be, users tend to use the bare minimun password requirements presented. Weak passwords are more convenient 

Recommendations  
- Have the password manager session automatically log out after a specific amount of time to reduce the risk of password being exposed by leaving a device unlocked  
- Have a more prominent password health section to warn the user of weak master passwords and prompt the user to change it
- Educate users on phishing and provide phishing warnings to prevent autofilling on compromised websites