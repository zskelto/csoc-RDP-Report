# Goal
Detect when Remote Desktop Protocol (RDP) is activated and in use on a Windows machine.

# Categorization
These attempts are categorized as [Lateral Movement / Remote Destop Protocol](https://attack.mitre.org/techniques/T1076/).

# Strategy Abstract
The strategy will function as follows:

* Monitor any activity over TCP and UDP port 3389.
* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\fDenyTSConnection should be set to 1.

# Technical Context
Technical Context provides detailed information and background needed for a responder to understand all components of the alert. This should appropriately link to any platform or tooling knowledge and should include information about the direct aspects of the alert. The goal of the Technical Context section is to provide a self-contained reference for a responder to make a judgement call on any potential alert, even if they do not have direct subject matter expertise on the ADS itself.

# Blind Spots and Assumptions
This strategy relies on the following assumptions:

* 

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
