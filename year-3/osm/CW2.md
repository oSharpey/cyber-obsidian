## Answer the following questions (to be presented in section Analysis of logs in your report):
1. **What was the file name of the executable uploaded by PoisonIvy, including the file extension?** 
	- Find machine doing vuln scan - gives you malicious ip - 40.80.148.42. Verify the IP is in fact malicious 
		- `index=botsv1 imreallynotbatman.com sourcetype=stream:http | stats count by src | sort - count`
		- `index=botsv1 imreallynotbatman.com src=40.80.148.42 sourcetype=suricata
	- Get IP of where the website is hosted (192.168.250.70)
		- `index=botsv1 src=40.80.148.42 sourcetype=stream:http | stats count by dest_ip | sort - count`
	- Look to see for traffic going out of the web server
		- `index=botsv1 src=192.168.250.70 sourcetype=stream:http` -> 9 events with our web server as the source which is weird
	- Look at the url to see *poisonivy-is-coming-for-you-batman.jpeg* being retrieved
		- `index=botsv1 src=192.168.250.70 sourcetype=stream:http | table url`
	- This can also be confirmed with Suricata and the firewall
		- `index=botsv1 sourcetype=fgt_utm "192.168.250.70" NOT dest="192.168.250.70" catdesc="Malicious Websites"`
		- `index=botsv1 src=192.168.250.70 sourcetype=suricata dest_ip=23.22.63.114 | stats values(url)`
1. **Can you locate two of the brute force password used?** 
	- Narrow down search dest to webserver (192.168.250.70)
	- Looking for post requests as trying to log in 
	- Look at http form data to search for usernames and passwords
		- `index=botsv1 dest_ip=192.168.250.70 sourcetype="stream:http" http_method=POST form_data="*username*passwd*" | table form_data`
	- Sort by src - most come from 23.22.63.114
		- `index=botsv1 dest_ip=192.168.250.70 sourcetype="stream:http" http_method=POST form_data="*username*passwd*" | stats count by src`
	- Narrow search to just that source and get a list of all passwords with regex
		- `index=botsv1 dest_ip=192.168.250.70 src=23.22.63.114 sourcetype="stream:http" http_method=POST | rex field=form_data "passwd=(?<bruteforce>\w+)" | search bruteforce=* | table bruteforce`
	- We get a list of 412 brute force passwords two of which *anthony* and *camaro*
2. **How many unique passwords were attempted in the brute force attempt?** 
	- Building off previous search, use dc() to get a unique count of all passwords
		- `index=botsv1 dest_ip=192.168.250.70 src=23.22.63.114 sourcetype="stream:http" http_method=POST | rex field=form_data "passwd=(?<bruteforce>\w+)" | search bruteforce=* | table bruteforce | stats dc(bruteforce)`
	- We get a unique count of *412*
3. **Workstation we8105desk was connected to a file server during a ransomware attack. Find and locate the IP address of the file server?** 
	- Find sourcetypes to look at - most come from sysmon so this seems like it would be useful and sysmon gives info on fileshares
		- `index=botsv1 we8105desk | stats count by sourcetype | sort - count`
	- Look for connections from we8105desk to other internal ip addresses
		- `index=botsv1 sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational host=we8105desk | stats count by src`
	- Setting the source in the search to out hostname (we8105desk.waynecorpinc.local) we can see traffic outgoing - all outgoing traffic is network connect
		- `index=botsv1 sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational host=we8105desk src=we8105desk.waynecorpinc.local`
	- We can see the most frequently hit ips from our pc with the search below - 2 IPs with most traffic 192.168.250.20, 192.168.2.50
		- `index=botsv1 sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational host=we8105desk src=we8105desk.waynecorpinc.local | stats count by dest_ip | sort - count`
	- Look for fileshares in the registry 
		- `index=botsv1 sourcetype=winregistry host=we8105desk fileshare`
	- Looking through the result we can see one IP address mentioned 192.168.250.20. This can be further verified with a regex lookup
		- `index=botsv1 sourcetype=winregistry host=we8105desk fileshare | rex field=key_path "(?<ip>(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)(\.(25[0-5]|2[0-4]\d|1\d\d|[1-9]?\d)){3})" | search ip=* | stats values(ip)`
1. **Locate and report how many unique PDF files did the ransomware encrypt on the remote file server?** 
2. **Locate and report how many unique text files did the ransomware encrypt on the Bob Smith’s host?** 
	- Look at text files references in sysmon data
		- `index=botsv1 sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational host=we8105desk *.txt`
	- Look at the events - 2 unique events ID: 2 (File create time), ID 1 (process create)
	- File create time looks more interesting - from microsoft:
		- 
1. **There was a VBScript found during the post mortem, which launches a temp file. Locate is the ParentProcessId of this initial launch and the name of the temp file the VBScript had executed?**