# Azure Active Directory Project - Part 2

## Overview
In the second part of the Azure AD project, I performed the following tasks:

- Created a new user with Powershell
- Made changes to the password policies
- Granted user permission to RDP into the Windows 10 VM
- Mapped and created a shared folder for the new user
- Logged into the PC as the new user and created a sample text document to ensure proper configuration

## Steps
1. **Creating a New User with Powershell**
   - Imported the Active Directory module
   - Created a new user using the `new-aduser` command
   - Verified user creation with the `Get-ADUser` command

2. **Password Policy Adjustment**
   - Accessed Group Policy Management to edit password policies
   - Adjusted password policies to meet requirements

3. **Granting RDP Access**
   - Added the user to the Remote Desktop Users group in Computer Management

4. **Mapping and Creating Shared Folder**
   - Accessed Server Manager and created a new share folder
   - Modified folder permissions to allow read and write access for the user

5. **Connecting User to Shared Folder**
   - Configured a home folder for the user in Active Directory
   - Mapped the shared folder to a drive for the user

6. **Testing Configuration**
   - Logged into the PC as the new user via RDP
   - Confirmed access to the shared folder and created a test text file

## Conclusion
This concludes the second part of the Azure Active Directory project. In the next part, I will utilize Powershell to create more accounts and configure the server with DHCP.
