## Answer the following questions (to be presented in section Analysis of logs in your report):
1. **What was the file name of the executable uploaded by PoisonIvy, including the file extension?** 
	- Find machine doing vuln scan - gives you malicious ip - 40.80.148.42. Verify the IP is in fact malicious 
		- `index=botsv1 imreallynotbatman.com sourcetype=stream:http | stats count by src | sort - count`
		- `index=botsv1 imreallynotbatman.com src=40.80.148.42 sourcetype=suricata
	- Get IP of where the website is hosted (192.168.250.70)
		- `index=botsv1 src=40.80.148.42 sourcetype=stream:http | stats count by dest_ip | sort - count`
1. **Can you locate two of the brute force password used?** 
	- Narrow down search dest to webserver (192.168.250.70)
	- Looking for post requests as trying to log in 
	- Look at http form data to search for usernames and passwords
		- `index=botsv1 dest_ip=192.168.250.70 sourcetype="stream:http" http_method=POST form_data="*username*passwd*" | table form_data`
	- Sort by src - most come from 23.22.63.114
		- `index=botsv1 dest_ip=192.168.250.70 sourcetype="stream:http" http_method=POST form_data="*username*passwd*" | stats count by src`
1. **How many unique passwords were attempted in the brute force attempt?** 
2. **Workstation we8105desk was connected to a file server during a ransomware attack. Find and locate the IP address of the file server?** 
3. **Locate and report how many unique PDF files did the ransomware encrypt on the remote file server?** 
4. **Locate and report how many unique text files did the ransomware encrypt on the Bob Smith’s host?** 
5. **There was a VBScript found during the post mortem, which launches a temp file. Locate is the ParentProcessId of this initial launch and the name of the temp file the VBScript had executed?**