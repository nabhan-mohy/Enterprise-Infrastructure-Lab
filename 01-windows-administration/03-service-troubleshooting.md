# 1.3 — Windows Services & Application Support

> **Lab Area:** Windows Administration  
> **Module:** 1.3 — Windows Services & Application Support  
> **Environment:** Virtualized Enterprise IT Support Lab  
> **Platform:** Microsoft Windows  
> **Primary Tools:** PowerShell, Windows Services, Event Viewer

---

## 1. Module Overview

Windows services are background processes that provide essential operating-system and application functionality.

IT support engineers and system administrators regularly troubleshoot services when users report problems such as:

- Applications failing to start
- Network connectivity problems
- Authentication failures
- Update failures
- Background tasks not running
- Monitoring agents stopping
- Backup failures
- Security services becoming unavailable
- Server applications becoming inaccessible

In this module, Windows services were inspected and a controlled test service was created to simulate a service-management and troubleshooting incident.

The exercise focused on:

- Service inventory
- Service status analysis
- Startup-type identification
- Service creation
- Service startup testing
- Event Viewer investigation
- Error identification
- Root-cause analysis
- Troubleshooting validation
- Evidence collection
- Administrative cleanup

---

# 2. Objectives

The objectives of this module were to:

- Understand Windows service architecture.
- Inventory installed Windows services.
- Identify important operating-system services.
- Check service status.
- Identify service startup types.
- Create a controlled test service.
- Attempt to start the test service.
- Investigate a failed service startup.
- Use Windows Event Viewer and PowerShell event logs.
- Identify relevant service-related Event IDs.
- Correlate service failures with Windows event logs.
- Determine the likely cause of the failure.
- Document the troubleshooting process.
- Validate the final service state.
- Remove temporary laboratory changes where appropriate.

---

# 3. What Is a Windows Service?

A Windows service is a background component that can run without a user actively interacting with the desktop.

Services are commonly used for:

- Networking
- Windows Update
- Security
- Logging
- Remote management
- Application infrastructure
- Backup
- Monitoring
- Database systems
- Endpoint protection

A service generally has properties such as:

```text
Service Name
Display Name
Status
Startup Type
Account
Description
Executable / Binary Path
```

A service may have one of several common states:

```text
Running
Stopped
Paused
Start Pending
Stop Pending
```

The startup configuration may include:

```text
Automatic
Automatic (Delayed Start)
Manual
Disabled
```

Understanding these properties is important when diagnosing service-related incidents.

---

# 4. Initial Service Inventory

The first step was to inspect the Windows service inventory.

### Command

```powershell
Get-Service
```

This command returns the services registered on the Windows workstation.

Important services observed during the baseline included:

```text
BITS
DHCP
DNS Client
EventLog
LanmanServer
LanmanWorkstation
Windows Defender Firewall
Remote Procedure Call (RPC)
```

These services represent different areas of Windows functionality.

For example:

| Service | General Function |
|---|---|
| BITS | Background file transfers |
| DHCP Client | Obtains network configuration |
| DNS Client | Performs DNS client-side resolution/caching |
| EventLog | Windows event logging |
| LanmanServer | File and printer sharing services |
| LanmanWorkstation | SMB client functionality |
| Windows Defender Firewall | Host firewall |
| RPC | Windows distributed service communication |

---

### 📸 Screenshot — Windows Service Inventory

<img src="screenshots/01-service-inventory.png"
     width="700"
     alt="PowerShell showing Windows service inventory">

**Evidence:** PowerShell `Get-Service` output showing the Windows service inventory.

---

# 5. Service Properties

Individual service properties can be inspected using:

```powershell
Get-Service -Name "ServiceName"
```

Additional information can be obtained with:

```powershell
Get-CimInstance Win32_Service
```

For example:

```powershell
Get-CimInstance Win32_Service |
Select-Object Name, DisplayName, State, StartMode, StartName
```

This provides useful information about:

```text
Service name
Display name
Current state
Startup mode
Service account
```

This type of information is useful during service-related troubleshooting.

---

# 6. Controlled Troubleshooting Scenario

## Incident Scenario

A controlled service failure was created inside the laboratory environment.

The purpose was to simulate an IT support incident where a Windows service has been installed but fails to start correctly.

The temporary service was named:

```text
SOC-Lab-Service
```

Display name:

```text
SOC Lab Test Service
```

Description:

```text
Temporary service used for IT support troubleshooting lab
```

Startup type:

```text
Manual
```

The service was intentionally created as a test service so that the failure could be investigated without intentionally disrupting an important Windows production service.

---

# 7. Create the Test Service

The temporary service was created using:

```powershell
New-Service `
    -Name "SOC-Lab-Service" `
    -BinaryPathName "C:\Windows\System32\cmd.exe /c exit 0" `
    -DisplayName "SOC Lab Test Service" `
    -Description "Temporary service used for IT support troubleshooting lab" `
    -StartupType Manual
```

The command registers a new Windows service using a controlled test executable path.

The service was created for troubleshooting practice.

---

### 📸 Screenshot — Test Service Creation

<img src="screenshots/02-create-soc-lab-service.png"
     width="700"
     alt="PowerShell creating the SOC Lab Test Service">

**Evidence:** PowerShell showing the `New-Service` command used to create the temporary laboratory service.

---

# 8. Verify the New Service

After creating the service, its status was checked.

### Command

```powershell
Get-Service -Name "SOC-Lab-Service"
```

The service was successfully registered.

The captured configuration showed:

```text
Service Name:
SOC-Lab-Service

Display Name:
SOC Lab Test Service

Status:
Stopped

Startup Type:
Manual
```

The service was therefore present in the Windows Service Control Manager but was not currently running.

---

### 📸 Screenshot — Test Service Status

<img src="screenshots/03-soc-lab-service-status.png"
     width="700"
     alt="PowerShell showing SOC-Lab-Service status">

**Evidence:** PowerShell showing the newly created service and its current state.

---

# 9. Attempt to Start the Service

The next troubleshooting step was to attempt to start the service.

### Command

```powershell
Start-Service -Name "SOC-Lab-Service"
```

The start operation failed.

Windows reported that the service did not respond correctly to the start request.

This created the controlled troubleshooting incident.

---

### 📸 Screenshot — Service Start Failure

<img src="screenshots/04-service-start-failure.png"
     width="700"
     alt="PowerShell showing SOC-Lab-Service startup failure">

**Evidence:** PowerShell showing the failed `Start-Service` operation.

---

# 10. Initial Troubleshooting

When a Windows service fails to start, the first step is to avoid immediately changing configuration.

Instead, the administrator should gather evidence.

The following information should be checked:

```text
1. Current service state
2. Startup type
3. Service account
4. Executable path
5. Dependencies
6. Recent Windows events
7. Application events
8. System events
9. Service Control Manager events
10. Whether the executable can run correctly
```

The service was therefore investigated using both PowerShell and Event Viewer.

---

# 11. Check Service Configuration

The service configuration was inspected using:

```powershell
Get-CimInstance Win32_Service -Filter "Name='SOC-Lab-Service'" |
Select-Object Name, DisplayName, State, StartMode, StartName, PathName
```

This allows the administrator to verify the registered service configuration.

Important properties include:

```text
Name
DisplayName
State
StartMode
StartName
PathName
```

A mismatched or invalid executable path can prevent a service from starting.

---

### 📸 Screenshot — Service Configuration

<img src="screenshots/05-service-configuration.png"
     width="700"
     alt="PowerShell showing SOC-Lab-Service configuration">

**Evidence:** PowerShell showing the service configuration and executable path.

---

# 12. Event Viewer Investigation

After the startup failure, Windows Event Viewer was investigated.

Event Viewer can be opened using:

```powershell
eventvwr.msc
```

Relevant locations include:

```text
Windows Logs
├── Application
├── Security
├── Setup
└── System
```

For service startup problems, the **System** log is particularly useful because the Service Control Manager records service-related events there.

---

### 📸 Screenshot — Event Viewer

<img src="screenshots/06-event-viewer.png"
     width="700"
     alt="Windows Event Viewer showing system events">

**Evidence:** Windows Event Viewer used to investigate the service startup failure.

---

# 13. Query Windows Event Logs with PowerShell

Instead of relying exclusively on the graphical Event Viewer, Windows event logs can also be queried from PowerShell.

Example:

```powershell
Get-WinEvent -LogName System -MaxEvents 50
```

For service-related events, the administrator can search for Service Control Manager events:

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'System'
    ProviderName = 'Service Control Manager'
} -MaxEvents 50
```

This provides a command-line method for investigating service activity.

---

# 14. Service Installation Event

The service installation generated a Windows Service Control Manager event.

The captured evidence shows:

```text
Event ID: 7045
```

Event ID 7045 indicates that a service was installed in the system.

The event identified the laboratory service:

```text
Service Name:
SOC-Lab-Service

Service File Name:
C:\Windows\System32\cmd.exe /c exit 0

Service Type:
User mode service

Service Start Type:
Demand start

Service Account:
LocalSystem
```

This is useful evidence because it confirms that Windows registered the test service and records important installation details.

---

### 📸 Screenshot — Service Installation Event

<img src="screenshots/07-service-installation-event.png"
     width="700"
     alt="Windows event log showing service installation event 7045">

**Evidence:** Event Viewer / PowerShell event output showing Event ID 7045 for the laboratory service installation.

---

# 15. Service Startup Failure Events

The failed startup generated Service Control Manager events.

The captured investigation identified events associated with the service startup failure, including:

```text
Event ID 7000
Event ID 7009
```

These events are important when investigating services that fail to start.

The captured Event ID 7009 information indicated a timeout while waiting for the service to connect.

The event showed that Windows waited for approximately:

```text
30 seconds
```

for the service to respond.

---

### 📸 Screenshot — Service Failure Events

<img src="screenshots/08-service-failure-events.png"
     width="700"
     alt="Windows event log showing service startup failure events">

**Evidence:** Service Control Manager events showing the service startup failure and timeout.

---

# 16. Event Correlation

The incident can be reconstructed from the collected evidence.

```text
Service created
      |
      v
SOC-Lab-Service registered
      |
      v
Service remains stopped
      |
      v
Start-Service executed
      |
      v
Service fails to start
      |
      v
Windows records Service Control Manager events
      |
      +-------------------+
      |                   |
      v                   v
   Event 7000         Event 7009
   Start failure      Start timeout
      |                   |
      +---------+---------+
                |
                v
       Troubleshooting evidence
```

The installation event and subsequent failure events provide a timeline of the incident.

---

# 17. Root-Cause Analysis

The service was intentionally configured with:

```text
C:\Windows\System32\cmd.exe /c exit 0
```

as its service executable command.

Although the command itself can execute from a normal command shell, a Windows service must communicate with the Windows Service Control Manager according to the expected service interface.

The test executable does not behave as a normal Windows service process.

As a result, the Service Control Manager waited for the expected service response and eventually timed out.

This explains the observed startup failure and Event ID 7009 timeout.

### Root Cause

```text
The registered executable was not implemented as a
proper Windows service process.

Therefore the Service Control Manager could not
establish the expected service connection during startup.
```

---

# 18. Why This Is a Useful IT Support Scenario

This controlled failure demonstrates a realistic troubleshooting principle:

> A service can exist in Windows and still fail to start.

An administrator should therefore distinguish between:

```text
Service exists
        ≠
Service is working
```

A service can be:

```text
Installed
Registered
Configured
Stopped
```

and still be unusable.

The administrator must verify the actual runtime state.

---

# 19. Troubleshooting Workflow

A practical Windows service troubleshooting workflow is:

```text
User reports application/service problem
                |
                v
Identify affected service
                |
                v
Check current service status
                |
                v
Check startup type
                |
                v
Check service account
                |
                v
Check executable path
                |
                v
Check dependencies
                |
                v
Review Event Viewer
                |
                v
Query Service Control Manager events
                |
                v
Identify error / timeout
                |
                v
Determine root cause
                |
                v
Apply corrective action
                |
                v
Restart / re-test
                |
                v
Verify service state
                |
                v
Document incident
```

---

# 20. Validation Commands

The following commands can be used to validate a Windows service.

### Service status

```powershell
Get-Service -Name "SOC-Lab-Service"
```

### Detailed service information

```powershell
Get-CimInstance Win32_Service -Filter "Name='SOC-Lab-Service'"
```

### Service configuration

```powershell
sc.exe qc SOC-Lab-Service
```

### Service status through `sc.exe`

```powershell
sc.exe query SOC-Lab-Service
```

### System service events

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'System'
    ProviderName = 'Service Control Manager'
} -MaxEvents 50
```

### Search for specific service events

```powershell
Get-WinEvent -FilterHashtable @{
    LogName = 'System'
    Id = 7000,7009,7045
} -MaxEvents 50
```

---

# 21. Event ID Reference

The incident demonstrated several useful Service Control Manager event IDs.

| Event ID | Meaning / Relevance |
|---:|---|
| 7000 | Service failed to start |
| 7009 | Service startup timeout |
| 7045 | A service was installed |

These event IDs are useful starting points during Windows service investigations.

> Event IDs should always be interpreted together with the event message, service name, timestamp, and surrounding events rather than in isolation.

---

# 22. Service Dependencies

In a real enterprise environment, a service may depend on other Windows services.

Dependencies can be checked using:

```powershell
Get-Service -Name "ServiceName" |
Select-Object -ExpandProperty ServicesDependedOn
```

and:

```powershell
Get-Service -Name "ServiceName" |
Select-Object -ExpandProperty DependentServices
```

A service may fail to start because one of its required dependencies is:

```text
Stopped
Disabled
Failed
Misconfigured
Unavailable
```

Therefore dependency checking should be part of the standard troubleshooting workflow.

---

# 23. Service Account Considerations

Windows services may run under different security identities.

Common service accounts include:

```text
LocalSystem
LocalService
NetworkService
Specific local account
Domain account
```

The account determines what resources the service can access.

When a service fails, the administrator should verify whether the configured service account has appropriate permissions.

For example:

```text
Service
   |
   v
Service Account
   |
   v
File Permissions
   |
   v
Registry Permissions
   |
   v
Network Permissions
   |
   v
Application Resources
```

Permission problems can cause service startup or application failures.

---

# 24. Change Management

Creating or modifying a Windows service is an administrative change.

A professional change record should document:

```text
Change requested
Change performed
Reason for change
System affected
Administrator
Date/time
Previous configuration
New configuration
Validation
Rollback / cleanup
```

For this laboratory exercise:

```text
Change:
Created SOC-Lab-Service

Purpose:
Controlled service troubleshooting exercise

System:
MOHY

Service:
SOC-Lab-Service

Startup Type:
Manual

Result:
Service creation successful

Startup Test:
Failed

Investigation:
Event Viewer / Service Control Manager

Finding:
Service startup timeout

Cleanup:
Temporary laboratory service should be removed after evidence collection
```

---
