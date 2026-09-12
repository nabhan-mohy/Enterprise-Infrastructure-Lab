[01-installation-baseline.md](https://github.com/user-attachments/files/32141960/01-installation-baseline.md)
# 2.1 — Linux Installation & Baseline

> **Lab Area:** Linux Administration  
> **Module:** 2.1 — Installation & Baseline  
> **Environment:** Virtualized Enterprise IT Support Lab  
> **Platform:** Linux / VirtualBox  
> **Primary Tools:** Bash, `hostnamectl`, `uname`, `ip`, `df`, `lsblk`, `systemctl`

---

## 1. Module Overview

This module establishes the initial baseline for a Linux workstation in the simulated enterprise infrastructure environment.

The purpose of the baseline is to document the system's initial configuration before performing administration, networking, troubleshooting, security, and monitoring activities.

The baseline covers:

- Hostname
- Operating system
- Kernel
- System architecture
- CPU and memory
- Disk and filesystem configuration
- Network interfaces
- IPv4 addressing
- Routing
- Default gateway
- Network connectivity
- Running services
- System uptime

This information provides a reference point for later troubleshooting and configuration changes.

---

## 2. Objectives

The objectives of this module were to:

- Verify the Linux installation.
- Identify the hostname.
- Identify the operating system.
- Identify the Linux kernel version.
- Verify system architecture.
- Review CPU and memory resources.
- Inspect disks and partitions.
- Review filesystem usage.
- Identify network interfaces.
- Record IPv4 addresses.
- Review routing configuration.
- Identify the default gateway.
- Check basic connectivity.
- Review system services.
- Establish a documented baseline.

---

## 3. Linux System Identity

The Linux workstation was identified as:

```text
Hostname:
LINUX-CLIENT01
```

The hostname is important for:

- Endpoint identification
- Remote administration
- Troubleshooting
- Log correlation
- Monitoring
- Incident documentation

### Command

```bash
hostname
```

or:

```bash
hostnamectl
```

---

### 📸 Screenshot — Hostname

<img src="screenshots/01-hostname.png"
     width="700"
     alt="Linux terminal showing hostname">

**Evidence:** Terminal output showing the Linux workstation hostname.

---

## 4. Operating System Information

The installed Linux distribution was checked using:

```bash
cat /etc/os-release
```

Additional system information can be collected using:

```bash
hostnamectl
```

These commands provide information such as:

- Operating system
- Distribution
- Version
- Architecture
- Kernel

The operating-system information establishes the software baseline for future administration and troubleshooting.

---

### 📸 Screenshot — Linux OS Information

<img src="screenshots/02-os-information.png"
     width="700"
     alt="Linux terminal showing operating system information">

**Evidence:** `/etc/os-release` and/or `hostnamectl` output showing the Linux installation details.

---

## 5. Kernel and Architecture

The Linux kernel and architecture were checked using:

```bash
uname -a
```

A more focused kernel query can be performed using:

```bash
uname -r
```

Architecture:

```bash
uname -m
```

The results establish the kernel version and processor architecture being used by the system.

This information is useful when troubleshooting:

- Driver compatibility
- Package compatibility
- Kernel-related problems
- Application requirements
- System updates

---

### 📸 Screenshot — Kernel Information

<img src="screenshots/03-kernel-architecture.png"
     width="700"
     alt="Linux terminal showing kernel and architecture">

**Evidence:** Terminal output showing the Linux kernel and system architecture.

---

## 6. CPU and Memory Baseline

Hardware resources were inspected to establish the initial system-performance baseline.

### CPU

```bash
lscpu
```

### Memory

```bash
free -h
```

The information can be used later when investigating:

- High CPU usage
- Memory exhaustion
- System slowdown
- Application performance
- Resource contention

---

### 📸 Screenshot — CPU and Memory

<img src="screenshots/04-cpu-memory.png"
     width="700"
     alt="Linux terminal showing CPU and memory information">

**Evidence:** Linux CPU and memory information.

---

## 7. Disk and Partition Inventory

Available disks and partitions were inspected using:

```bash
lsblk
```

This command provides an overview of:

- Physical disks
- Partitions
- Mount points
- Filesystem relationships

The information establishes which storage devices are attached to the Linux workstation.

---

### 📸 Screenshot — Disk and Partition Inventory

<img src="screenshots/05-disk-partitions.png"
     width="700"
     alt="Linux terminal showing disks and partitions">

**Evidence:** `lsblk` output showing the Linux storage configuration.

---

## 8. Filesystem Usage

Filesystem utilization was checked using:

```bash
df -h
```

This provides:

- Filesystem
- Total size
- Used space
- Available space
- Usage percentage
- Mount point

Monitoring filesystem usage is important because a nearly full filesystem can cause:

- Application failures
- Logging failures
- Package installation problems
- Database problems
- System instability

---

### 📸 Screenshot — Filesystem Usage

<img src="screenshots/06-filesystem-usage.png"
     width="700"
     alt="Linux terminal showing filesystem usage">

**Evidence:** `df -h` output showing filesystem capacity and utilization.

---

## 9. Network Interface Baseline

Network interfaces were inspected using:

```bash
ip -br addr
```

The Linux workstation was identified as:

```text
LINUX-CLIENT01
```

The captured network configuration included:

```text
eth0    UP    10.0.2.15/24
eth1    UP    192.168.56.105/24
```

Additional virtual interfaces included Docker-related interfaces.

The primary interface used for external connectivity was:

```text
eth0
```

with:

```text
10.0.2.15/24
```

---

### 📸 Screenshot — Network Interface Baseline

<img src="screenshots/07-network-interfaces.png"
     width="700"
     alt="Linux terminal showing network interfaces and IP addresses">

**Evidence:** `ip -br addr` output showing Linux network interfaces and IPv4 addresses.

---

## 10. Routing Baseline

The routing table was inspected using:

```bash
ip route
```

The captured configuration included:

```text
default via 10.0.2.2 dev eth0
```

The directly connected network was:

```text
10.0.2.0/24
```

The Linux system also had a host-only network:

```text
192.168.56.0/24
```

The default route determines where traffic destined for networks outside the directly connected networks is sent.

---

### 📸 Screenshot — Routing Table

<img src="screenshots/08-routing-table.png"
     width="700"
     alt="Linux terminal showing IP routing table">

**Evidence:** `ip route` output showing connected routes and the default gateway.

---

## 11. Route Selection Validation

The route selected for an external destination was checked using:

```bash
ip route get 8.8.8.8
```

The captured result showed traffic being routed:

```text
via 10.0.2.2
dev eth0
src 10.0.2.15
```

This confirms that Linux selected `eth0` and the `10.0.2.2` gateway for the test destination.

This command is particularly useful when troubleshooting systems with multiple network interfaces.

---

### 📸 Screenshot — Route Selection

<img src="screenshots/09-route-selection.png"
     width="700"
     alt="Linux terminal showing route selection for external destination">

**Evidence:** `ip route get 8.8.8.8` output showing the selected route.

---

## 12. Network Connectivity Test

Basic connectivity was tested using:

```bash
ping -c 4 8.8.8.8
```

A successful response indicates IP-level connectivity to the destination.

Connectivity testing helps separate different layers of a network problem:

```text
Interface
   ↓
IP configuration
   ↓
Gateway
   ↓
Routing
   ↓
External connectivity
```

---

### 📸 Screenshot — Connectivity Test

<img src="screenshots/10-connectivity-test.png"
     width="700"
     alt="Linux terminal showing network connectivity test">

**Evidence:** Linux connectivity test output.

---

## 13. DNS Baseline

DNS configuration can be inspected using:

```bash
resolvectl status
```

or:

```bash
cat /etc/resolv.conf
```

DNS testing can be performed using:

```bash
getent hosts google.com
```

or:

```bash
nslookup google.com
```

The purpose is to establish whether the Linux workstation can resolve domain names.

DNS will be investigated in greater detail during the dedicated DNS phase of the lab.

---

### 📸 Screenshot — DNS Baseline

<img src="screenshots/11-dns-baseline.png"
     width="700"
     alt="Linux terminal showing DNS configuration and resolution">

**Evidence:** DNS configuration and resolution test.

---

## 14. Running Services

The initial system service state can be reviewed using:

```bash
systemctl --type=service --state=running
```

Individual services can be inspected using:

```bash
systemctl status <service-name>
```

This provides a baseline for determining which services are running before later configuration or troubleshooting activities.

Service management will be expanded in subsequent Linux Administration modules.

---

### 📸 Screenshot — Running Services

<img src="screenshots/12-running-services.png"
     width="700"
     alt="Linux terminal showing running services">

**Evidence:** `systemctl` output showing running Linux services.

---

## 15. System Uptime and Baseline Health

System uptime was checked using:

```bash
uptime
```

Additional information can be collected using:

```bash
uptime -p
```

Uptime provides useful context when investigating:

- Recent reboots
- Service interruptions
- Unexpected shutdowns
- Performance issues
- Patch/reboot requirements

The uptime value was recorded as part of the initial system baseline.

---

### 📸 Screenshot — System Uptime

<img src="screenshots/13-system-uptime.png"
     width="700"
     alt="Linux terminal showing system uptime">

**Evidence:** Linux uptime output.

---

## 16. Baseline Summary

The Linux workstation baseline can be summarized as:

```text
Hostname:
LINUX-CLIENT01

Primary Interface:
eth0

Primary IPv4:
10.0.2.15/24

Host-Only Interface:
eth1

Host-Only IPv4:
192.168.56.105/24

Default Gateway:
10.0.2.2

Primary Network:
10.0.2.0/24

Host-Only Network:
192.168.56.0/24
```

The system also contained virtual networking interfaces associated with Docker.

---

## 17. Troubleshooting Relevance

The baseline provides a reference for future Linux support incidents.

For example, if the workstation later loses Internet connectivity, the administrator can compare:

```text
Current IP
      ↓
Baseline IP
      ↓
Current interface state
      ↓
Baseline interface state
      ↓
Current routing table
      ↓
Baseline routing table
      ↓
Gateway connectivity
      ↓
DNS resolution
```

This helps determine whether the problem is related to:

- Interface configuration
- IP addressing
- Routing
- Gateway connectivity
- DNS
- External network access

---

## 18. Key Commands

```bash
# System identity
hostname
hostnamectl

# Operating system
cat /etc/os-release

# Kernel and architecture
uname -a
uname -r
uname -m

# CPU
lscpu

# Memory
free -h

# Disks and partitions
lsblk

# Filesystem usage
df -h

# Network interfaces
ip -br addr

# Routing
ip route

# Route selection
ip route get 8.8.8.8

# Connectivity
ping -c 4 8.8.8.8

# DNS
resolvectl status
getent hosts google.com

# Running services
systemctl --type=service --state=running

# System uptime
uptime
```

---
