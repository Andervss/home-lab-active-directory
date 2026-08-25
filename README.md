# Active Directory Home Lab

## Overview
Built a virtual Windows Active Directory lab using VirtualBox, Windows Server 2022 and Windows 10 clients.

The lab is used to practise:
- Active Directory administration
- User and group management
- Group Policy
- Shared folders and permissions
- Windows networking and troubleshooting
- Remote administration
- Software deployment and inventory management

## Lab Environment
- **Server:** Windows Server 2022
- **Clients:** Windows 10 Pro
- **Virtualisation:** VirtualBox
- **Domain:** `andervss.com`
- **NetBIOS:** `ANDERVSS`

### Server2022
- 8GB RAM
- 2 CPUs
- 50GB storage
- Renamed from the default hostname to `Server2022`
- Installed Active Directory Domain Services (AD DS)
- Promoted to Domain Controller

## Server Networking - Changing Static IP on Server 2022 Lab
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
- `taskmgr`

## Group Policy & Useful Commands

### Group Policy Commands
- `gpresult /r` → Displays applied user and computer policies
- `gpresult /h gpresult.html` → Generates an HTML report of applied Group Policy
- `gpupdate /force` → Refreshes and reapplies Group Policy settings

### Network Notes
- `DHCP Enabled = Yes` → Dynamic IP address
- `DHCP Enabled = No` → Static IP address

## Desktop1 Windows 10 Pro Lab
- Created a Windows 10 Pro client VM (`Desktop1`) for testing within the domain.
- Initially attempted to use a Windows 11 ISO but encountered a black screen during the first boot.
- Switched to Windows 10 after troubleshooting compatibility issues with VirtualBox.
- Allocated 8GB RAM, 2 CPUs and 50GB storage.
- Enabled the built-in Administrator account.
- Removed the standard user account to simplify the lab setup.
- Renamed the computer to `Desktop1`.
- Configured a static IP:
  - IP address: `10.1.10.3`
  - Subnet mask: `255.0.0.0`
  - Default gateway: `10.1.10.1`
  - Preferred DNS server: `10.1.10.2`
  - Alternate DNS server: `10.1.10.1`
- Changed the VirtualBox network adapter from NAT to Host-only Adapter.
- Joined the `andervss.com` domain using domain administrator credentials.
- Successfully logged into the domain using the `helpdesk` account.
- Practised creating organisational units, user accounts and moving accounts between OUs.
- Explored Group Policy Management, including:
  - Password policy
  - Account lockout duration
  - Account lockout threshold
  - Maximum password age

## Remote Server Administration Tools (RSAT)
- Used RSAT to explore managing Windows Server from a Windows client.
- RSAT allows administrators to manage Windows Server roles and services remotely without directly accessing the server.
- Tools explored:
  - Server Manager
  - Active Directory Domain Services Tools
  - Group Policy Management Tools
  - DNS Server Tools
  - DHCP Server Tools

## Desktop2 Windows 10 Pro Lab
- Created a second Windows 10 Pro client VM (`Desktop2`).
- Configured a static IP:
  - IP address: `10.1.10.4`
- Joined the `andervss.com` domain.
- Successfully logged into the domain using the `helpdesk` account.
- Tested account lockout using user `patty`.
- Practised unlocking accounts and resetting passwords through Active Directory.
- Removed `Desktop2` from the domain and successfully rejoined it.

## Shared Folders, Security Groups & Permissions
- Created shared folders `HR` and `Personal` through Server Manager.
- Configured shared network paths:
  - `\\SERVER2022\HR`
  - `\\SERVER2022\Personal`
- Created a security group called `HR`.
- Added user `patty` to the relevant security groups.
- Documented shared folder paths within group descriptions for easier administration.
- Disabled inheritance on shared folders to configure custom permissions.
- Removed default users/groups from the folder permissions.
- Added the `helpdesk` account and assigned Modify permissions.
- Configured `HR` and `Personal` folder permissions with Read/Write access.

### Share Access Testing
- Logged into `Desktop2` using user `patty`.
- Connected to the `HR` shared folder using:
  - `\\SERVER2022\HR`
- Created a shortcut and pinned the shared folder to Quick Access.
- Mapped network drives:
  - `Z:` drive → `HR` share
  - `P:` drive → `Personal` share using:
    - `\\SERVER2022\Personal\%username%`
- Tested adding and removing an additional user (`test`) from the relevant permissions/groups.
- Practised modifying user access and folder permissions.

## Remote Desktop & Remote Administration

### Remote Desktop
- Enabled Remote Desktop connections on `Desktop2`.
- Tested a Remote Desktop connection from `Desktop1`.
- Verified remote access by creating and moving folders on user `patty`'s desktop.

### Remote Registry
- Attempted to connect to `Desktop2` remotely using Registry Editor.
- Encountered an error related to the Remote Registry service.
- On `Desktop2`:
  - Set the `RemoteRegistry` service Startup Type to `Automatic`.
  - Started the service successfully.
- Successfully connected to `Desktop2` through Registry Editor after enabling the service.

### Administrative Shares & Remote File Access
- Accessed the hidden administrative share:
  - `\\Desktop2\c$`
- Tested remote file access by creating folders on user `patty`'s desktop.
- Explored available shared folders and permissions.

### Windows Remote Assistance
- Located Windows Remote Assistance using `msra` / `msra.exe`.
- Attempted to establish a Remote Assistance connection to `Desktop2`.
- Encountered a permissions issue when adding `helpdesk` to the Remote Desktop Users group for user `patty`.
- Resolved the issue by logging into the `helpdesk` account directly and re-adding the required permissions.

### Remote Assistance Session
- Created a Remote Assistance invitation file on `Desktop2`.
- Accessed the invitation from `Desktop1` using:
  - `\\Desktop2\c$`
- Connected successfully using the invitation password.
- Tested chat functionality and remote control.
- Verified remote control by interacting with `Desktop2` and editing text remotely.

## Group Policy Management (GPO)

### Creating and Applying Group Policy
- Created a Group Policy Object (GPO) called `Task Manager` in Group Policy Management on `Server2022`.
- Linked the GPO to the `HR` organisational unit (OU).
- Configured the following policies under User Configuration → Ctrl + Alt + Delete Options:
  - Remove Change Password
  - Remove Task Manager
- Applied the policy to user `patty`.

### Testing Group Policy on Desktop2
- Logged into `Desktop2` as `patty`.
- Forced a Group Policy refresh using:
  - `gpupdate /force`
- Verified the policy by confirming that `taskmgr` could not be opened.
- Tested the behaviour with Command Prompt launched as Administrator and confirmed Task Manager could still be accessed.

### Additional GPO Testing
- Created and tested a second GPO to remove the Recycle Bin icon from the desktop.
- Found that signing out and back in was required before the policy fully applied.
- Verified the Recycle Bin could still be accessed manually using:
  - `shell:RecycleBinFolder`

### Troubleshooting – Group Policy Results Wizard
- Attempted to run the Group Policy Results Wizard for `Desktop2`.
- Encountered an RPC Server error.
- Tested network connectivity using:
  - `ping 10.1.10.4`
- Checked the following services on `Desktop2`:
  - Remote Procedure Call (RPC)
  - RPC Endpoint Mapper
- Confirmed both services were running and set to `Automatic`.
- Temporarily disabled the Private Firewall to identify whether it was blocking the connection.
- Confirmed the firewall was causing the issue and re-enabled it after testing.

### RSOP / Group Policy Verification
- Used RSOP (Resultant Set of Policy) to review applied Group Policy settings.
- Verified that the expected policies were being applied to `patty`.

## PDQ Deploy

### Environment Preparation
- Installed VirtualBox Guest Additions and restarted the virtual machines.
- Created a shared folder `andervss_lab` and enabled Auto-mount.
- Downloaded PDQ Deploy (Free Edition) and stored the installer in the shared folder.
- Changed the network adapter to Bridged Adapter to provide internet access.
- Reverted IP and DNS settings to obtain addresses automatically using DHCP.
- Verified internet connectivity using:
  - `ping 8.8.8.8`
- Confirmed web browser access.

### Installing and Testing PDQ Deploy
- Installed PDQ Deploy (Free Edition) on `Server2022`.
- Created a deployment package using PDFsam Basic.
- Performed a test deployment targeting `Server2022`.
- Verified successful deployment by confirming PDFsam Basic was installed and accessible.

### PDQ Inventory
- Downloaded and installed PDQ Inventory on `Server2022`.
- Added `Desktop2` to the inventory and verified successful communication.
- Accessed the administrative share:
  - `\\Desktop2\c$`
- Tested remote file management by creating and moving files on the user's desktop.
- Tested remote reboot functionality on `Desktop2`.
- Verified Remote Desktop connectivity.
- Generated a PDF report showing installed applications on `Desktop2`.

### Remote Software Deployment
- Opened PDQ Deploy through PDQ Inventory and targeted `Desktop2`.
- Successfully deployed PDFsam Basic remotely.
- Verified that PDFsam Basic was installed successfully on `Desktop2`.

## Printer Management
- Added **Print Management** through Server Manager.
- Added **Microsoft Publisher Color Printer** for testing and published it in Active Directory.
- Configured the printer to be available to the **HR** group.
- Logged into `Desktop2` as `ANDERVSS\patty` and attempted to find and add the shared printer.
- **Issue:** Error **#740 – Operation could not be completed. Requested operation requires elevation.**
- **Cause:** Installing the printer required administrator privileges.
- **Resolution:** Logged into `Desktop2` using the `ANDERVSS\Administrator` account and installed the printer successfully.
- Logged back into `ANDERVSS\patty` and successfully connected to the installed printer.
