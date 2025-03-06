# CSIM CW2 notes
## Hash verification
- Use FTK imager to verify the image

## Initial analysis
- Spiderman photos have been found
- Email correspondence about 'drops' for red and blue things
- Emails about hiding images -> mention SM (spiderman)

## To-do
- [x] Categorise images 
- [x] Look at email accounts
- [x] Find out what drops are
- [x] Look at web downloads
- [x] Prove he is interested in the images
- [ ] Create timeline
	- [ ] OS installed:  Thu Jun 20 12:54:09 2024
	- [ ] Accounts created: 2024-06-20 12:53:20
	- [ ] First images downloaded
	- [ ] First email to other suspects
	- [ ] First google searches
	- [ ] First images sent via wetransfer
	- [ ] Last logon date: 2024-09-06 03:47:29
	- [ ] Last Shutdown:  Friday, September 6, 2024 3:51:15 AM
	- [ ] Warrant executed


## Interview statements
### Not interested in images like that
- Google searches for spiderman images
- Downloads for spiderman images 
- Emails between suspect, Jenny and dinny
### There must have been malware
### Would have deleted them straight away
- Downloaded images still in both downloads folder and myphotos folder, not deleted
### Never been in contact with people who like those things
- Emails showing distribution of images via wetransfer
- Emails regarding going to meetings about SM (spiderman) images

## Analysis
### Images
- 116 Category A images
- 187 Category B images
- This is not an exhaustive list of images, enough were identified in order to prosecute 
- Found in these locations mainly 
	- `/img_Radovan-Marc-Andran.img/vol_vol2/$RECYCLE_BIN/S-1-5-21-2189243936-2770537914-435387343-1001`
	- `/img_Radovan-Marc-Andran.img/vol_vol2/Users/vboxuser/Downloads/`
	- `/img_Radovan-Marc-Andran.img/vol_vol2/Users/vboxuser/Desktop/myphhotos/`
- Arrested in September, images have been present in downloads since beginning of august, in myphotos since end of july
- Google searches for 
	- `spiderman`
	- `spiderman photos`
	- `spiderman photos new`
- Provenance of the images can be tracked to actions taken by the user
	- Downloads of zip files containing hundreds of spiderman photos each 
	- 

### Emails
- Found suspicious emails from Andran and Radovan
- Andran emails seems related to drugs, with drops and references to grams and dealing
- Radovan emailing various accounts about:
	- SM meet
	- numbers of photos
	- emailing wetransfer links for zip folders found on the system containing spiderman photos


### Malware
- There are 3 malicious files on the PC in LucusPics
	- Identified with cyber triage malware scanner
	- One is an XMRig miner (Spidey2)
	- Other is a cred stealer, StealC (Spidey3)
- I do not believe this provides a defence
	- First communication regarding spiderman photos was before these files were on the system
	- No evidence that these files were executed on the PC
	- Registry keys that the malware is known to set are not changed on the system
- Spidey1 and Spidey2 were accessed (from jumplist) 
- It is of my opinion that this malware was never executed on the machine


### Registry
- Decrypted Userasist
```
UEME_CTLCUACount:ctor	
{9E3995AB-1F9C-4F13-B827-48B24B6C7174}\TaskBar\Internet Explorer.lnk	
UEME_CTLSESSION	
C:\Users\Public\Desktop\Thunderbird.lnk	
{A77F5D77-2E2B-44C3-A6A2-ABA601054A51}\System Tools\Control Panel.lnk
{0139D44E-6AFE-49F2-8690-3DAFCAE6FFB8}\Thunderbird.lnk	
C:\Users\vboxuser\AppData\Local\Microsoft\Windows\Application Shortcuts\Microsoft.BingWeather_8wekyb3d8bbwe\App.lnk	
C:\Users\vboxuser\AppData\Local\Microsoft\Windows\Application Shortcuts\microsoft.windowscommunicationsapps_8wekyb3d8bbwe\Microsoft.WindowsLive.Mail.lnk	
{A77F5D77-2E2B-44C3-A6A2-ABA601054A51}\Internet Explorer.lnk	
{9E3995AB-1F9C-4F13-B827-48B24B6C7174}\TaskBar\Google Chrome.lnk	
{A77F5D77-2E2B-44C3-A6A2-ABA601054A51}\System Tools\Windows.Defender.lnk
C:\Users\Public\Desktop\Google Chrome.lnk
{A77F5D77-2E2B-44C3-A6A2-ABA601054A51}\Accessories\Notepad.lnk	
{0139D44E-6AFE-49F2-8690-3DAFCAE6FFB8}\Accessories\Snipping Tool.lnk	


UEME_CTLCUACount:ctor	
Microsoft.Windows.Shell.RunDialog
UEME_CTLSESSION	
Microsoft.Windows.ControlPanel	
Microsoft.Windows.Explorer	
Microsoft.InternetExplorer.Default	
C:\Users\vboxuser\AppData\Local\Microsoft\Windows\INetCache\IE\7CKFGW48\Thunderbird Setup 115.12.1.exe	
D78BF5DD33499EC2	
windows.immersivecontrolpanel_cw5n1h2txyewy!microsoft.windows.immersivecontrolpanel	
{1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\rundll32.exe	
Microsoft.BingWeather_8wekyb3d8bbwe!App	
microsoft.windowscommunicationsapps_8wekyb3d8bbwe!Microsoft.WindowsLive.Mail	
Microsoft.Windows.ControlPanel.Taskbar	
C:\Users\vboxuser\AppData\Local\Microsoft\Windows\INetCache\IE\7CKFGW48\ChromeSetup.exe	
{7C5A40EF-A0FB-4BFC-874A-C0F2E0B9FA8E}\Google\Temp\GUMEDD8.tmp\GoogleUpdate.exe	
{7C5A40EF-A0FB-4BFC-874A-C0F2E0B9FA8E}\Google\Update\GoogleUpdate.exe	
Chrome	
C:\Users\vboxuser\AppData\Local\Microsoft\Windows\INetCache\IE\1N1221UW\ChromeSetup.exe	
{7C5A40EF-A0FB-4BFC-874A-C0F2E0B9FA8E}\Google\Temp\GUMC86C.tmp\GoogleUpdate.exe
C:\Users\vboxuser\Downloads\DropboxInstaller.exe
{7C5A40EF-A0FB-4BFC-874A-C0F2E0B9FA8E}\Dropbox\Temp\GUMC933.tmp\DropboxUpdate.exe
{7C5A40EF-A0FB-4BFC-874A-C0F2E0B9FA8E}\Dropbox\Update\DropboxUpdate.exe	
Microsoft.Windows.Defender	
FileManager_cw5n1h2txyewy!Microsoft.Windows.PhotoManager	
{1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\OpenWith.exe	
{1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\notepad.exe	
{1AC14E77-02E7-4E5D-B744-2EB1AE5198B7}\SnippingTool.exe	
```


### System Information
**Current OS and Build**
- Windows 8.1 Pro, Version 6.3, Build 9600
**Install date**
- Thu Jun 20 12:54:09 2024 (UTC)
- Thu Jun 20 13:54:09 2024 (LOCAL)
**Who installed it?**
- Unsure, RegisteredOwner not set
**Name of computer**
- ANDREJA
**Who (appears to have) has an account on this system?**
- S-1-5-21-2189243936-2770537914-435387343-1001	C:\Users\vboxuser (Radovan shown in autopsy)
- S-1-5-21-2189243936-2770537914-435387343-1002	C:\Users\Andran
- S-1-5-21-2189243936-2770537914-435387343-1003	C:\Users\Marc
**Who was the last user to login?**
- vboxuser (Radovan)
**When did they last login**
- 2024-09-06 03:47:29
**When was Radovan Account created**
- 2024-06-20 12:53:20
**What was the timezone the computer was set to?**
- European Standard Time, CET, UTC+01:00
**When did each of them last shutdown the computer?**

**When was the last ever shutdown**
- Friday, September 6, 2024 3:51:15 AM
**What USB devices have been accessed on this system?**


**What programmes have been installed on this system? Is there evidence of programmes having been uninstalled (see my paper)?**

