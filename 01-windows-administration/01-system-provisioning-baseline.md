# 1.1 — Windows System Provisioning & Baseline

> **Lab Area:** Windows Administration  
> **Module:** 1.1 — System Provisioning & Baseline  
> **Environment:** Virtualized Enterprise IT Support Lab  
> **Platform:** Microsoft Windows + VirtualBox  
> **Primary Tools:** PowerShell, Windows System Information, Windows Networking Tools

---

## 1. Module Overview

This module establishes the initial system baseline for a Windows endpoint in a simulated enterprise IT support environment.

The purpose of a system baseline is to document the known state of a workstation before additional administration, troubleshooting, monitoring, security testing, or configuration changes are performed.

During this module, the Windows workstation was inspected and its:

- Operating system
- Hostname
- Hardware
- Processor
- Memory
- Storage
- BIOS/firmware
- Network configuration
- DHCP configuration
- DNS configuration
- Local user accounts
- Administrative group membership
- Virtualization environment
- Security configuration
- Installed hotfix information

were documented.

This baseline will be used as a reference point for later modules in the Enterprise Infrastructure Lab.

---

# 2. Objectives

The objectives of this module were to:

- Establish a known baseline for the Windows workstation.
- Verify the computer hostname.
- Identify the installed Windows edition and version.
- Collect operating-system build information.
- Identify the virtualization platform.
- Collect CPU and memory information.
- Inspect storage volumes and filesystem types.
- Verify BIOS/firmware information.
- Review local user accounts.
- Review local administrator membership.
- Identify the active network adapter.
- Verify IPv4 configuration.
- Verify DHCP configuration.
- Identify the configured DNS server.
- Test basic network connectivity.
- Perform a basic DNS resolution test.
- Record the initial security-related configuration.
- Preserve screenshots and command output as evidence.

---

# 3. Lab Environment

The workstation used for this module is a virtual machine running under VirtualBox.

| Category | Observed Configuration |
|---|---|
| Hostname | `MOHY` |
| Operating System | Windows Professional |
| System Type | x64-based PC |
| Virtualization | VirtualBox |
| Domain | `WORKGROUP` |
| CPU | AMD Ryzen 5 5500H with Radeon Graphics |
| CPU Cores | 2 |
| Logical Processors | 2 |
| Physical Memory | ~4.3 GB |
| Primary Filesystem | NTFS |
| Network Adapter | Intel(R) PRO/1000 MT Desktop Adapter |
| IPv4 Address | `192.168.1.10` |
| Subnet Mask | `255.255.255.0` |
| DHCP Server | `192.168.1.1` |
| DNS Server | `192.168.1.1` |
| Default Gateway | `192.168.1.1` |
| DHCP | Enabled |
| VBS | Not enabled |
| App Control Policy | Enforced |
| User Mode Policy | Audit |


> **Security note:** Personally identifying information, Windows Product IDs, hardware serial numbers, email addresses, and unnecessary MAC-address information should not be published in a public GitHub repository. Any such information in screenshots should be cropped or redacted before uploading.

---

# 4. System Identification

## 4.1 Hostname Verification

The first step was to identify the hostname of the workstation.

### Command

```powershell
hostname
```

The workstation returned:

```text
mohy
```

The system information output also identified the computer as:

```text
Host Name: MOHY
```

The hostname is important for:

- Help-desk ticket identification
- Endpoint inventory
- Remote administration
- Event-log correlation
- Network troubleshooting
- Monitoring
- Incident documentation

---

<img width="208" height="208" alt="MODULE-1 1 1" src="https://github.com/user-attachments/assets/cc23aeb7-9820-45e7-b4a2-728827b27d9e" />

**Evidence:** PowerShell showing the `hostname` command and `systeminfo` output.

**Insert screenshot here:**


<img width="1204" height="653" alt="MODULE-1 1 2" src="https://github.com/user-attachments/assets/9d7085fa-414e-4e51-adf1-ca3004e0bf3b" />

---

# 5. Operating System Baseline

The Windows operating system was inspected using the built-in `systeminfo` utility and PowerShell `Get-ComputerInfo`.

## 5.1 `systeminfo`

### Command

```powershell
systeminfo
```

The captured system information reported:

```text
OS Name                  : Microsoft Windows 11 Pro
OS Version               : 10.0.26200
OS Build Type            : Multiprocessor Free
System Manufacturer      : innotek GmbH
System Model             : VirtualBox
System Type              : x64-based PC
Domain                   : WORKGROUP
```

The system information also showed approximately:

```text
Total Physical Memory    : 4,329 MB
Available Physical Memory: 662 MB
```

and confirmed that the system was operating in a VirtualBox environment.

---

## 5.2 `Get-ComputerInfo`

Additional Windows information was collected using:

```powershell
Get-ComputerInfo
```

The captured output reported:

```text
WindowsEditionId        : Professional
WindowsInstallationType : Client
WindowsVersion          : 2009
OSDisplayVersion        : 25H2
BiosCaption             : VirtualBox
BiosDescription         : VirtualBox
BiosFirmwareType        : Uefi
BiosManufacturer        : innotek GmbH
```

### Important Evidence Note

The captured evidence contains a discrepancy between the `systeminfo` and `Get-ComputerInfo` outputs.

`systeminfo` reports:

```text
Microsoft Windows 11 Pro
OS Version: 10.0.26200
```

while the captured `Get-ComputerInfo` output identifies:

```text
WindowsProductName : Windows 10 Pro
OSDisplayVersion   : 25H2
```

This discrepancy is intentionally documented rather than silently corrected because both values are present in the collected lab evidence.

For future troubleshooting, the exact command output should be treated as the authoritative historical evidence for that collection point.

---

### 📸 Screenshot — Windows OS Baseline

**Evidence:** `Get-ComputerInfo` output showing Windows edition, version, BIOS, and installation information.

**Insert screenshot here:**


<img width="1160" height="770" alt="MODULE-1 1 3" src="https://github.com/user-attachments/assets/c371fd04-2e2a-46aa-94ca-c2def8be9b14" />

---

# 6. Computer and Virtualization Information

The computer hardware and virtualization environment were queried using:

```powershell
Get-CimInstance Win32_ComputerSystem
```

The result identified:

```text
Name                  : MOHY
Domain                : WORKGROUP
TotalPhysicalMemory   : 4538966016
Model                 : VirtualBox
Manufacturer          : innotek GmbH
```

This confirms that the workstation is running as a VirtualBox virtual machine.

The system also reported the presence of a hypervisor.

Virtualization is useful for this laboratory because it allows administrative and troubleshooting activities to be performed in an isolated environment.

---

### 📸 Screenshot — Computer System Information

**Evidence:** `Get-CimInstance Win32_ComputerSystem`

**Insert screenshot here:**

<img width="1063" height="829" alt="MODULE-1 1 4" src="https://github.com/user-attachments/assets/57433a93-d764-492a-8828-9c5bbdc3e49f" />

Recommended filename:

<img width="583" height="600" alt="MODULE-1 1 5" src="https://github.com/user-attachments/assets/a1acdf7e-8bc7-418e-8dce-98786d2a57ad" />


---

# 7. Hardware Inventory

Hardware information was collected to establish the initial endpoint resource baseline.

---

## 7.1 Physical Memory

### Command

```powershell
Get-CimInstance Win32_ComputerSystem |
Select-Object TotalPhysicalMemory
```

Observed value:

```text
TotalPhysicalMemory
-------------------
4538******
```

This represents approximately 4.3 GB of physical memory allocated to the virtual machine.

Memory information is useful when investigating:

- Slow system performance
- Application crashes
- High memory utilization
- Paging
- Browser performance
- Resource exhaustion

---

## 7.2 Processor

### Command

```powershell
Get-CimInstance Win32_Processor |
Select-Object Name, NumberOfCores, NumberOfLogicalProcessors
```

Observed configuration:

```text
Name:
AMD Ryzen 5 5500H with Radeon Graphics

NumberOfCores:
2

NumberOfLogicalProcessors:
2
```

This information provides the CPU baseline for future performance investigations.

---

### 📸 Screenshot — Hardware and Processor Inventory

**Evidence:** Memory and processor information.

**Insert screenshot here:**

<img width="583" height="600" alt="MODULE-1 1 5" src="https://github.com/user-attachments/assets/861d6198-80aa-4edf-b6a4-450496afe5ea" />


---

# 8. Storage Baseline

Storage configuration was collected using:

```powershell
Get-Volume |
Select-Object DriveLetter, FileSystemLabel, FileSystem, Size, SizeRemaining
```

The primary Windows volume was identified as:

```text
DriveLetter : C
FileSystem  : NTFS
```

The captured C: volume had approximately:

```text
Size          : 84,844,474,368 bytes
SizeRemaining : 51,210,625,024 bytes
```

Additional volumes included the EFI/FAT32 volume, an NTFS system-related volume, and the VirtualBox Guest Additions CD-ROM.

The storage baseline provides a reference for later troubleshooting involving:

- Low disk space
- Windows Update failures
- Application installation failures
- Temporary-file growth
- Log-file growth
- Filesystem issues
- Storage performance

---

### 📸 Screenshot — Storage Baseline

**Evidence:** `Get-Volume` showing the C: volume, filesystem, size, and remaining capacity.

**Insert screenshot here:**

```text
[SCREENSHOT 05 — STORAGE BASELINE]
```

Recommended filename:

```text
screenshots/05-storage-baseline.png
```

---

# 9. BIOS and Firmware Baseline

BIOS information was collected using:

```powershell
Get-CimInstance Win32_BIOS |
Select-Object Manufacturer, SMBIOSBIOSVersion, ReleaseDate
```

Observed information:

```text
Manufacturer        : innotek GmbH
SMBIOSBIOSVersion   : VirtualBox
```

The system information also identified the firmware type as:

```text
UEFI
```

The BIOS baseline can be useful when troubleshooting:

- Boot problems
- Firmware compatibility
- Virtual machine configuration
- Hardware initialization
- Secure Boot-related issues
- System startup problems

---

### 📸 Screenshot — BIOS / Firmware Information

**Insert screenshot here:**

```text
[SCREENSHOT 06 — BIOS AND FIRMWARE BASELINE]
```

Recommended filename:

```text
screenshots/06-bios-firmware.png
```

---

# 10. Network Configuration Baseline

The initial network configuration was collected using:

```powershell
ipconfig /all
```

The workstation reported:

```text
Host Name        : mohy
Node Type        : Hybrid
IP Routing       : No
WINS Proxy       : No
```

The primary Ethernet adapter was:

```text
Intel(R) PRO/1000 MT Desktop Adapter
```

The adapter was configured to use DHCP.

Observed network configuration:

```text
DHCP Enabled     : Yes
IPv4 Address     : 192.168.1.10
Subnet Mask      : 255.255.255.0
Default Gateway  : 192.168.1.1
DHCP Server      : 192.168.1.1
DNS Server       : 192.168.1.1
```

The network baseline provides the information required for later connectivity and DNS troubleshooting.

---

### 📸 Screenshot — Network Configuration

**Evidence:** `ipconfig /all` output showing the network adapter, IPv4 address, DHCP, gateway, and DNS server.

**Insert screenshot here:**

```text
[SCREENSHOT 07 — NETWORK CONFIGURATION BASELINE]
```

Recommended filename:

```text
screenshots/07-network-configuration.png
```

---

# 11. PowerShell Network Configuration Verification

The Windows network configuration was also verified through PowerShell.

### Command

```powershell
Get-NetIPConfiguration
```

The Ethernet interface reported:

```text
InterfaceAlias    : Ethernet
InterfaceIndex    : 7
InterfaceDescription : Intel(R) PRO/1000 MT Desktop Adapter
IPv4DefaultGateway: 192.168.1.1
IPv4Address       : 192.168.1.10
DNSServer         : 192.168.1.1
```

This provided a second method of validating the network configuration.

Using multiple commands to validate important information is a useful troubleshooting practice.

---

### 📸 Screenshot — PowerShell Network Configuration

**Insert screenshot here:**

```text
[SCREENSHOT 08 — GET-NETIPCONFIGURATION]
```

Recommended filename:

```text
screenshots/08-get-netipconfiguration.png
```

---

# 12. Network Adapter Status

The Ethernet adapter status was checked using:

```powershell
Get-NetAdapter |
Select-Object Name, InterfaceDescription, Status, MacAddress, LinkSpeed
```

The adapter was reported as:

```text
Name                : Ethernet
InterfaceDescription: Intel(R) PRO/1000 MT Desktop Adapter
Status              : Up
LinkSpeed           : 1 Gbps
```

The adapter being reported as `Up` indicates that the Windows network interface was operational at the time of collection.

---

### 📸 Screenshot — Network Adapter Status

**Insert screenshot here:**

```text
[SCREENSHOT 09 — NETWORK ADAPTER STATUS]
```

Recommended filename:

```text
screenshots/09-network-adapter-status.png
```

> **Public GitHub note:** Crop or redact the physical MAC address before publishing the screenshot.

---

# 13. DNS Configuration

The configured DNS server was checked using:

```powershell
Get-DnsClientServerAddress -AddressFamily IPv4
```

The Ethernet interface reported:

```text
InterfaceAlias : Ethernet
AddressFamily  : IPv4
ServerAddresses: 192.168.1.1
```

This confirms that the workstation was configured to use `192.168.1.1` as its DNS server.

---

# 14. DNS Resolution Test

A basic DNS lookup was performed using:

```powershell
nslookup google.com
```

The initial query reported:

```text
Server:  Unknown
Address: 192.168.1.1
```

The lookup initially showed:

```text
DNS request timed out.
```

However, the query subsequently returned an address for `google.com`.

A direct DNS resolution test was also performed using:

```powershell
Resolve-DnsName google.com
```

The command successfully returned DNS records, including:

```text
google.com
Type: A
```

and an IPv4 address.

This demonstrates an important troubleshooting observation:

> A DNS request timeout does not necessarily mean that DNS is completely unavailable. Multiple tests should be performed before concluding that the DNS service is down.

---

### 📸 Screenshot — DNS Configuration and Resolution

**Evidence:** DNS server configuration, `nslookup google.com`, and `Resolve-DnsName google.com`.

**Insert screenshot here:**

```text
[SCREENSHOT 10 — DNS CONFIGURATION AND RESOLUTION]
```

Recommended filename:

```text
screenshots/10-dns-resolution-test.png
```

---

# 15. Windows Hotfix Baseline

Installed Windows hotfix information was collected using:

```powershell
Get-HotFix
```

The captured baseline contained five installed hotfixes, including:

```text
KB5066128
KB5054156
KB5095189
KB5079473
KB5083532
```

Patch information is important for:

- Vulnerability management
- Security compliance
- Troubleshooting update-related issues
- Identifying missing patches
- Change management
- Incident investigation

This information can be compared with later Windows Update results.

---

### 📸 Screenshot — Installed Hotfixes

**Insert screenshot here:**

```text
[SCREENSHOT 11 — WINDOWS HOTFIX BASELINE]
```

Recommended filename:

```text
screenshots/11-windows-hotfixes.png
```

---

# 16. Local User Baseline

The local user accounts were inspected using:

```powershell
Get-LocalUser
```

The captured system contained the following accounts:

| Account | Enabled | Description |
|---|---:|---|
| Administrator | No | Built-in administrative account |
| DefaultAccount | No | System-managed account |
| Guest | No | Built-in guest account |
| `kewex` | Yes | Primary user account |
| WDAGUtilityAccount | No | Windows Defender Application Guard-related account |

The primary user account identified during the baseline was:

```text
kewex
```

The local user inventory is important for later authentication and privilege troubleshooting.

---

### 📸 Screenshot — Local User Inventory

**Evidence:** `Get-LocalUser` output.

**Insert screenshot here:**

```text
[SCREENSHOT 12 — LOCAL USER INVENTORY]
```

Recommended filename:

```text
screenshots/12-local-user-inventory.png
```

---

# 17. Local Administrator Group Baseline

Local administrator membership was checked using:

```powershell
Get-LocalGroupMember -Group "Administrators"
```

The captured configuration included:

```text
MOHY\Administrator
MOHY\kewex
```

This establishes which accounts have local administrative privileges.

Administrator membership is important when investigating:

- Access denied errors
- Software installation problems
- Service-management issues
- Configuration changes
- Privilege escalation concerns
- User access problems

---

### 📸 Screenshot — Administrator Group Membership

**Insert screenshot here:**

```text
[SCREENSHOT 13 — ADMINISTRATOR GROUP MEMBERSHIP]
```

Recommended filename:

```text
screenshots/13-administrator-group.png
```

---

# 18. Security Baseline

The initial system security state was also captured.

The Windows system reported:

```text
Virtualization-Based Security:
Not enabled
```

The captured configuration also reported:

```text
App Control for Business policy:
Enforced

App Control for Business user mode policy:
Audit
```

These settings were documented as part of the baseline.

They provide a reference point for later security-hardening and security-monitoring activities.

---

### 📸 Screenshot — Security Baseline

**Insert screenshot here:**

```text
[SCREENSHOT 14 — SECURITY BASELINE]
```

Recommended filename:

```text
screenshots/14-security-baseline.png
```

---

# 19. System Identity Verification

The current logged-in identity was checked using:

```powershell
whoami
```

Observed result:

```text
mohy\kewex
```

This confirms the Windows account context under which the PowerShell session was operating.

This is particularly important when performing administrative troubleshooting because command results and access permissions can depend on the current user context.

---

### 📸 Screenshot — Current User Identity

**Insert screenshot here:**

```text
[SCREENSHOT 15 — WHOAMI]
```

Recommended filename:

```text
screenshots/15-whoami.png
```

---

# 20. Basic Connectivity Validation

Basic network connectivity was tested using:

```powershell
Test-NetConnection 8.8.8.8
```

The captured result showed:

```text
ComputerName     : 8.8.8.8
RemoteAddress    : 8.8.8.8
InterfaceAlias   : Ethernet
SourceAddress     : 192.168.1.10
PingSucceeded     : True
```

This confirms that the workstation was able to reach the test destination during the baseline collection.

The source address confirms that traffic was being sent through the Ethernet interface using:

```text
192.168.1.10
```

---

### 📸 Screenshot — Connectivity Test

**Insert screenshot here:**

```text
[SCREENSHOT 16 — TEST-NETCONNECTION]
```

Recommended filename:

```text
screenshots/16-connectivity-test.png
```

---

# 21. Baseline Validation

After collecting the system information, the following baseline checks were completed.

| Validation | Result |
|---|---|
| Hostname identified | ✅ |
| Operating system identified | ✅ |
| Windows build information collected | ✅ |
| Virtualization platform identified | ✅ |
| CPU information collected | ✅ |
| Memory information collected | ✅ |
| Storage volumes identified | ✅ |
| Filesystem identified | ✅ |
| BIOS information collected | ✅ |
| Network adapter identified | ✅ |
| IPv4 address verified | ✅ |
| Subnet mask verified | ✅ |
| Default gateway verified | ✅ |
| DHCP configuration verified | ✅ |
| DNS server identified | ✅ |
| DNS resolution tested | ✅ |
| Local users reviewed | ✅ |
| Administrator membership reviewed | ✅ |
| Security configuration recorded | ✅ |
| Installed hotfixes documented | ✅ |
| Internet connectivity tested | ✅ |

---

# 22. Baseline Summary

The initial Windows endpoint baseline can be summarized as follows:

```text
Hostname:
MOHY

Environment:
VirtualBox

Domain:
WORKGROUP

Primary User:
kewex

IPv4:
192.168.1.10

Subnet:
255.255.255.0

Default Gateway:
192.168.1.1

DHCP Server:
192.168.1.1

DNS Server:
192.168.1.1

Network Interface:
Intel(R) PRO/1000 MT Desktop Adapter

CPU:
AMD Ryzen 5 5500H

CPU Cores:
2

Logical Processors:
2

Memory:
~4.3 GB

Primary Filesystem:
NTFS
```

---

# 23. IT Support Relevance

System provisioning and baseline collection is a fundamental IT support and system administration activity.

Before troubleshooting a workstation, an administrator should know what the system looks like under normal conditions.

For example, if a user reports that the workstation cannot access the Internet, the baseline provides the information needed to immediately inspect:

```text
IP address
    ↓
Subnet mask
    ↓
Default gateway
    ↓
Network adapter
    ↓
DHCP
    ↓
DNS
    ↓
External connectivity
```

If the system later develops performance problems, the hardware baseline provides a reference for:

```text
CPU
Memory
Storage
Running processes
Network configuration
```

If a user reports an access problem, the local-account baseline provides information about:

```text
User account
Account status
Administrator membership
Authentication context
```

This makes the baseline useful beyond this single module.

---

# 24. Troubleshooting Value of the Baseline

The collected information can be used during future incidents.

## Example: Internet Connectivity Problem

A future support ticket might report:

> "The workstation cannot access the Internet."

The administrator can compare the current configuration against the baseline:

```text
Baseline IP:
192.168.1.10

Baseline Gateway:
192.168.1.1

Baseline DNS:
192.168.1.1

Baseline Adapter:
Ethernet

Baseline Adapter Status:
Up
```

Any unexpected difference can become a troubleshooting lead.

---

## Example: DNS Problem

The baseline already includes DNS testing.

The captured evidence showed that `nslookup google.com` initially reported a timeout while subsequent DNS resolution succeeded.

This demonstrates why troubleshooting should not rely on a single command.

A support engineer could perform:

```powershell
ipconfig /all
```

then:

```powershell
Get-DnsClientServerAddress -AddressFamily IPv4
```

then:

```powershell
nslookup google.com
```

and:

```powershell
Resolve-DnsName google.com
```

The results can then be compared to the baseline.

---
