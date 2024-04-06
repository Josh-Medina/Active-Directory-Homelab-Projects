# Setting Up Azure Virtual Machines and Active Directory Domain Services
Today I’m creating 2 virtual machines on Azure - a Windows 10 PC and a Windows Server 2019. After creating them, I’ll log in to the server and install the Active Directory Domain Services role. Then I'll put both machines on the same VNet so they can communicate with each other and join them on the same Domain in Active Directory. I could’ve configured them to be on the same VNet from the start, but I wanted to demonstrate the process of moving a VM to another VNet.


## Getting Started with Azure

Azure allows for a 30-day free trial and a $200 credit for signing up. After signing up it brought me to the main dashboard where I clicked on virtual machines and then clicked create Azure virtual machine. There are many options to choose from but I stuck with the simple & free options for both of my machines. During the process, it prompted me to create an Administrator account and password to be able to log in.

An important aspect is to have a password that satisfies the full requirements. Passwords must have 3 of the following: 1 lower case character, 1 upper case character, 1 number, and 1 special character, AND the value must be between 12 and 123 characters long. For some reason, 12 characters were fine and I was able to login to my Windows 10 VM with no problem, however the same wasn't true for the Server.

Despite using the same password for both it would not let me log in. I ended up troubleshooting for hours and double-checking all network and security configurations. I even changed the password multiple times and dug deep into Microsoft documentation on Azure.


<img src="https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/c60cba4f-d81b-4e7a-bb2e-27cdd1d55b7e.jpg" alt="serverloginerrer" style="width:50%;"><br>


I was about to delete the VM and create a new one until I finally figured out that I had to use 15 characters to be able to log in to the Server. I thought I was going crazy because I could not find any other issues, so that is a mistake I'll be sure never to make again. This [video](link_to_video) here is what helped me finally figure it out.

## Installing Active Directory Domain Services

Now that I was passed that issue, I can log in to the server and configure it for Active Directory Domain Services. I did this simply by going into the server manager, clicking on the manage tab on the top right, and choosing “Add roles and features”. I chose the installation type “role-based or feature-based installation”.

From here it brought me to server roles, there was a big list of roles to choose from. I chose “Active Directory Domain Services” and gave the name domain.homelab, then installed it. After installation, it logged me out and restarted the computer.

![install ADDS on Server](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/0dafb789-26dc-4811-a4ea-513f8b5da84c)
![ADD services install](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/a78b2d39-abf2-46a2-aba4-75e2968a9e5f)


## Joining VMs to the same Vnet

In this next part, I took the following steps before I realized that the VMs are on 2 different vnets and cannot communicate. The first thing I did was open the command prompt and attempt to ping the server to see if communication could be established. They would not ping each other.

I then went to my control panel -> view network status and tasks -> change adaptive settings -> double click on ethernet -> click properties ->click internet protocol version 4 (TCP/IPV4) -> properties -> chose “use the following DNS server address”, then I entered the private IPV4 address for the server(10.0.0.4). I also selected “use the following IP address” and entered IP address 10.0.0.5, subnet mask 255.255.255.0, and default gateway 10.0.0.1 for my Windows 10 machine.

![changing adapter settings](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/e0994739-9c2a-4b7e-8276-a4a52005ddbc)

 After pressing enter it kicked me out of the Windows 10 VM. This is because I changed the private IP address from 10.0.0.4 to 10.0.0.5.

In the Azure dashboard, I then navigated to network settings -> windows10220_z2 | IP configurations -> IP configurations. There I clicked on ipconfig and an option popped up to edit the private IP address.

![edit private IP on azure for pc](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/859111ea-2efe-44f4-bef5-26b5e56d5ede)

 I changed the private IP address to static and entered 10.0.0.5. This allowed me to log in again to the PC, however, the 2 VMs were still not communicating with each other. The reason for this was because they are on 2 different isolated virtual networks.

If I wanted them to communicate I was considering 2 options, 1: Switch one VM over to the other vnet by partially deleting it (probably the Windows 10). 2: peer them so they can communicate, a feature in Azure that allows for vnets to establish a network connection between them.

I chose option one and placed them on the same vnet. This solution included partially deleting the Windows 10 VM. I did this by following the instructions in this [video](link_to_video). I went to the VM I wanted to move, clicked delete, then selected delete the network interfaces and unselected the disk. Then I hit delete. This deleted all the resources associated with the VM (NIC & vnet) except for the disk image itself.

![Screenshot 2024-03-29 8 10 14 PM](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/7924b2a8-1049-4be9-9c3e-5887ff999e6f)


Afterwards, I went to the resource group where the VM is located and selected osdisk -> then selected create VM. From there I filled out everything as before EXCEPT - I made sure to select the vnet I wanted to move the Windows 10 vm to(server2019-vnet/default). I then clicked create and sure enough they are now on the same vnet. I activated both the server and Windows 10 VM and pinged both just to be sure.

I also made sure to adjust the firewall settings on the Windows 10 VM to allow inbound and outbound ICMP echo requests over IPV4 private. I did this by going into the advanced settings in Windows Defender. As you can see in the images below I can now ping both the Windows 10 VM ( 10.0.0.6) & the Windows 2019 server ( 10.0.0.4).

![outbound firewall settings pc](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/a8bb32ab-80f8-4f68-93cc-3e96f9e1f20f)
![pinging server from pc](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/1eed123c-911f-42b8-93fc-992c7dd4085a)
![ping pc from server firewall settings](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/8dc982f7-4643-49f7-a1cd-0b98adb6b7da)


## Joining VMs to the Domain

Finally, I was able to join the Windows 10 VM to the domain of the 2019 server. After going into the pc I followed the same prior steps: control panel -> view network status and tasks -> change adaptive settings -> double click on ethernet -> click properties ->click internet protocol version 4 (TCP/IPV4) -> properties -> choose “use the following DNS server address”, then entered the private IPV4 address for the server (10.0.0.4).

![adapter change 2](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/ecc19a12-e81b-437c-a0c5-c7ad3fa71029)

 I also had to use the dashboard in Azure to go into the NIC for the Windows VM and go to overview -> networking -> click on network interface desktop -> settings on the left pane -> DNS servers -> select custom -> and add the private IP of the server 10.0.0.4. I then restarted the VM and it took a while but I was able to log back in and ping domain.homelab.
 
 ![Screenshot 2024-03-29 8 35 32 PM](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/c1967ea5-b1df-4266-8da5-9d2c2a780e8b)
![pinging domain homelab](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/0451bc6f-1dbc-4b7c-8e6a-410a4e6e83c5)

The next step is to join the Windows machine to the domain. I went to File Explorer -> right click this PC -> properties -> rename this PC (advanced) -> change -> select member of domain -> type domain.homelab -> click ok - >log in using server credentials -> computer will restart after login.

![Adding Pc to domain](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/6a4fd320-2eb5-49ca-9ca4-691a88a8eaaf)


After restarting I checked the server, under active directory users and computers -> computers -> and there it is as shown below. I can now log in to the PC using the credentials from my server with the domain name Master using “domain.homelab\Master”, this is the power user who has access to the active directory from the Windows 10 machine.

![computer on domain](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/ea325876-2032-4446-a27a-3f0cefd92273)
<img src="https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/83fb8e6b-2004-4340-adb4-b4a8e9f6b2c6" width="50%">
![Master domain in pc](https://github.com/Josh-Medina/Active-Directory-Homelab-Projects/assets/162754106/07509c96-b1eb-43e3-9143-50f1a571193f)



This concludes part 1 of the Azure Active Directory project. In part 2 I’ll be creating a new user with Powershell, Giving that user permission to use RDP, then creating and mapping a folder.
