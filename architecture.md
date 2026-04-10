# Exploit Architecture

User Mode

     ↓
   
DeviceIoControl

     ↓
   
GDRV.sys

     ↓
   
Vulnerable memcpy

     ↓
   
Arbitrary Read/Write

     ↓
   
Kernel Memory Access

     ↓
   
EPROCESS Traversal

     ↓
   
Token Overwrite

   ↓
SYSTEM Privilege

