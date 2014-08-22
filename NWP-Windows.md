Enabling Powershell on NWP Windows Servers
=========
---
Steps
---
---

1) Go to Start >> Open IIS Manager

2) In the top left click where it says "Connections" click on the hostname (ie, 'SAAM-DEV')

3) In the 'IIS' area, double-click on "Server Certificates"

4) On the upper right, click on Create Self-Signed Certificate. Follow the steps.

5) Close IIS

6) Go to Start and search for 'mmc' and open it

7) Click on File >> Add/Remove Snap-in...

8) Highlight 'Certificates', click on 'Add', select 'Computer account', click "Finish"

9) Click 'Ok'

10) On the left click the expand '+' icon next to 'Certificates (Local Computer)', expand 'Personal', click "Certificates"

11) Double-click on the self-signed certificate you made earlier

12) Click the "Details" tab, scroll down and select 'Thumbprint', leave this window open

13) Open cmd.exe as **Administrator**

14) Do: 
```
winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname="HOSTNAME";CertificateThumbprint="THUMBPRINT-WITH-NO-SPACES"}
```
15) Do:
```
winrm set winrm/config/service/auth @{Basic="true"}
```

16) Do:
```
netsh advfirewall firewall add rule Profile=Any name="Allow WinRM HTTPS" dir=in localport=5986 protocol=TCP action=allow
```
17) Lastly, ensure that the instance has "Allow from Ansible" security group attached
