
# AWS Active Directory Project - Part 2

# Overview
This 2nd project in this series contains the steps involved in configuring Active Directory Domain Services (AD DS) on an AWS Windows Server 2019 instance. 
This guide includes creating a new AD forest, promoting a server to a domain controller, and joining both a Linux (Kali Linux) and a Windows 10 VM to the domain over the VPN set up in the previous project.

# Project steps
## Installation and Configuration of AD DS Role

### Install AD DS Role 
- Access Server Manager via RDP on the AWS Windows Server 2019 instance.
- Install Active Directory Domain Services role on the Windows Server VM.
  
### Promote the Server to Domain Controller
- Launch the AD DS Configuration Wizard from Server Manager after installing the AD DS role.
- Create a New AD Forest and specify the root domain name (`AWS.homelab`) and restart on completion.
  
### Verify AD DS Installation
- Validate the AD DS Role by logging back into the server post-restart and accessing 'Active Directory Users and Computers' from the tools tab in Server Manager.

## Joining Clients to the Domain

### Join Linux Client (Kali Linux)
- Install Required Packages for domain joining (e.g., using the pbis-open package).
- Run the Installation and setup scripts required.
- Join the domain and verify.

### Join Windows 10 VM to AD Domain
- Open System Properties and access the Computer Name tab to change settings.
-  Join the Domain by selecting Domain under 'Member of' and enter the credentials of an administrator account.
- Restart and Verify Domain Membership by attempting to log in with a domain account.

## Conclusion
This guide outlines the steps for setting up AD DS on AWS Windows Server 2019 and joining both a Linux and a Windows client to the domain. Ensure all devices' time and date settings are synchronized with the domain controller to avoid authentication issues.
