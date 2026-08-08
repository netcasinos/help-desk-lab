# Ticket 002 — Slow Computer

## Ticket Information

**Technician:** Cameron Carter  
**Date:** August 8, 2026  
**Priority:** Medium  
**Status:** In Progress  
**Category:** System Performance  

## Reported Issue

The user reports that their Windows computer has become unusually slow.
Applications take longer to open, switching between programs causes lag,
and overall system performance has degraded.

## Questions Asked

1. When did the computer begin running slowly?
2. Does the slowdown occur constantly or only when certain applications are open?
3. Has any new software been installed recently?
4. Does the computer become slow immediately after startup?
5. Have there been any recent Windows updates?
6. Is there enough available storage space?

## Possible Causes

- High CPU utilization
- High memory usage
- Insufficient disk space
- Excessive startup applications
- Failing storage device
- Corrupted Windows system files
- Background applications or services
- Pending updates or restart
- Malware or unwanted software

## Troubleshooting Procedure

The reported issue was degraded Windows system performance. I reviewed the
computer's system resources, running processes, storage, startup configuration,
and operating-system integrity to identify potential performance bottlenecks.

### Step 1 — Verify System Information and Uptime

**Command used:**

`systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Boot Time"`

**Purpose:**

Reviewed the operating system version and system boot time to establish basic system information and determine whether an extended uptime could be contributing to degraded performance.

**Evidence:**

![[Ticket002-SystemInfo.png]]

**Finding:**


The system is running Windows 11 Home, build 26200. The computer last booted on August 6, 2026 at 9:24 PM, resulting in approximately 1 day and 20 hours of uptime at the time of testing. This is not an unusually long uptime period, so extended system uptime was unlikely to be the primary cause of the reported performance degradation.


### Step 2 — Review Resource-Intensive Processes

**Command used:**

`Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, CPU, @{Name="MemoryMB";Expression={[math]::Round($_.WorkingSet64 / 1MB,2)}}`

**Purpose:**

Reviewed running processes to identify applications that may be consuming excessive CPU time or system memory and contributing to degraded performance.

**Evidence:**

![[Ticket002-TopProcesses.png|900]]



**Finding:**

Several resource-intensive applications were active during testing. The process list showed multiple Wallpaper Engine processes with significant accumulated CPU time, while Discord was using approximately 867 MB of memory. Additional active applications included Visual Studio Code, Brave Browser, Riot Client, OneDrive, and Task Manager.

The CPU values displayed by `Get-Process` represent accumulated processor time rather than current CPU utilization; however, the results still indicate that several background and user applications had been consuming system resources over time.

The number of simultaneously running applications suggested that background software could be contributing to the reported slowdown, particularly when combined with the limited amount of available system memory identified during later testing.

### Step 3 — Check Available System Memory

**Command used:**

`Get-CimInstance Win32_OperatingSystem`

**Purpose:**

Checked total and available physical memory to determine whether insufficient RAM was contributing to slow system performance.

**Evidence:**
![[Ticket002-MemoryUsage.png]]


**Finding:**


The system contained approximately 15.76 GB of usable physical memory, with only 2.15 GB available at the time of testing. This means approximately 13.61 GB, or roughly 86% of the system's physical memory, was currently in use.

The low amount of available memory indicated significant memory pressure and was a likely contributor to the reported slow performance. With multiple applications and background processes running simultaneously, Windows may need to rely more heavily on memory management and paging, which can reduce overall system responsiveness.

### Step 4 — Verify Available Disk Space

**Command used:**

`Get-PSDrive C`

**Purpose:**

Checked available storage on the Windows system drive. Low free disk space can negatively affect virtual memory, Windows updates, temporary file operations, and overall system performance.

**Evidence:**

![[Ticket002-DiskSpace.png]]

**Finding:**



The Windows system drive contained approximately 156.40 GB of free storage space out of approximately 930.52 GB total capacity.

Although the drive is relatively full, approximately 16.8% of the disk remained available. This amount of free space was sufficient for normal Windows operations, temporary files, virtual memory, and updates. Low disk space was therefore not identified as the primary cause of the reported slowdown.

Storage usage should still be monitored because continued reduction in available disk space could eventually affect system performance.


### Step 5 — Verify Storage Device Health

**Command used:**

`Get-PhysicalDisk | Select-Object FriendlyName, MediaType, HealthStatus, OperationalStatus`

**Purpose:**

Reviewed the physical storage device health and operational status to determine whether a failing or degraded disk could be responsible for slow system performance.

**Evidence:**



**Finding:**

![[Ticket002-DiskHealth.png]]



The installed WD Green SN3000 1TB solid-state drive reported a HealthStatus of `Healthy` and an OperationalStatus of `OK`.

No evidence of storage-device failure or degradation was detected. Because the SSD was operating normally, a failing storage device was ruled out as a likely cause of the reported system slowdown.


### Step 6 — Review Startup Applications

**Command used:**

`Get-CimInstance Win32_StartupCommand | Select-Object Name, Location`

**Purpose:**

Reviewed applications configured to launch during Windows startup. Excessive startup applications can increase boot time and consume system resources in the background.

**Evidence:**

![[Ticket002-StartupPrograms.png]]



**Finding:**

The startup configuration contained a large number of applications configured to launch automatically with Windows. These included OneDrive, Steam, Spotify, Riot Client, Discord, Wallpaper Engine, Medal, Slack, EA software, Proton VPN, Proton Drive, Microsoft Lists, Roblox, Firefox, Microsoft Copilot, and Microsoft Edge auto-launch components.

The number of automatically launching applications could contribute to longer startup times and increased background CPU and memory consumption. This finding was particularly significant because the system also had only 2.15 GB of available RAM during testing.

Unnecessary startup applications were identified as a likely contributor to the user's performance complaint.


### Step 7 — Verify Windows System File Integrity

**Command used:**

`sfc /scannow`

**Purpose:**

Ran the Windows System File Checker to identify missing, corrupted, or modified protected operating-system files that could contribute to system instability or degraded performance.

**Evidence:**

![[Ticket002-SFCScan.png]]

**Finding:**



System File Checker completed successfully at 100%. Windows Resource Protection reported that no integrity violations were detected.

This confirmed that protected Windows system files were intact and that operating-system file corruption was unlikely to be responsible for the reported performance issue.

## Root Cause

The investigation did not identify hardware failure, insufficient disk space, or corrupted Windows system files.

The primary performance concern was high memory utilization. Approximately 86% of the system's 15.76 GB of usable RAM was in use during testing, leaving only 2.15 GB available.

The system was also configured to automatically launch numerous applications, including gaming clients, communication applications, cloud synchronization services, browsers, VPN software, and Wallpaper Engine. Several of these applications remained active in the background and consumed additional system resources.

The most likely cause of the reported slowdown was therefore excessive background application activity combined with high physical memory utilization.

## Resolution

The startup configuration was reviewed and unnecessary third-party applications were identified for removal from automatic startup.

Nonessential applications were disabled from launching automatically with Windows to reduce background CPU and memory consumption. Applications that provide security or required synchronization functionality were left enabled.

The system was then restarted to clear existing memory usage and apply the revised startup configuration.

## Verification

Following the startup configuration changes and system restart, system responsiveness was re-evaluated and available physical memory was checked again.

The system successfully booted and remained operational without errors. Resource utilization was reviewed to confirm that fewer unnecessary applications were running automatically in the background.

The user was able to resume normal system operation, and no hardware or Windows system-file integrity issues were detected.