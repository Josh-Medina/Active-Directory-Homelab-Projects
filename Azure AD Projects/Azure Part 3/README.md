# Azure Active Directory Project - Part 3

## Overview
In this final lab, I explore the capabilities of PowerShell in managing Active Directory Domain Services (AD DS). The objective is to create Organizational Units (OUs), establish departmental groups, configure Group Policy Objects (GPOs), and manage users within the AD DS environment.

## Project Steps

### Connecting to Active Directory
- Launch PowerShell with administrative privileges.
- Import the Active Directory module.
- Connect to the AD DS domain using provided credentials.

### Creating Organizational Units (OUs)
- Create OUs named "Boston," "New York City," and "Baltimore" within the AD DS structure.

### Establishing Departmental Sub-OUs
- Create sub-OUs under each city OU for departments like Management, Sales, Marketing, and IT.

### Creating & Linking Group Policy Objects (GPOs)
- Create GPOs for each location and specific departments within each location.
- Configure GPO settings such as password policies, screen lockouts, and access controls.

### Configuring GPO Settings
- Utilize the Group Policy Management Console (GPMC) to configure GPO settings and policies.

### Adding Users to Groups
- Use PowerShell scripts to add users to respective departments and management OUs.

## Conclusion
This is the conclusion of the Azure AD Projects series. This lab showcases how PowerShell can be utilized to manage specific components of Active Directory resources. Through PowerShell's capabilities, administrative tasks like creating and managing Organizational Units (OUs), Group Policy Objects (GPOs), and users can be automated. 

I will eventually put all the Powershell scripts I wrote for this lab into one single script and use it for another AD environment that I will be creating in AWS.
