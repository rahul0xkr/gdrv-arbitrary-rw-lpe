# Windows Kernel LPE via Vulnerable Driver (gdrv.sys)

## 📌 Overview

This project demonstrates a Windows kernel privilege escalation (LPE) exploit using a vulnerable OEM driver (`gdrv.sys`).

The exploit achieves **SYSTEM privileges** by leveraging an arbitrary read/write primitive exposed by the driver and performing token stealing.

---

## ⚙️ Vulnerability

The driver exposes an IOCTL that allows arbitrary memory copy operations:

- No validation of source/destination pointers
- Leads to **arbitrary kernel read/write**

This is a classic example of a **BYOVD (Bring Your Own Vulnerable Driver)** scenario.

---

## 🔥 Exploit Strategy

1. Obtain arbitrary read/write primitive via IOCTL
2. Resolve `PsInitialSystemProcess` dynamically
3. Leak SYSTEM `_EPROCESS`
4. Dynamically discover structure offsets:
   - Locate `UniqueProcessId` via SYSTEM PID (4)
   - Infer `ActiveProcessLinks`
   - Identify `Token` via pointer heuristics
5. Traverse process list to find current process
6. Replace current process token with SYSTEM token
7. Spawn SYSTEM shell

---

## 💣 Result

```text
C:\> whoami
nt authority\system