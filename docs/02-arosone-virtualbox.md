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
  - Inside AROS One: select **e100.0device**, leave everything on DHCP
  - I did not get PCnet-FAST III to work but should with the right driver ("Message too long")
- Getting files in and out of the VM
  - USB Pendrive is probably the easiest
  - Samba should work (didn't try myself)
  - In: building an ISO with `genisoimage -R` is easy on Linux Host, set as optical drive in Virtualbox
  - FTP on the host with `pyftpdlib` and a simple client on AROS

<!-- TODO: write full guide, verify on a fresh VM -->
