# AWS Active Directory Project Part 1

## Overview

This guide outlines setting up Windows and Ubuntu Server's on AWS, then configuring a VPN to facilitate secure communications between local VMs and the Windows Server in the AWS environment.

## Project Steps

### 1. **Windows Server 2019 Setup on AWS**
   - Sign up for AWS and access the Management Console.
   - Launch a Windows Server 2019 instance, configure network settings, security groups, and connect via RDP.

### 2. **EC2 Instance for WireGuard**
   - Create an Ubuntu Server instance configured for WireGuard use.
   - Assign a static IP and install WireGuard using PiVPN.
   - Set up security groups to manage traffic flow.

### 3. **Configuring and Accessing Servers Using WireGuard**
   - Install and configure the WireGuard client on a local VM.
   - Establish a VPN connection and ensure secure communication channels.

### 4. **DNS Configuration and Final Adjustments**
   - Enable DNS forwarding on the AD server to ensure proper domain name resolution.
   - Modify VPN settings to use AD server's private IP for DNS.

### 5. **WireGuard Installation on Windows 10**
   - Prepare a new VPN client configuration for Windows 10.
   - Install WireGuard, import configuration, and verify connectivity.

