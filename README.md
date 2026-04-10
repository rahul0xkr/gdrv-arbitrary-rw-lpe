# Windows Kernel LPE via GDRV.sys (Arbitrary Read/Write → Token Stealing)

##  Overview

This project demonstrates a Local Privilege Escalation (LPE) vulnerability in the GDRV.sys driver. The driver exposes an IOCTL that performs a memory copy operation without validating user-supplied pointers.

By exploiting this vulnerability, we achieve arbitrary kernel read/write and escalate privileges by stealing the SYSTEM process token.

---

## ⚠️ Vulnerability Summary

IOCTL: `0xC3502808`

The driver performs:

memcpy(dst, src, size)

Where:
- `dst` = user-controlled
- `src` = user-controlled
- `size` = user-controlled

No validation is performed → leads to arbitrary memory access in kernel.

---

## 🔬 Root Cause

```
v8 = *(_BYTE *)(v6 + v3++);
*(_BYTE *)(v3 - 1) = v8;

v3 → destination pointer (user-controlled)
v5 → source pointer (user-controlled)
v4 → size (user-controlled)
```
This results in arbitrary read/write primitive.



## ⚙️ Exploitation Steps
1) Open device \\.\GIO
2) Build arbitrary read/write primitive
3) Leak kernel base via EnumDeviceDrivers
4) Resolve PsInitialSystemProcess
5) Read SYSTEM EPROCESS
6) Traverse ActiveProcessLinks
7) Locate current process
8) Steal SYSTEM token
9) Spawn SYSTEM shell
    
🧪 Demo

[+] Token stolen!
whoami
nt authority\system
🛠️ Build & Run
g++ exploit.cpp -o exploit.exe -lpsapi
exploit.exe
