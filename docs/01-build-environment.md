---
title: "1. Build environment setup"
nav_order: 2
---

# Setting up the AROS x86_64 build environment on Linux

*Status: draft — being written and verified.*

What this guide will cover:

- Why there are two AROS repositories on GitHub, and why you must use
  `deadwood2/AROS` (stable ABIv11) — not the development team fork
- Installing build dependencies (apt and dnf)
- Cloning the source and building the cross-toolchain with `rebuild.sh`
- Building the hosted AROS system (core-linux-x86_64 DEBUG)
- Adding `x86_64-aros-gcc` to your PATH
- Booting hosted AROS with `AROSBootstrap`
- Understanding the directory layout you just created

<!-- TODO: write full guide, verify every command on a clean machine -->
