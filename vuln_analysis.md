# Vulnerability Analysis

## Driver Entry

The driver creates a device:

\Device\GIO → \DosDevices\GIO


This allows user-mode access via `CreateFile`.

---

## IOCTL Handler

The driver processes IOCTL requests and dispatches:

0xC3502808 → vulnerable memcpy function

---

## Vulnerable Code

```c
v3 = *v2;          // dest
v5 = v2[1];        // src
v4 = size;

loop:
    byte = *(src)
    *(dest) = byte
```

## Root Cause

No validation of src/dest
User fully controls memory access
Leads to arbitrary read/write

## Impact

Kernel memory disclosure
Arbitrary write primitive
Privilege escalation to SYSTEM
