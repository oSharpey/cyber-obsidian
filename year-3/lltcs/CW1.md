
## DOCM File
- Contains malicious vba macro
- All strings and variable names heavily obfuscated 
- ViperMonkey can parse the obfusacted strings and give IOCs
- Looks like it pulls down a packed malware file and ps1 file
	- ms457.exe
	- 12152021_17_59_52.ps1
- Changes registry keys to disable protected view and VBA security, along with stopping internet cache files being saved
- Runs the powershell file (Powershell -File ""C:\\Windows\\Temp\\12152021_17_59_52.ps1"")

| Intermediate IOC                     | Value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Base64 String                        | c/blrFFGzJwUDWpdVBM1WO9xExkjgIB9euvb5lcOj3GFDrrKs8VGpDOyfPrp2W+tdIdIxKWVMM81wTkH2Z9unLFgc84o3/FnchOXx/GEr/bJwZW4yNYzXM0NxfmN6h+i+8lIdnPDw/y99zpDvDTK1Rvkc9O0NMCd5NOyBlLNYv2/oBJf/pk1i/ywXqz6SD+Ed4Zv+YiufwJJ2V412ghirofXNRg6HuC8oL/m7KO1Baapnn6VwCYqqtQfvBCgTy7H1aV9av5p//lHil1/JUO7blGt502yy9FBSiuqu5n+YN7rZMfPbhDYkU6EaaZ9xSRnmH5LmIf5wkQ4sImEfBeS966EKhnyxALH2FDISSEQQYpjdJb50BCoJosACu2xHyyfmXRrYBDbjXcQpk36v6HRXExJ/1Ub07jxnY2UXWxZm0+DmgaA81Hnpi02YexVWjdGN2iMJx6Wp3HK9xjPTuE8e7RRmnM= |
| IP Address                           | 10.0.2.4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| URL                                  | http\[:\]//10.0.2.4\[:\]8000/12152021_17_59_52.ps1                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| URL                                  | http\[:\]//10.0.2.4\[:\]8000/ms457.exe                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| SHA256 Hash of ms457.exe             | a5a57ea739a939b96f7d20b0c2e482f55144bd938312e41c41cad6373d642769                                                                                                                                                                                                                                                                                                                                                                                                                             |
| SHA256 Hash of 12152021_17_59_52.ps1 | 20e892d628581f8727fb8f378962ddbf638434918baae9609a570640bb3d47da                                                                                                                                                                                                                                                                                                                                                                                                                             |
### PCAP File
- Contains the downloaded exe file and ps1 file


## 12152021_17_59_52.ps1
- loads the ms457.exe into memory
- changes a byte at 0x3c to 0xd0
	- Changes the location of the PE header 
	- Anti-debugging technique to prevent just running binary
- Writes the new file to appdata
- Deletes original file
- runs new malware file

## ms457.exe
- TeslaCrypt dropper
- Seems to be packed 
- Dynamically resolves VirtualAlloc address so does not show in imports
- Unpacked woo


## Unpacked Malware
- No imports, most likely resolves all at runtime
- Stores address of VirtualAlloc on the stack to cal
- 


