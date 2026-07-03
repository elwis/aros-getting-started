---
title: Home
layout: home
nav_order: 1
permalink: /
---

# AROS — The Things You Missed
{: .fs-9 }

A beginner-friendly, zero-ambiguity guide to building, running, and
developing for AROS x86_64 (ABIv11) — the open source, Amiga-compatible
operating system.
{: .fs-6 .fw-300 }

---

## What is this?

AROS is a modern, open source re-implementation of AmigaOS 3.x APIs that
runs natively on x86_64 hardware. It's a fantastic system — but the
documentation for getting started as a developer is scattered across
forums, wikis, and years-old threads.

This site fixes that. Every guide here follows two rules:

1. **Every command has been run, every path has been verified.** Nothing
   is written from memory or copied from an old wiki. If it's in a guide,
   it worked on a real machine.
2. **Zero assumed knowledge.** If you can open a Linux terminal, you can
   follow these guides. We explain *why* each step exists, not just what
   to type.

## The guides

| # | Guide | What you'll have at the end |
|---|-------|------------------------------|
| 1 | [Build environment setup](docs/01-build-environment) | A working AROS x86_64 cross-compiler and a bootable hosted AROS on your Linux machine |
| 2 | [AROS One in VirtualBox](docs/02-arosone-virtualbox) | A full AROS desktop in a VM, with working networking and file transfer |
| 3a | [Coding on Linux](docs/03a-coding-linux) | Your first C program, cross-compiled on Linux and running on AROS |
| 3b | [Coding on AROS itself](docs/03b-coding-on-aros) | Editing and compiling directly inside AROS One |
| 4 | [Zune for beginners](docs/04-zune-tutorial) | Your first real GUI application, explained line by line |

## Who writes this?

A long-time Amiga user who set all of this up from scratch, hit every
pitfall so you don't have to, and wrote down what actually worked.
