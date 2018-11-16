# Goal
Detect when Remote Desktop Protocol (RDP) is activated and in use on a Windows machine.

# Categorization
These attempts are categorized as [Lateral Movement / Remote Destop Protocol](https://attack.mitre.org/techniques/T1076/).

# Strategy Abstract
The strategy will function as follows:

* Monitor any activity over TCP and UDP port 3389.
* Set up process monitoring for tscon.exe.
* Monitor services starting with cmd.exe /k or cmd.exe /c.

# Technical Context
[Remote Desktop Protocol](https://docs.microsoft.com/en-us/windows/desktop/termserv/remote-desktop-protocol) is a built-in Windows tool that allows a user to start an interactive session with a desktop GUI on a remote system and commonly runs on TCP/UDP port 3389. When in use, it is commonly used to work remotely or receive remote help from the IT department. 

Attackers can use RDP to connect to a remote system and gain expanded access. Attackers can use RDP in conjunction with Credential Access techniques and Accessibility Features. Attackers can easily perform RDP session hijacking by stealing a legitimate user’s remote session or credentials. An attacker can run the following command with proper credentials and the session key in order to hijack a session: ```c:\windows\system32\tscon.exe [session number to be stolen]```

# Blind Spots and Assumptions
This strategy relies on the following assumptions:



A blind spot will occur if any of the assumptions are violated. For instance, the following would not trip the alert:

# False Positives
There are several instances where false positives will occur:

* Users working from home.

# Priority
The priority is set to high under the following conditions:

* Traffic is running over TCP or UDP port 3389.

The priority is set to medium under the following conditions:

* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\fDenyTSConnection is set to 0.

# Validation
RDP is enabled on the computer if the following command returns 0:

```
reg query "HKLM\System\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnection 
```

# Response
The following actions are recommended to be taken if the alert fires:

* Immediately block traffic over port 3389.
* Investigate the computer that RDP is running on.
  * Change the password of the user connected to RDP
  * Check for startup procedures starting RDP
* Run the following in Powershell:
  * -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server" -Name "fDenyTSConnections" –Value 1

# Additional Resources
