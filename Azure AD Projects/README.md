
# Azure Part 1

## Overview
This first project contains detailed documentation of creating a home lab environment on Microsoft Azure. The setup involved deploying two virtual machines (VMs) - a Windows 10 PC and a Windows Server 2019.

## Goals
- Create a virtual network environment on Azure.
- Deploy Windows 10 and Windows Server 2019 VMs.
- Configure Active Directory Domain Services on the Windows Server.
- Establish communication between VMs within the virtual network.
- Join the Windows 10 VM to the domain hosted on the Windows Server.

## Project Steps

### Azure Setup
1. Signed up for a 30-day free trial with Azure.
2. Created a resource group and deployed Windows 10 and Windows Server 2019 VMs.

### Active Directory Configuration
1. Installed Active Directory Domain Services role on the Windows Server VM.
2. Configured the domain name as domain.homelab.

### Troubleshooting
- Encountered login issues due to password length requirements (15 characters for the Windows Server VM).
- Explored Microsoft documentation and troubleshooting resources.
- Resolved login issue by adjusting the password length.

### Network Configuration
- Explored network settings and DNS configurations.
- Realized VMs were on different VNets, hindering communication.

### VNet Peering
- Explored options to establish communication between VMs.
- Opted to move Windows 10 VM to the Windows Server 2019 Vnet to enable communication between the two.
- partially deleted Windows 10 VM and recreated it within the same Vnet as Windows Server 2019.

### Joining VMs to Domain
1. Updated DNS settings to point to the domain controller.
2. Joined the Windows 10 VM to the domain (domain.homelab).
3. Verified successful domain join and user authentication.

## Conclusion
The project successfully established a home lab environment on Azure, enabling hands-on learning and experimentation with Windows Server, Active Directory, and network configurations.
