# Active Directory with PowerShell: Creating OUs, GPOs, and Users
In this lab, I explored the capabilities of PowerShell in managing Active Directory Domain Services (AD DS). My objective was to create Organizational Units (OUs), establish departmental groups, and configure Group Policy Objects (GPOs) to enforce specific settings within the AD DS environment. 

By leveraging PowerShell, I aimed to streamline administrative tasks and ensure efficient management of AD DS resources. In the last step I also leveraged Powershell to create users in the environment.

## Step 1: Connecting to Active Directory:
First, I launched PowerShell with administrative privileges and imported the Active Directory module using the `Import-Module ActiveDirectory` command. 

Next, I connected to the AD DS domain by using the command `$cred = Get-Credentials`, then provided appropriate credentials using the `Connect-AzureAD` cmdlet.

![p1](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/ae1503db-bfda-4879-b0d7-098bea94f83d)

## Step 2: Creating Organizational Units (OUs):
With the connection established, I proceeded to create the necessary OUs within the AD DS structure. OUs provide a means of organizing and managing objects within Active Directory. Using PowerShell, I created Boston first using the Command `New-ADOrganizationalUnit -Name “Boston” -Path “DC=domain,DC=homelab”`.

I then checked for successful creation with the command `Get-ADOrganizationalUnit -Filter {Name -eq “Boston”}`. Finally I repeated the process to create OUs named "New York City," and "Baltimore".

![pic2](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/5fe47d03-e854-4542-9720-22ec4b2a0ce4)
![pic3ad3](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/a5bf9696-c28a-47a8-a919-fe3a05b9d8d9)

## Step 3: Establishing Departmental Sub-OUs:
In this step, I decided to create four sub-OUs under the City OUs to organize users based on their departments. The four departments are: Management, Sales, Marketing, and IT.

I started the tedious process of typing each command manually, but then realized I could create a simple script to do this for me. So, I activated `powershell_ise` and wrote out the script below to create them, then checked for confirmation.

![pic4](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/d8971c49-600e-4c74-8c26-1aecfdb5956e)
![pic5](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/973546d8-6985-437e-af2c-7d233905d98c)

## Step 4: Creating & linking Group Policy Objects (GPOs)
Group Policy Objects (GPOs) play a crucial role in enforcing specific configurations and settings across the Active Directory Domain Services (AD DS) environment. Normally this would be done using the GPMC(Group Policy Management Console) however I intend to use this same setup for a future AD DS environment.

So, I created and linked these with a PowerShell script, then I configured them using the GPMC. I implemented the following GPOs for each location within the organization:
- Boston OU: A policy was set to a minimum password length of 12.
- New York OU: A policy was implemented to automatically lock screens after 5 minutes of inactivity.
- Baltimore OU: A policy was configured to set the account lockout threshold to 5 failed login attempts..

Within the same script, I created and linked the following GPOs for specific departments within each location:
- Boston IT - Users password must meet complexity requirements.
- New York City Management - enabled RDP.
- Baltimore Sales - disable control panel access.

![ad3p6](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/3a8be55d-5c57-47a1-a410-03b1e0e9ded0)
![ad3p7](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/be114da3-09a2-433e-bb76-995847555af9)

## Step 6: Configuring GPO Settings:
With the GPOs linked to the OUs, I utilized the Group Policy Management Console (GPMC) to configure the settings and policies defined within each GPO.

Through the GPMC interface, I had granular control over various configurations, allowing me to tailor the settings to meet specific organizational requirements. Pictures of configuring some of the GPO settings are below.

![a3p8](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/7efefe51-8f61-4b35-b5c5-a16c17f7b5db)
![a3p9](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/1b3a0ae0-30d2-4ca2-9ec7-8de512b27101)
![a3p10](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/2837676b-72a7-4ce3-ba18-d590e17a4de8)

## Step 7: Adding Users to Groups:
Using a modified PowerShell script I got from [here](link), I added 4 users for each management department, and 25 users for all other departments, for a total of 237 users within the organization.

![ad3p11](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/f8d9ab4d-df46-497c-8068-d1aefd9f1913)
![ad3p12](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/9a6d56b8-cf28-4638-929d-ed92c5f6ab6a)
![ad3p13](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/7e969841-e395-436c-816b-8f06bbf5b94d)

## Conclusion:
In conclusion, this lab demonstrated the effectiveness of PowerShell in managing Active Directory resources. By following the step-by-step instructions outlined above, I efficiently created OUs, established departmental groups, configured GPOs, enforced specific settings, and created users within the AD DS environment.

This lab also allowed me to see the limitations with PowerShell in configuring GPO settings and that using a combination of both PowerShell and the GPMC can maximize AD DS management.
