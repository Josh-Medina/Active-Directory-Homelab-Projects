## Creating New User, Modifying Password Policies, and Setting Permissions

In this second part of the Azure AD project, I created a new user with Powershell, modified password policies, and granted RDP permissions to the user. I also mapped and created a shared folder on the server for the new user.
Finally I created a sample text document to ensure proper configuration.

### Creating a New User with Powershell

The first step was to use Powershell to create a new user. I began by running the command `Import-Module ActiveDirectory` to import the module for Active Directory. Then, I created a user with the command `new-aduser Wookie`. To verify its creation, I ran the command `Get-ADUser -Filter * | Select-Object Name`.

![Screenshot 2024-04-04 6 27 24 PM](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/d5ac9746-da74-4d0b-874c-cd267ee460b5)

### Modifying Password Policies

I attempted to set a password for Wookie but encountered an error stating that the password didn't fulfill the requirements. I addressed this issue by accessing the Group Policy Management option. From there, I right-clicked the Default Domain Policy and chose Edit. I then clicked Policies -> Windows Settings -> Security Settings - Account Policies -> Password Policy.

![Screenshot 2024-04-04 6 20 35 PM](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/18e56b55-f6c8-4234-9bab-f781624ee427)

Here is where I can edit the password policies to whatever I decide. After clicking apply, I refreshed everything and was able to add the password I wanted to use and activate Wookie. For demonstration purposes, I weakened the password requirements.
### Granting RDP Permissions

I proceeded to give Wookie the ability to RDP into the Windows 10 PC. I did this by going to Computer Management -> clicking on groups in the Local User and Groups dropdown -> then selecting Remote Desktop Users -> clicking Add -> typing the name until it populated automatically and clicking ok. I could then log in via RDP as the user Wookie.

![add wookie rdp user](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/13e4794d-f26f-4868-9e56-7338baf61ca9)

### Mapping a Folder and Creating a Shared Folder

Moving on, I mapped and created a shared folder specifically for Wookie. This involved accessing the Server Manager dashboard and selecting File and Storage Manager Services on the left -> Shares -> right clicking and selecting New Share. Here it gives options for how to set up the folder. I only modified the name to “wookies.folder” to keep it simple and created it. 

![Screenshot 2024-04-04 6 51 53 PM](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/4dad8b8f-708f-4b09-bd62-e6591c514197)

Next I navigated to the path of the folder and added a home folder to Wookie's folder.
 I right-click it and selected Properties -> Sharing -> Advanced Sharing -> share this folder. Then I clicked the Security tab in properties and selected Advanced -> Disable Inheritance -> convert inherited permissions into explicit permissions on this object -> clicked ok -> then select users and removed them. After that I clicked add -> select a principal -> enter Wookie -> click ok -> select modify on the list  -> then apply -> then ok.
 
 Then I went back to the properties and sharing tab -> click share -> modify Wookie's permissions to read & write -> click share -> click done. 
 
 Then I copied the directory path to the home folder and went back to users in Active Directory -> select Wookie -> select Profile -> select Connect -> select a drive(I chose W) -> paste directory path next to drive W chosen and hit ok. The pictures below show each step.

![Screenshot 2024-04-04 6 56 12 PM](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/3b2ce1b5-fb1f-4f40-877b-38f5dda9b042)
![Screenshot 2024-04-04 6 59 07 PM](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/322618cf-ebe2-40ff-ad7d-fb3941177ccd)
![Screenshot 2024-04-04 7 00 32 PM](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/2fc25dc6-aef8-49f5-b75c-3749d75a3a25)
![wookies home folder](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/f51f1607-917b-47bd-af8d-87a92feba411)

### Final Verification

Finally, I logged out of the domain power-user on the Windows 10 VM and RDP’d in as Wookie. Upon successful connection, I navigated to the W drive and confirmed access to the shared folder.

![logged in as Wookie](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/938db188-6527-4f0c-bc41-b932aa50f554)
![W drive loc](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/296bc94b-1102-443a-a958-5bd02e81900b)

Additionally, I demonstrated bidirectional synchronization by adding a test text file on the Windows VM, which promptly appeared on the server as shown below. 

![test dock wookie](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/0c1dd32c-d1d3-4203-9088-6524a9076c40)
![test doc for wookies file](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/b4c07871-f57b-40b0-9f47-920ed1f41a26)

This concludes the 2nd part of the Azure Active Directory project. In the 3rd and final part, I’ll utilize Powershell to create some organizational units, group policy objects, and users.
