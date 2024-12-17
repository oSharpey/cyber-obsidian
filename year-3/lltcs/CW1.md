
## Malicious macro in word doc
- All strings and variable names heavily obfuscated 
- ViperMonkey can parse the obfusacted strings and give IOCs
- Looks like it pulls down a packed malware file and ps1 file
- Changes registry keys to disable protected view and VBA security, along with stopping internet cache files being saved
- Runs the powershell file (Powershell -File ""C:\\Windows\\Temp\\12152021_17_59_52.ps1"")

## Pcap File
- Contains the downloaded exe file and ps1 file


