## Answer the following questions (to be presented in section Analysis of logs in your report):
1. **What was the file name of the executable uploaded by PoisonIvy, including the file extension?** 
	- Find machine doing vuln scan - gives you malicious ip - 40.80.148.42. Verify the IP is in fact malicious 
		- `index=botsv1 imreallynotbatman.com sourcetype=stream:http | stats count by src | sort - count`
		- `index=botsv1 imreallynotbatman.com src=40.80.148.42 sourcetype=suricata
	- Get IP of where the website is hosted (192.168.250.70)
		- `index=botsv1 src=40.80.148.42 sourcetype=stream:http | stats count by dest_ip | sort - count`
1. **Can you locate two of the brute force password used?** 
2. **How many unique passwords were attempted in the brute force attempt?** 
3. **Workstation we8105desk was connected to a file server during a ransomware attack. Find and locate the IP address of the file server?** 
4. **Locate and report how many unique PDF files did the ransomware encrypt on the remote file server?** 
5. **Locate and report how many unique text files did the ransomware encrypt on the Bob Smith’s host?** 
6. **There was a VBScript found during the post mortem, which launches a temp file. Locate is the ParentProcessId of this initial launch and the name of the temp file the VBScript had executed?**