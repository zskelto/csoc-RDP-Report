# Goal
Detect when Remote Desktop Protocol (RDP) is activated and in use on a Windows machine.

# Categorization
These attempts are categorized as [Lateral Movement / Remote Destop Protocol](https://attack.mitre.org/techniques/T1076/).

# Strategy Abstract
The strategy will function as follows:

* Monitor any activity over TCP and UDP port 3389.
* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\fDenyTSConnection should be set to 1.

# Technical Context
[Remote Desktop Protocol](https://docs.microsoft.com/en-us/windows/desktop/termserv/remote-desktop-protocol) is a built-in Windows tool for controlling the GUI from a remote location.



# Blind Spots and Assumptions
This strategy relies on the following assumptions:

* No legitimate user will be connecting to their work computer via remote desktop.

A blind spot will occur if any of the assumptions are violated. For instance, the following would not trip the alert:

# False Positives
There are several instances where false positives will occur:

* Users working from home using RDP will cause traffic over 3389 and a modified fDenyTSConnection value.

# Priority
The priority is set to critical under the following conditions:

* Traffic is running over TCP or UDP port 3389.

The priority is set to high under the following conditions:

* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\fDenyTSConnection is set to 1.

# Validation


# Response


# Additional Resources
