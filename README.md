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
- `ipconfig /flushdns` → Clears cached DNS records
- `net use` → Shows mapped network drives / shared resources
- `net user <username> /domain` → Displays domain user account details
- `ping <ip>` → Tests connectivity to another device on the network
- `ping <ip> -t` → Continuously pings a device to monitor connectivity (`Ctrl + C` to stop)

## Further CMD Commands / Group Policy
- `gpresult` → Displays Resultant Set of Policy (RSOP) information
- `gpresult /r` → Displays applied user and computer policies
- `gpresult /r > c:\result.txt` → Exports policy results to a text file
- `gpresult /h gpresult.html` → Generates HTML Group Policy report
- `gpupdate` → Refreshes and reapplies Group Policy settings
  
## Command Notes
- DHCP Enabled = Yes → Dynamic IP address  
- DHCP Enabled = No → Static IP address  

## Desktop1 Windows 10 Pro Lab
- Originally downloaded Windows 11 ISO
- Encountered black screen on first boot and attempted troubleshooting
- Switched to Windows 10 ISO due to compatibility issues with VirtualBox
- Allocated 8GB RAM, 2 CPUs, 50GB disk
- Enabled built-in Administrator account
- Removed standard user account for simplified lab setup
- Renamed computer to `Desktop1`
- Configured static IP:
  - IP address: `10.1.10.3` (must be different from server)
  - Subnet mask: `255.0.0.0`
  - Default gateway: `10.1.10.1`
  - Preferred DNS server: `10.1.10.2`
  - Alternate DNS server: `10.1.10.1`
- Changed network adapter from NAT to Host-only Adapter in VirtualBox settings
- Joined domain `andervss.com` using domain administrator credentials
- Successfully logged into the domain using the `helpdesk` account
- Practiced creating an organisational unit, creating a user account, and moving accounts into folders
- Explored Group Policy Management, including password policy, account lockout duration, account lockout threshold, and max password age

## Notes
- In most help desk roles, direct access to servers (e.g. Server Manager) is restricted
- Administration is typically performed using tools like RSAT from a client machine

## Optional RSAT Features
- Remote Server Administration Tools (RSAT) allow administrators to manage Windows Server roles remotely from a Windows 10/11 machine
- Common RSAT tools added:
  - Server Manager
  - Remote Desktop Services Tools
  - Active Directory Domain Services Tools
  - Active Directory Certificate Services Tools
  - Group Policy Management Tools
  - DNS Server Tools
  - DHCP Server Tools

## Desktop2 Windows 10 Pro Lab
- Created second client VM with similar configuration to Desktop1
- Renamed machine to `Desktop2`
- Configured static IP: `10.1.10.4`
- Joined domain `andervss.com`
- Successfully logged in using `helpdesk` account
- Tested account lockout using user account `patty`
- Practiced unlocking accounts and resetting passwords through Active Directory
- Removed `Desktop2` from the domain and successfully rejoined it

## Shared Folders and Security Groups
- Created shared folders `HR` and `Personal` through Server Manager
- Configured shared network paths:
  - `\\SERVER2022\HR`
  - `\\SERVER2022\Personal`
- Created security group `HR`
- Added shared folder network paths to group descriptions for easier reference
- Added user `patty` to relevant security groups

## NTFS Permissions and Access Control
- Disabled inheritance on shared folders to configure custom permissions
- Removed default users/groups from permissions
- Added `helpdesk` account and assigned Modify permissions
- Configured `HR` and `Personal` folder permissions with Read/Write access

## Desktop2 Share Access Testing
- Logged into `Desktop2` using user `patty`
- Connected to shared folders using:
  - `\\SERVER2022\HR`
- Created shortcut and pinned shared folder to Quick Access
- Mapped network drives:
  - `Z:` drive → HR share
  - `P:` drive → Personal share using:
    - `\\SERVER2022\Personal\%username%`
- Tested adding/removing additional user `test`
- Practiced modifying user access and permissions

## Remote Desktop and Remote Administration

### Remote Desktop Testing
- Enabled remote connections to `Desktop2`
- Tested Remote Desktop connection from `Desktop1`
- Verified functionality by creating and moving folders on user `patty` desktop remotely

### DNS Troubleshooting
- Used `ipconfig /flushdns` to clear cached DNS records

### Remote Registry Access
- Attempted remote connection to `Desktop2` using Registry Editor
- Encountered error relating to Remote Administration / Remote Registry service
- On `Desktop2`, enabled `RemoteRegistry` service:
  - Set Startup Type to `Automatic`
  - Started service successfully
- Successfully connected to `Desktop2` through Registry Editor after enabling service

### Administrative Shares and File Access
- Accessed hidden administrative share:
  - `\\Desktop2\c$`
- Tested remote file access by creating folders on user `patty` desktop
- Explored accessible shared folders and permissions

### Windows Remote Assistance
- Located Remote Assistance using `msra` / `msra.exe`
- Attempted remote assistance connection to `Desktop2`
- Encountered permissions issue on `patty` when adding `helpdesk` to Remote Desktop Users group
- Resolved issue by logging into `helpdesk` account directly and re-adding permissions successfully

### Remote Assistance Session Testing
- On `Desktop2`, created Remote Assistance invitation file
- Accessed invitation from `Desktop1` using:
  - `\\Desktop2\c$`
- Connected successfully using invitation password
- Tested chat functionality and remote control access
- Verified remote control by interacting with desktop and editing text remotely
