
# Help Desk Ticket 002 — Slow Computer

## Ticket Information

**Technician:** Cameron Carter  
**Date:** August 8, 2026  
**Priority:** Medium  
**Category:** System Performance  
**Status:** Resolved  

---

## Reported Issue

The user reported that their Windows computer had become unusually slow. Applications were taking longer to open, switching between programs caused noticeable lag, and overall system responsiveness had degraded.

---

## Initial Questions

1. When did the computer begin running slowly?
2. Does the slowdown occur constantly or only when certain applications are open?
3. Has any new software been installed recently?
4. Does the computer become slow immediately after startup?
5. Have there been any recent Windows updates?
6. Is there sufficient free storage space?

---

## Possible Causes

Potential causes considered during troubleshooting included:

- High CPU utilization
- High memory utilization
- Excessive background applications
- Excessive startup applications
- Insufficient disk space
- Failing storage hardware
- Corrupted Windows system files
- Extended system uptime

---

# Troubleshooting Procedure

## Step 1 — Verify System Information and Uptime

### Command

```powershell
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Boot Time"