---
title: "2. AROS One in VirtualBox"
nav_order: 3
---

# Installing AROS One x86_64 in VirtualBox

*Status: draft — being written and verified.*

What this guide will cover:

- Downloading AROS One v1.3 (64-bit) and creating the VM
- VM settings that actually work
- Networking: why the adapter type matters
  - Use **Intel PRO/1000 MT Desktop (82540EM)** with **NAT**
  - Inside AROS One: select **e100.device**, leave everything on DHCP
  - Why PCnet-FAST III does *not* work ("Message too long")
- Getting files in and out of the VM
  - In: building an ISO with `genisoimage -R`
  - Out: running an FTP server on the host with `pyftpdlib`

<!-- TODO: write full guide, verify on a fresh VM -->
