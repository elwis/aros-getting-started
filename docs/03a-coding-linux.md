---
title: "3a. Coding on Linux"
nav_order: 4
---

# Writing and cross-compiling your first AROS program on Linux

*Status: draft — being written and verified.*

What this guide will cover:

- Setting up VS Code for AROS C development (and why the header
  squiggles are safe to ignore)
- A minimal C program and a reusable Makefile
- The one flag that matters: `--sysroot` pointing at your built
  Development directory — and the "Illegal address access" crash you
  get without it (ABI mismatch, not a code bug)
- Running your program in hosted AROS (copying to `C:`)
- Testing the same binary on AROS One in VirtualBox
- Saving your work to GitHub (including why your `.gitignore` needs
  extension-less binaries — AROS executables aren't `*.exe`)

<!-- TODO: write full guide -->
