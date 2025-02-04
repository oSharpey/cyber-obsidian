## Answer the following questions (to be presented in section Analysis of logs in your report):
1. **What was the file name of the executable uploaded by PoisonIvy, including the file extension?** 
	- Find machine doing vuln scan - gives you malicious ip - 40.80.148.42. Verify the IP is in fact malicious 
		- `index=botsv1 imreallynotbatman.com sourcetype=stream:http | stats count by src | sort - count`
		- `index=botsv1 imreallynotbatman.com src=40.80.148.42 sourcetype=suricata
	- Get IP of where the website is hosted (192.168.250.70)
		- `index=botsv1 src=40.80.148.42 sourcetype=stream:http | stats count by dest_ip | sort - count`
	- Look for executables being sent to the web server
		- `index=botsv1 sourcetype=stream:http dest="192.168.250.70" *.exe` 
	- Look at part_filename we see 3791.exe and agent.php
		- `index=botsv1 src=192.168.250.70 sourcetype=stream:http | table url`
	- This can also be confirmed with Suricata and the firewall
		- `index=botsv1 sourcetype=fgt_utm dest=192.168.250.70 .exe | stats values(filename)`
		- `index=botsv1 sourcetype=suricata (dest=imreallynotbatman.com OR dest="192.168.250.70") http.http_method=POST .exe | stats values(fileinfo.filename)`
2. **Can you locate two of the brute force password used?** 
	- Narrow down search dest to webserver (192.168.250.70)
	- Looking for post requests as trying to log in 
	- Look at http form data to search for usernames and passwords
		- `index=botsv1 dest_ip=192.168.250.70 sourcetype="stream:http" http_method=POST form_data="*username*passwd*" | table form_data`
	- Sort by src - most come from 23.22.63.114
		- `index=botsv1 dest_ip=192.168.250.70 sourcetype="stream:http" http_method=POST form_data="*username*passwd*" | stats count by src`
	- Narrow search to just that source and get a list of all passwords with regex
		- `index=botsv1 dest_ip=192.168.250.70 src=23.22.63.114 sourcetype="stream:http" http_method=POST | rex field=form_data "passwd=(?<bruteforce>\w+)" | search bruteforce=* | table bruteforce`
	- We get a list of 412 brute force passwords two of which *anthony* and *camaro*
3. **How many unique passwords were attempted in the brute force attempt?** 
	- Building off previous search, use dc() to get a unique count of all passwords
		- `index=botsv1 dest_ip=192.168.250.70 src=23.22.63.114 sourcetype="stream:http" http_method=POST | rex field=form_data "passwd=(?<bruteforce>\w+)" | search bruteforce=* | table bruteforce | stats dc(bruteforce)`
	- We get a unique count of *412*
4. **Workstation we8105desk was connected to a file server during a ransomware attack. Find and locate the IP address of the file server?** 
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
5. **Locate and report how many unique PDF files did the ransomware encrypt on the remote file server?** 
	- We need to first find the hostname of the file server with ip 192.168.250.20
		- `index=botsv1 sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" 192.168.250.20`
	- The dest_host and src_host have the most frequent value being *we9041srv* this is most likely our hostname
	- First look at sysmon to see if there are any PDFs mentioned - gives us no results so change to look at other windows source types, gives us 526 results
		- `index=botsv1 host="we9041srv" pdf (sourcetype="WinEventLog:Security" OR source="WinEventLog:System")`
	- As the malware was run from Bob Smiths machine we should filter for that 
		- `index=botsv1 host="we9041srv" pdf (sourcetype="WinEventLog:Security" OR source="WinEventLog:System") Source_Address="192.168.250.100"`
	- Use dc() to get the number of unique pdfs
		- `index=botsv1 host="we9041srv" pdf (sourcetype="WinEventLog:Security" OR source="WinEventLog:System") Source_Address="192.168.250.100" | stats dc(Relative_Target_Name)`
	- And we get *257* unique PDFs encryped
6. **Locate and report how many unique text files did the ransomware encrypt on the Bob Smith’s host?** 
	- Look at text files references in sysmon data
		- `index=botsv1 sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational host=we8105desk *.txt`
	- Look at the events - 2 unique events ID: 2 (File create time), ID 1 (process create)
	- File create time looks more interesting - from microsoft:
		- *The change file creation time event is registered when a file creation time is explicitly modified by a process. This event helps tracking the real creation time of a file. Attackers may change the file creation time of a backdoor to make it look like it was installed with the operating system. Note that many processes legitimately change the creation time of a file; it does not necessarily indicate malicious activity.*
	- Filter by Event code 2
	- Looking at the text files returned we need to filer again to only paths on the machines user directory
		- `index=botsv1 sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational host=we8105desk EventCode=2 TargetFilename="C:\\Users\\bob.smith.WAYNECORPINC\\*.txt"`
	- We can use the dc() function like we used before to get a count of all unique files
		- `index=botsv1 sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational host=we8105desk EventCode=2 TargetFilename="C:\\Users\\bob.smith.WAYNECORPINC\\*.txt" | stats dc(TargetFilename)`
	- This gives *406* text files most likely encrypted
7. **There was a VBScript found during the post mortem, which launches a temp file. Locate is the ParentProcessId of this initial launch and the name of the temp file the VBScript had executed?**
	- As we know a usb drive was used we can look for common external drive letters D:\ E:\ and F:\ and we can see the Miranda tate file is on D:\
		- `index=botsv1 sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational host="we8105desk" ("d:\\" OR "e:\\" OR "f:\\")`
	- We can then look at processes executed and their command lines in the D:\ drive
		- `index=botsv1 sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational host=we8105desk (CommandLine="*d:\\*Miranda*" OR ParentCommandLine="*d:\\*Miranda*") | table _time CommandLine ParentCommandLine | sort _time`
	- We can see Miranda_tate_unveiled.dotm execute 2 child processes - a VB script and an exe file
	- We can see the ParentProcessId of the VBScript and EXE as *3756*
	- The name of the exe executed is *splwow.exe*



### **1. Detection Rule: Malicious File Uploads**  
**MITRE ATT&CK Mapping**:  
- **Tactic**: Command and Control (TA0011)  
- **Technique**: Ingress Tool Transfer (T1105)  
- **ID**: T1105  

**Splunk SPL Detection Rule**:  
```spl
index=botsv1 
((sourcetype="suricata" dest_ip="192.168.250.70" http.http_method=POST fileinfo.filename=*.exe)
OR
(sourcetype=fgt_utm dest=192.168.250.70 eventtype="ftnt_fgt_virus" .exe))
| eval destination=coalesce(dest_ip,dest)
| eval filename=coalesce(fileinfo.filename, filename) 
| eventstats dc(sourcetype) as sourcetype_count by filename  
| where sourcetype_count >=2  
| table _time, sourcetype, filename, src, destination
```  
**Rationale**:  
- Monitors HTTP POST requests containing `.exe` files to the web server (`dest_ip=192.168.250.70`), which is indicative of adversaries uploading malicious tools.  
- Maps to **T1105** (Upload malicious files to victim infrastructure).  

---

### **2. Detection Rule: Brute Force Attacks**  
**MITRE ATT&CK Mapping**:  
- **Tactic**: Credential Access (TA0006)  
- **Technique**: Brute Force (T1110)  
- **ID**: T1110  

**Splunk SPL Detection Rule**:  
```spl
index=botsv1 sourcetype=stream:http http_method=POST form_data="*passwd*" dest_ip=192.168.250.70 
| rex field=form_data "passwd=(?<password>\w+)" 
| stats dc(password) as unique_passwords, values(password) as passwords_used by src 
| where unique_passwords > 5 
```  
**Rationale**:  
- Detects multiple unique passwords (`dc(password)`) used in HTTP POST requests to the web server, a hallmark of brute force attacks.  
- Threshold (`unique_passwords > 5`) reduces noise from legitimate login attempts.  

---

### **3. Detection Rule: Command and Control (C2) Behavior**  
**MITRE ATT&CK Mapping**:  
- **Tactic**: Command and Control (TA0011)  
- **Technique**: Non-Application Layer Protocol (T1095)  
- **ID**: T1095  

**Splunk SPL Detection Rule**:  
```spl
index=botsv1 sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational 
| stats count by dest_ip, Image 
| where count > 100 AND dest_ip IN ("192.168.250.20", "192.168.2.50") 
| table _time, host, dest_ip, Image  
```  
**Rationale**:  
- Identifies abnormal traffic to internal servers (e.g., `192.168.250.20`), which may indicate ransomware spreading laterally or C2 communication.  
- Focuses on Sysmon events to track process-to-IP connections.  

---

### **4. Detection Rule: Ransomware Activity**  
**MITRE ATT&CK Mapping**:  
- **Tactic**: Impact (TA0040)  
- **Technique**: Data Encrypted for Impact (T1486)  
- **ID**: T1486  

**Splunk SPL Detection Rule**:  
```spl
index=botsv1 (sourcetype=WinEventLog:Security OR sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational) 
(EventCode=2 OR EventCode=11) (TargetFilename=*.pdf OR TargetFilename=*.txt) 
| stats dc(TargetFilename) as encrypted_files by host 
| where encrypted_files > 100 
```  
**Rationale**:  
- Detects mass file encryption by monitoring Sysmon `EventCode=2` (FileCreateTime) and Windows Security logs for rapid file modifications.  
- Threshold (`encrypted_files > 100`) highlights ransomware behavior.  

---

### **5. Detection Rule: Scripts Launching Temp Files**  
**MITRE ATT&CK Mapping**:  
- **Tactic**: Execution (TA0002)  
- **Technique**: Command and Scripting Interpreter (T1059)  
- **ID**: T1059  

**Splunk SPL Detection Rule**:  
```spl
index=botsv1 sourcetype=XmlWinEventLog:Microsoft-Windows-Sysmon/Operational 
(CommandLine="*wscript*" OR CommandLine="*cscript*") (CommandLine="*temp*" OR CommandLine="*.vbs*") 
| stats count by ParentProcessId, ProcessId, CommandLine 
| table _time, host, ParentProcessId, ProcessId, CommandLine  
```  
**Rationale**:  
- Flags scripts (e.g., VBScript) executed from temporary directories (`temp`) or removable drives (`D:\`), which are common in ransomware delivery.  
- Tracks parent-child process relationships (e.g., `splwow.exe` spawned by `Miranda_Tate_unveiled.dotm`).  

---

### **Additional Recommendations**:  
1. **False Positive Reduction**:  
   - Combine rules with threat intelligence (e.g., blocklisted IPs, known malicious hashes).  
   - Use Splunk’s `lookup` command to cross-reference internal asset databases.  
2. **Automated Response**:  
   - Integrate with SOAR tools to quarantine hosts or block IPs upon detection.  
3. **MITRE ATT&CK Enrichment**:  
   - Use Splunk’s ATT&CK Framework App to visualize rule mappings.  

These rules align with the BOTS v1 dataset patterns and real-world adversarial TTPs.