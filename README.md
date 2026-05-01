# Active Directory Home Lab

## Overview
Built a virtual lab using VirtualBox and Windows Server 2022 to simulate a basic Active Directory environment.

## Lab Setup
- Allocated 8GB RAM, 2 CPUs, 50GB disk
- Renamed server to `Server2022` (instead of default hostname)
- Adjusted advanced performance settings (tested “Best Performance” but reverted to default for better readability)

## Active Directory Configuration
- Installed Active Directory Domain Services (AD DS) via Server Manager
- Promoted server to Domain Controller
- Installation and promotion process took ~10 minutes
- Accessed Active Directory Users and Computers via Server Manager tools

## Domain Details
- Domain Name: `andervss.com`
- NetBIOS Name: `ANDERVSS`

## Changing Static IP on Server 2022 Lab
- Configured static IP to simplify lab environment and avoid DHCP changes
- Control Panel → Network and Sharing Center → Change adapter settings → IPv4 Properties
- IP address: `10.1.10.2`
- Subnet mask: `255.0.0.0`
- Default gateway: `10.1.10.1`
- Preferred DNS server: `10.1.10.2`
- Alternate DNS server: `10.1.10.1`
- Changed network adapter from NAT to Host-only Adapter in VirtualBox settings

## Using AD Users and Computers
- Searched entire directory when users were not immediately visible
- Enabled “Advanced Features” in View tab to locate user object paths
- Practiced searching for users (e.g. Guest account)
- Enabled Recycle Bin in Active Directory Administrative Center (creates “Deleted Objects” container)
- Created a “helpdesk” user by copying an existing account template for faster setup

## Basic CMD Commands
- `ipconfig` → Shows basic IP configuration
- `ipconfig /all` → Displays detailed network configuration (IP, DNS, DHCP, MAC)
- `net use` → Shows mapped network drives / shared resources
- `net user <username> /domain` → Displays domain user account details

## Command Notes
- DHCP Enabled = Yes → Dynamic IP address  
- DHCP Enabled = No → Static IP address  

## Windows 10 Lab
- Originally downloaded Windows 11 ISO
- Encountered black screen on first boot and attempted troubleshooting
- Switched to Windows 10 ISO due to compatibility issues with VirtualBox
- 
