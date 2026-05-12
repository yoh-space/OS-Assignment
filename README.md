# Operating Systems Assignment: Parrot OS Setup & System Call Demo

**Student:** Yohannes Damtie Aleka  
**Hostname:** parrot-yohannes  
**OS:** Parrot OS 6.4 (Security Edition)

---

## Table of Contents

1. [Parrot OS Installation](#1-parrot-os-installation)
2. [Problems & Solutions](#2-problems--solutions)
3. [Filesystem Support](#3-filesystem-support)
4. [Parrot OS Advantages & Disadvantages](#4-parrot-os-advantages--disadvantages)
5. [Virtualization Overview](#5-virtualization-overview)
6. [OS Verification Proof](#6-os-verification-proof)
7. [System Call: sysinfo()](#7-system-call-sysinfo)
8. [References](#8-references)

---

## 1. Parrot OS Installation

**Hardware used:** Multi-core CPU, 8GB RAM, 64GB storage.  
**Steps:**

1. Downloaded Parrot OS Security Edition ISO from [parrotsec.org](https://parrotsec.org).
2. Created bootable USB using **Etcher**.
3. Booted from USB (BIOS → USB priority).
4. Launched installer, selected language/time zone/keyboard.
5. **Manual partitioning** (preserved existing data).
6. Set username `Yohannes Damtie Aleka`, hostname `parrot-yohannes`.
7. Installed GRUB and rebooted.

---

## 2. Problems & Solutions

| Problem | Solution |
|---------|----------|
| Slow `apt update` due to distant mirrors | Used Parrot Mirror Selector to switch to faster local mirrors |

---

## 3. Filesystem Support

Parrot OS (Debian-based) supports many filesystems. **Default is ext4** due to:

- Maturity and stability
- Good performance
- Journaling for crash resilience
- Universal Linux support

Other supported filesystems: NTFS, FAT32, exFAT, Btrfs, ZFS (via modules).

---

## 4. Parrot OS Advantages & Disadvantages

| Advantages | Disadvantages |
|------------|----------------|
| Security‑hardened kernel | Steep learning curve |
| 600+ pre‑installed security tools | Not optimized for gaming/multimedia |
| Built‑in anonymity (Anonsurf) | Frequent, large updates |
| Development‑ready (compilers, IDEs) | Tools can be misused |
| Lightweight MATE desktop | |

---

## 5. Virtualization Overview

- **Virtualization** – Running multiple isolated OS instances on one physical machine.
- **Hypervisor** – Software layer managing VMs (Type 1: bare‑metal, Type 2: hosted).
- **Benefits** – Resource optimization, isolation, portability, cost efficiency.
- **Security** – VMs are sandboxed; a crash/breach in one does not affect others.
- **Examples** – VMware ESXi (Type 1), VirtualBox (Type 2).

---

## 6. OS Verification Proof

Run these commands to confirm Parrot OS:

```bash
cat /etc/os-release
uname -a
lsb_release -a
Sample output (verified on my system):

text
NAME="Parrot OS"
VERSION="6.4 (Lorikeet)"
Linux parrot-yohannes 6.5.0-1parrot1-amd64 ... x86_64
Distributor ID: Parrot
Description: Parrot OS 6.4 (Lorikeet)
7. System Call: sysinfo()
The sysinfo() system call returns system resource statistics (uptime, RAM, swap, load averages).

Code (sysinfo_demo.c)
c
#include <stdio.h>
#include <sys/sysinfo.h>

int main() {
    struct sysinfo info;
    if (sysinfo(&info) != 0) {
        perror("sysinfo");
        return 1;
    }
    printf("Uptime        : %ld seconds\n", info.uptime);
    printf("Total RAM     : %ld MB\n", info.totalram / (1024*1024));
    printf("Free RAM      : %ld MB\n", info.freeram / (1024*1024));
    printf("Total swap    : %ld MB\n", info.totalswap / (1024*1024));
    printf("Free swap     : %ld MB\n", info.freeswap / (1024*1024));
    printf("Processes     : %u\n", info.procs);
    printf("Load averages : %lu, %lu, %lu\n",
           info.loads[0], info.loads[1], info.loads[2]);
    return 0;
}
Compilation & Execution
bash
gcc -o sysinfo_demo sysinfo_demo.c
./sysinfo_demo
Sample Output
text
Uptime        : 12345 seconds
Total RAM     : 7876 MB
Free RAM      : 2048 MB
Total swap    : 2048 MB
Free swap     : 2048 MB
Processes     : 312
Load averages : 65536, 32768, 16384
Load averages are scaled by 65536; divide by 65536.0 for actual values.
```

## 8. References
Oracle. What Is Virtualization? – Link

NIST. Guide to Security for Full Virtualization Technologies – SP 800-125

IBM. What Are Hypervisors? – Link

AWS. Type 1 vs Type 2 Hypervisors – Link
