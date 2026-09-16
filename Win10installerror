# HP ZBook 15 G4 — SSD Upgrade & Clean Windows Install

A quick write-up of upgrading the storage on an HP ZBook 15 G4, including two issues I hit along the way and how I resolved them.

## Overview

- **Machine:** HP ZBook 15 G4
- **Task:** Install a new NVMe SSD, back up important files, and perform a clean Windows install
- **Result:** New SSD detected, benchmarked, and Windows installed successfully after resolving two separate issues

## Step 1: New SSD Not Detected

After physically installing the new SSD, it wasn't showing up in Windows.

**Diagnosis checklist:**
1. Checked BIOS/UEFI (Esc → F10) to confirm the drive was detected at the firmware level
2. Confirmed the ZBook 15 G4 supports both a 2.5" SATA bay and an M.2 slot — worth checking which one your drive/slot combo actually supports (M.2 slots on some G4 configs are SATA-only, not NVMe)

**Result:** BIOS saw the drive fine, which pointed to Windows simply not having initialized it yet — normal behavior for a brand-new blank drive.

**Fix:**
1. `Win + X` → **Disk Management**
2. New SSD appeared as **"Not Initialized"**
3. Right-click → **Initialize Disk** → chose **GPT**
4. Right-click the unallocated space → **New Simple Volume** → assigned drive letter, formatted NTFS

Drive showed up in File Explorer immediately after.

## Step 2: Benchmarking

Ran CrystalDiskMark to confirm the drive was performing as expected:

| Test | Read (MB/s) | Write (MB/s) |
|---|---|---|
| SEQ1M Q8T1 | 3173.01 | 1352.46 |
| SEQ1M Q1T1 | 1657.54 | 1190.00 |
| RND4K Q32T1 | 263.82 | 217.53 |
| RND4K Q1T1 | 41.85 | 102.58 |

Sequential speeds well above SATA's ~550 MB/s ceiling confirmed this was a properly-recognized NVMe drive performing normally.

## Step 3: Clean Windows Install — Partition Error

Attempting a fresh Windows install on the new SSD threw:

> *"We couldn't create a new partition or locate an existing one. For more information, see the Setup log files."*

**Cause:** Leftover partition structure from the manual GPT initialization in Disk Management conflicted with what Windows Setup expected.

**Fix:**
1. In the Setup partition screen, deleted both existing partitions (the MSR reserved partition and the primary volume) to leave only unallocated space
2. Selected the unallocated space and clicked **Next** directly — let Windows Setup create its own partition layout (EFI, MSR, recovery, primary) automatically rather than pre-partitioning manually

## Step 4: Install Failure — Corrupted USB Media

Setup then failed partway through with:

> *"Windows cannot install required files... Error code: 0x80070022"*

**Cause:** Turned out to be a bad USB flash drive, not a disk or partition issue.

**Fix:** Re-flashed the Windows installer onto a different USB drive. Install completed with no further errors.

## Step 5: HP Hotkey Support — Brightness/Volume Keys Not Working

After the OS install, hardware function keys (brightness, volume, etc.) weren't responding. Installing the HP driver pack's **Hotkey Support** driver threw:

> *"HP Software Framework is not installed or not running. Please download and install HP Software Framework..."*

**Cause:** The Hotkey Support driver depends on HP Software Framework (HP CASL) being installed first — it wasn't present on the fresh install.

**Fix:**
1. Downloaded and installed **HP Software Framework** from HP's support site
2. Restarted the system as prompted
3. Reinstalled the **Hotkey Support / Driver-Keyboard, Mouse and Input Devices** driver

Function keys worked normally afterward.

## Key Takeaways

- **BIOS detects, OS doesn't →** check Disk Management for an uninitialized disk before assuming a hardware fault
- **CrystalDiskMark sequential reads well above 550 MB/s** confirm you're actually running NVMe, not SATA
- **"Couldn't create a new partition" during Setup** is often caused by leftover partition structure — wipe to unallocated space and let Setup handle partitioning itself
- **Error 0x80070022 during file copy** is frequently bad installation media (USB drive or corrupted ISO), not a disk problem — try a different USB stick before troubleshooting further
- **HP hotkeys (brightness/volume) not working after a fresh install** usually means HP Software Framework needs to be installed before the Hotkey Support driver will function

---
*Part of the Retro Lab hardware/troubleshooting notes.*
