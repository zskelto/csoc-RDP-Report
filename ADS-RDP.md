# Goal
Detect potentially malicious, incoming activity on the active Remote Desktop Protocol (RDP) port to discover outside vulnerability scans and unauthorized connection attempts to RDP-enabled network devices. 

# Categorization
These attempts are categorized as [Lateral Movement / Remote Destop Protocol](https://attack.mitre.org/techniques/T1076/).

# Strategy Abstract
The strategy will function as follows:

* Monitor any activity over TCP and UDP port 3389.
* Scan for network devices with port 3389 open.
* Set up process monitoring for tscon.exe.
* Monitor services starting with cmd.exe /k or cmd.exe /c.
* Disable RDP unless it is essential.
* Alert when external machines attempting to RDP does not connect after multiple attempts on one or multiple machines. 
* Detect users who are using uncommon/foreign keyboard layouts 4:33

# Technical Context
[Remote Desktop Protocol](https://docs.microsoft.com/en-us/windows/desktop/termserv/remote-desktop-protocol) is a built-in Windows tool that allows a user to start an interactive session with a desktop GUI on a remote system and commonly runs on TCP/UDP port 3389. When in use, it is commonly used to work remotely or receive remote help from the IT department. 

Attackers can use RDP in conjunction with Credential Access techniques and Accessibility Features to connect to a remote system and gain expanded access. They can easily perform RDP session hijacking by stealing a legitimate user’s remote session or credentials. RDP session hijackers use techniques like spear phishing, keylogging, and brute-force attacks to manipulate user accounts, modify accessibility features and steal session keys.

An attacker can run the following command with proper credentials and the session key in order to hijack a session: ```c:\windows\system32\tscon.exe [session number to be stolen]```. 

As countermeasures to prevent compromising systems through RDP hacking, methods like continuous monitoring of the network on TCP/UDP port 3389 are implemented to check for any vulnerabilities that may arise. 

# Blind Spots and Assumptions
This strategy relies on the following assumptions:

* Process monitoring is active.
* Network traffic is being monitored.
* No legitimate user will be connecting to their work computer via remote desktop.
* Attacker will be scanning network for RDP enabled machines.
* Attacker will attempt bruteforce on RDP through TCP/UDP port 3389.
* RDP is disabled or login credentials are invalid.

A blind spot will occur if any of the assumptions are violated. For instance, the following would not trip the alert:

* Process monitoring has been deactivated.
* Malicious user does not perform a scan or bruteforce attack.
* RDP is enabled when attacker uses stolen credential to connect through RDP.


# False Positives
There are several instances where false positives will occur, including:

* Users working from home.
* Legitimate users accessing the RDP port with non-standard or non-English keyboards.

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
For detecting brute force, start an RDP connection to a windows machine with RDP enabled. Then enter invalid login credentials. Inspect the flow search between the IP of the remote computer and the user's computer on port 3389 of the remote computer. There will be an example of a failed connection, where no packets are sent between computers.

For network scanning, try connecting with RDP to a few machines with invalid credentials. Inspect the flow search of the IP attempting to connect. There will be several events of the IP connecting to other machines, but with no packets being delivered.

# Response
The following actions are recommended to be taken if the alert fires:

* Immediately block traffic over port 3389.
* Investigate the computer that RDP is running on.
  * Change the password of the user connected to RDP
  * Check for startup procedures starting RDP
* Run the following in Powershell:
  * -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server" -Name "fDenyTSConnections" –Value 1

# Additional Resources
* [General Detection Information](https://www.novetta.com/2015/03/advanced-methods-to-detect-advanced-cyber-attacks-rdp-keyboard-layout/)
* [TSCON](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/tscon)
* [Bomgar](https://www.bomgar.com/remote-support/features/remote-desktop-protocol-rdp)
* [Flow Detection of RDP](https://is.muni.cz/repo/1112279/Flow-based_detection_of_RDP_brute_force-attacks.pdf)
* [MITRE Details](https://attack.mitre.org/techniques/T1076/)
