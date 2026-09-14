# 01 — Windows workstation baseline

**Status:** Ready to run  
**Type:** Hands-on, read-only  
**Goal:** Distinguish evidence from guesses about a slow computer.

## Scenario

A fictional user reports that their workstation is slow after signing in. Collect a baseline; do not assume that an idle-machine snapshot reproduces their problem.

## Steps

1. Open Start, type **Windows PowerShell**, and open it normally.
2. Run:
   ```powershell
   Get-Date
   Get-CimInstance Win32_OperatingSystem | Select-Object Caption,Version,LastBootUpTime
   Get-CimInstance Win32_OperatingSystem | Select-Object TotalVisibleMemorySize,FreePhysicalMemory
   Get-PSDrive -PSProvider FileSystem
   Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 ProcessName,CPU,WorkingSet64
   ```
3. Memory figures from Win32_OperatingSystem are in KiB; WorkingSet64 is bytes. Process CPU is cumulative CPU time, not current utilisation.
4. Press Ctrl+Shift+Esc. In Task Manager, observe CPU, memory and disk activity while opening an ordinary application. Record the time and whether the symptom occurred.
5. Run:
   ```powershell
   Get-Service | Where-Object Status -eq 'Stopped' | Select-Object -First 15 Name,Status
   ```
   A stopped service is not automatically faulty. Many start only when needed.
6. Open Event Viewer from Start. Select **Windows Logs > Application**. Look around the recorded time for relevant warnings or errors. Do not clear logs. Record only sanitised event IDs and descriptions.
7. Compare observed utilisation, available disk space and event timing before naming a cause.

## Deliverable

A baseline table and a short explanation of what is supported, what remains unknown and what you would ask the user next.

**Pass condition:** correctly distinguish cumulative CPU from live usage, interpret units, and avoid presenting an unobserved fault as diagnosed.

**Rollback:** none; these checks do not change configuration.
