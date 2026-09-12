# 1.5 — Storage & Disk-Space Troubleshooting

> **Lab Area:** Windows Administration  
> **Module:** 1.5 — Storage & Disk-Space Troubleshooting  
> **Environment:** Virtualized Enterprise IT Support Lab  
> **Platform:** Microsoft Windows  
> **Primary Tools:** PowerShell, Disk Management, File System Utilities

---

## 1. Module Overview

Storage and disk-space problems are common IT support incidents.

Insufficient disk space or disk-related issues can cause:

- Applications to fail
- Windows Updates to fail
- Temporary files to accumulate
- System performance to decrease
- Software installations to fail
- User profiles to consume excessive storage
- File operations to fail

In this module, the Windows storage configuration was inspected, available disk space was measured, disk health was checked, large directories and files were investigated, and a controlled test file was created.

---

## 2. Objectives

The objectives of this module were to:

- Identify available Windows volumes.
- Check filesystem types.
- Measure disk capacity and free space.
- Calculate available disk-space percentage.
- Inspect physical disk health.
- Check filesystem dirty status.
- Identify large files and directories.
- Investigate temporary-file usage.
- Check for the presence of `Windows.old`.
- Perform a controlled storage test.
- Document findings and evidence.

---

## 3. Check Windows Volumes

The first step was to identify the available storage volumes.

### Command

```powershell
Get-Volume |
Select-Object DriveLetter, FileSystem, Size, SizeRemaining
```

The primary system volume was:

```text
Drive Letter: C
File System: NTFS
```

The captured C: volume had approximately:

```text
Total Size:       84.8 GB
Remaining Space:  41.2 GB
```

This provided the initial storage baseline.

---

### 📸 Screenshot — Volume Inventory

<img src="screenshots/01-volume-inventory.png"
     width="700"
     alt="PowerShell showing Windows volume information">

**Evidence:** PowerShell showing available volumes, filesystem type, total capacity, and remaining space.

---

## 4. Calculate Free-Space Percentage

The percentage of remaining space was calculated using PowerShell.

```powershell
$disk = Get-Volume -DriveLetter C

[math]::Round(
    ($disk.SizeRemaining / $disk.Size) * 100,
    2
)
```

The captured result was:

```text
48.25
```

Therefore, approximately **48.25% of the C: volume was available** during the storage check.

---

### 📸 Screenshot — Free-Space Calculation

<img src="screenshots/02-free-space-calculation.png"
     width="700"
     alt="PowerShell calculating available disk space percentage">

**Evidence:** PowerShell calculation showing the percentage of remaining storage.

---

## 5. Inspect Physical Disk

The physical disk configuration was inspected using PowerShell.

### Commands

```powershell
Get-Disk
```

and:

```powershell
Get-PhysicalDisk
```

The laboratory system used a virtual disk provided by the virtualization environment.

The disk was reported as online and healthy.

---

### 📸 Screenshot — Physical Disk Health

<img src="screenshots/03-physical-disk-health.png"
     width="700"
     alt="PowerShell showing physical disk health">

**Evidence:** PowerShell showing the physical disk state and health information.

---

## 6. Check Filesystem Dirty Status

The NTFS volume was checked to determine whether Windows had marked the filesystem as dirty.

### Command

```powershell
fsutil dirty query C:
```

The captured result indicated:

```text
Volume C: is NOT Dirty
```

This provided additional evidence that the filesystem was not currently marked as requiring a dirty-volume repair state.

---

### 📸 Screenshot — NTFS Dirty Check

<img src="screenshots/04-dirty-bit-check.png"
     width="700"
     alt="PowerShell showing C drive dirty status">

**Evidence:** `fsutil dirty query C:` output showing that the C: volume was not dirty.

---

## 7. Storage Reliability Information

Storage reliability information was also inspected.

Example command:

```powershell
Get-PhysicalDisk |
Get-StorageReliabilityCounter
```

This type of information can help identify indicators such as:

```text
Temperature
Wear
Read errors
Write errors
Power-on hours
```

The laboratory environment is virtualized, so hardware-level reliability information may not expose meaningful physical-drive telemetry.

The important point is to understand the command and recognize the limitations of storage monitoring inside a virtual machine.

---

## 8. Search for Large Directories

Large directories were investigated to identify potential sources of disk consumption.

Example:

```powershell
Get-ChildItem C:\Users -Directory |
ForEach-Object {
    $size = (
        Get-ChildItem $_.FullName -Recurse -File -ErrorAction SilentlyContinue |
        Measure-Object Length -Sum
    ).Sum

    [PSCustomObject]@{
        Folder = $_.FullName
        SizeGB = [math]::Round($size / 1GB, 2)
    }
} |
Sort-Object SizeGB -Descending
```

This type of analysis helps identify directories that may require cleanup or further investigation.

---

### 📸 Screenshot — Large Directory Investigation

<img src="screenshots/05-large-directories.png"
     width="700"
     alt="PowerShell showing large directories">

**Evidence:** PowerShell output used to identify directories consuming significant storage.

---

## 9. Temporary File Investigation

Temporary directories were examined because temporary data can accumulate over time.

The investigation included:

```text
User temporary files
Windows temporary files
Other large temporary locations
```

The captured investigation found approximately:

```text
User Temp:
~349.85 MB

Windows Temp:
~52.5 MB
```

These values were recorded as part of the storage investigation.

---

### 📸 Screenshot — Temporary File Usage

<img src="screenshots/06-temp-directory-analysis.png"
     width="700"
     alt="PowerShell showing temporary directory sizes">

**Evidence:** PowerShell output showing temporary-directory storage usage.

---

## 10. Windows.old Investigation

The system was also checked for:

```text
C:\Windows.old
```

The presence of this directory can indicate that Windows retained files from a previous installation or upgrade.

This directory can consume significant storage and should be investigated before deciding whether it can safely be removed.

---

### 📸 Screenshot — Windows.old

<img src="screenshots/07-windows-old.png"
     width="700"
     alt="PowerShell showing Windows.old directory">

**Evidence:** PowerShell showing the presence and/or size of `C:\Windows.old`.

---

## 11. Large File Investigation

Large individual files were searched to identify potential storage consumers.

Example:

```powershell
Get-ChildItem C:\ -File -Recurse -ErrorAction SilentlyContinue |
Sort-Object Length -Descending |
Select-Object -First 20 FullName,
@{Name="SizeGB";Expression={
    [math]::Round($_.Length / 1GB, 2)
}}
```

Large files should be investigated before deletion.

An administrator should determine:

```text
What is the file?
Who owns it?
Is it required?
Is it temporary?
Is it a system file?
Can it be safely archived or removed?
```

---

### 📸 Screenshot — Large Files

<img src="screenshots/08-large-files.png"
     width="700"
     alt="PowerShell showing large files">

**Evidence:** PowerShell output showing large files identified during storage investigation.

---

## 12. Storage Troubleshooting Scenario

### Incident

A workstation reports reduced available storage and possible performance problems.

### Investigation

```text
Check volume capacity
        ↓
Calculate free space
        ↓
Check physical disk health
        ↓
Check filesystem state
        ↓
Find large directories
        ↓
Find large files
        ↓
Investigate temporary data
        ↓
Check Windows.old
        ↓
Perform controlled storage test
        ↓
Document findings
```

The investigation did not rely on deleting files immediately.

Instead, storage usage was first measured and analyzed.

---

## 13. Controlled Storage Test

A dedicated test directory was created:

```text
C:\IT-Lab-DiskTest
```

A controlled test file was also created to demonstrate disk consumption and storage monitoring.

Example:

```powershell
New-Item -ItemType Directory -Path "C:\IT-Lab-DiskTest" -Force
```

The test environment was intentionally separated from important Windows directories.

This prevents accidental modification of operating-system files during the exercise.

---

### 📸 Screenshot — Storage Test Directory

<img src="screenshots/09-storage-test.png"
     width="700"
     alt="PowerShell showing IT lab storage test directory">

**Evidence:** PowerShell showing creation and use of the controlled storage-test directory.

---

## 14. Verify Storage After Test

After creating the test data, disk space was checked again.

```powershell
Get-Volume -DriveLetter C |
Select-Object DriveLetter, Size, SizeRemaining
```

The purpose was to confirm that storage consumption could be measured before and after a controlled change.

This is a useful IT support technique when validating disk-space alerts or storage monitoring.

---

### 📸 Screenshot — Post-Test Storage Check

<img src="screenshots/10-post-test-storage.png"
     width="700"
     alt="PowerShell showing disk space after storage test">

**Evidence:** PowerShell showing storage capacity after the controlled test.

---

## 15. Troubleshooting Findings

The storage investigation produced the following findings:

| Check | Result |
|---|---|
| Primary volume | C: |
| Filesystem | NTFS |
| Approx. total capacity | 84.8 GB |
| Approx. remaining space | 41.2 GB |
| Free-space percentage | 48.25% |
| Physical disk state | Healthy / Online |
| NTFS dirty status | Not Dirty |
| User Temp usage | ~349.85 MB |
| Windows Temp usage | ~52.5 MB |
| `Windows.old` | Present |
| Large-file investigation | Performed |
| Controlled storage test | Performed |

The available evidence did not indicate an immediately critical disk-space condition.

The investigation instead demonstrated how an administrator can systematically determine where storage is being consumed.

---
Infrastructure Operations
Troubleshooting
```
