# Getting Started on AWS: Instance Setup, VPN Configuration & Implementation

This guide is the first part of a three-part series. It details how to get started on AWS by creating and setting up EC2 instances and configuring a VPN to allow secure communication between your local VMs and an Active Directory (AD) Server on AWS.

## Step 1: Set Up of Windows Server 2019 on AWS

### Quick Start Guide for Launching a Windows Server 2019 Instance in AWS

- **Sign Up for AWS Free Tier:**
  - Sign up for AWS and select the free tier option. Access the AWS Management Console and navigate to the EC2 Dashboard to manage and launch instances.

![50createacc](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/dd906cfc-6531-4494-aa3c-3e0973e07649)

- **Launch Instance:**
  - Click on 'Launch Instance' to begin the instance setup wizard. Select the Microsoft Windows Server 2019 Base AMI from the list of available Amazon Machine Images (AMIs). Opt for a free instance type such as t2.micro.

![51instancelaunch](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/bd417120-364e-4133-be57-f5fcbdd79a74)
![52choose](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/c7b307e5-c436-4cba-b61f-73ee90e23861)
![55instance type](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/68f9742a-1491-4766-9864-c15aa22cf291)

- **Configure Instance Details:**
  - For the network, choose or create a Virtual Private Cloud (VPC) and select a subnet within your VPC that allows public internet access. Ensure that Auto-assign Public IP is enabled.

- **Configure Security Group:**
  - Create a new security group or select an existing one that permits the necessary inbound traffic, including at least RDP (TCP port 3389) for remote desktop access.

![56config storage](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/67610b46-2083-4654-91d1-e9d45d759411)


- **Review and Launch:**
  - Review all configurations to ensure everything is correctly set up. Select an existing key pair or create a new one, then click on the 'Launch' button. Once the instance is running, connect to it using an RDP client with the public IP address and the password associated with the account.

![56revlaunch](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/8ffcff89-3f8f-4fe6-9361-0fc483815d79)


## Creating an EC2 Instance for WireGuard

- **Log Into AWS Management Console:**
  - Access the EC2 Dashboard and initiate the process to launch a new instance.

- **Configure the Instance:**
  - Choose an Ubuntu Server with a 64-bit ARM processor. Opt for an instance type like t4g.nano, which is perfect for this purpose.
  - Keep the default settings for instance details, storage, and configure tags.
  - Set up a new security group allowing SSH access (port 22) and a specific port (51820/UDP) for WireGuard.

![16selectinstance](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/518c22b6-ae2e-4cb3-b261-901f4dc0efb8)
![17securityrule](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/7eda8b3f-22da-43bd-996b-26b9d77b3ae7)

- **Launch the Instance:**
  - Review and finalize the configurations, then launch the instance.

- **Assign a Static IP Address:**
  - Allocate and associate an Elastic IP with the Ubuntu instance to ensure the IP address remains constant across reboots.

![18allocateip](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/a9d8cadd-23f8-45a1-85d1-de622ee55980)
![19associateip](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/16c8cf8b-0906-43fd-8015-f7f7f21b43e6)
![20associate2](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/15ec7267-baf3-4310-bd82-d840bed183de)


- **Install WireGuard:**
  - SSH into the newly created EC2 instance. Install PiVPN with the curl command, which helps simplify the installation process and is compatible with Ubuntu.
  - Follow the prompts to configure specifics such as the VPN port (maintain the default 51820/UDP) and DNS settings, opting for custom settings and inputting the AD server's PRIVATE IP address.

![21sshlogon](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/0415ee96-2e79-4b6a-b585-98abc293ce78)
![22installpivpn](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/01c17da2-2c35-467e-9aa2-97126fad2995)

  - After going through the rest of the prompts, finish the installation and reboot the instance.
  - Once logged back in, use `pivpn add` to add a user, which creates a config file.
  - Navigate to the config file at `/home/ubuntu/configs/filename`, copy the config file, and save it to your local machine.

![23createclient](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/eb8ab86c-2d62-4713-b78c-dfd98b86a53d)


## Configuring Security Groups

- **Adjust Windows 2019 Server Settings:**
  - In the AWS console, adjust the Security group settings for the Windows 2019 Server to remove public access rules (e.g., RDP or SSH via public IP).
  - Ensure that only traffic over the VPN is permitted by specifying that inbound rules accept all traffic from the security group associated with the WireGuard instance (Ubuntu server).

![24editrule1](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/0607137f-76f4-4daa-8c45-1c49b353c9bf)

- **Adjust WireGuard Server Security Group:**
  - Adjust security group settings for Ubuntu server to allow all traffic from the security group of the Windows 2019 Server.

![25editrule2](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/027e5b40-1b4b-4229-b7de-f6c28839d389)

## Accessing Servers Using WireGuard

- **VPN Client Setup:**
  - Download and install the WireGuard client on your local Kali Linux VM using the commands `sudo apt update`, `sudo apt install wireguard`, and `sudo apt install wireguard-tools`. Import the VPN configuration obtained during the PiVPN setup.

![26installwireguard](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/987026ed-c71f-4db7-824c-cabbc371c2ea)

- **Establish VPN Connection:**
  - Move the config file to the appropriate directory (`/etc/wireguard`) so that WireGuard can use it upon activation using the `mv` command.
  - Use the command `wg-quick up filename` to activate the VPN connection, which securely encapsulates all traffic between your local machine and the AWS environment.

![27mvconffile](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/59a471bb-f340-4b00-9a22-cc5a3f3ab93f)
![28connecttovpn](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/2bb8091d-9875-4bb1-8e67-132116b1377e)

- **Enable DNS forwarding on AD Server (this ensures local machines can access the internet while on VPN):**
  - On the Windows Server 2019 instance, open the DNS Manager from the "Administrative Tools" in the Control Panel or by searching for "DNS" in the start menu.
  - Right-click on your DNS server in the left pane of the DNS Manager and select "Properties".
  - Go to the Forwarders tab and add the IP addresses of public DNS servers like 8.8.8.8 (Google DNS) or 1.1.1.1 (Cloudflare DNS) by clicking "Edit" or "New".
  - Click "OK" or "Apply" to save your settings.

![30dnsforwarding](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/0e9a5c42-cb75-4477-a57c-5dbcf1765293)
![31editdns2](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/6790fb64-f2fa-41be-8d09-3454f1ce5eaa)
![32editdns](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/7ebadcbe-4621-4fc5-87b0-951cf2153308)



- **Final Adjustments:**
  - Modify the VPN config file to replace the public DNS with the private IP of the server. Shut down and restart WireGuard with `wg-quick down filename` followed by `wg-quick up filename` to apply changes. Then ping `aws.homelab` to check resolution.

![33pingawshomelab](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/7cf3f5db-8580-4fa1-8793-2535636cd786)

## Installing WireGuard on Windows 10 VM

- **Configure New VPN Client:**
  - On the VPN server, run `pivpn add` and create a new config file for the next VM. Save this file to your local machine.

![39config2](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/97b57766-4cb2-47e7-81f3-64d2c2de356a)

  - On the Windows 10 VM, go to the official WireGuard website and download the Windows installer. Install WireGuard on your Windows 10 VM following the setup instructions.

![40downloadwireguard](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/e2335fde-0c14-4fee-93de-a88949d15d8c)
  
  - Import the configuration file you created, modifying the DNS settings to use the private IP of the server.

![41conffile](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/aed9dfef-f252-4970-b03d-72f52270e5af)

  - Connect to the VPN by activating the relevant tunnel in the WireGuard application and verify connectivity by pinging the AD server.

![42pingaws](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/a611afe5-3dbd-4cac-9a71-5a921c2002d7)


In the next step of the project, I will detail how to configure AD DS on the Windows 2019 server, and how to join the local VMs (Windows 10, Kali Linux) to the domain while using the VPN connection established in this tutorial.
