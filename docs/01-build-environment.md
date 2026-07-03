---
title: "1. Build environment setup"
nav_order: 2
---

# Setting up the AROS x86_64 build environment on Linux

This guide takes you from a plain Linux installation to a complete AROS
development environment: a cross-compiler that produces AROS binaries,
and a full AROS system running as a window on your Linux desktop that
you can test them in.

**Time required:** about 2–3 hours, most of it waiting for compiles.

**You need:** a Linux machine (this guide was written and verified on
Pop!_OS, which is Ubuntu-based — any Debian/Ubuntu derivative works the
same), roughly 20 GB of free disk space, and basic terminal comfort:
you can `cd` into a directory and run a command. That's genuinely all.

---

## Before you type anything: the one mistake that ruins everything

There are **two** AROS source repositories on GitHub, and picking the
wrong one is the single most common way to waste a weekend:

- ✅ **`https://github.com/deadwood2/AROS`** — the stable ABIv11 line.
  This is what you should build against, and what AROS One
  (the distribution you'll run in VirtualBox in guide 2) is built from.
- ❌ **`https://github.com/aros-development-team/AROS`** — the bleeding
  edge development tree. 

**Why this matters:** ABI stands for *Application Binary Interface* —
the low-level contract between your compiled program and the operating
system: how functions are called, how libraries are opened, how memory
is laid out. If you compile a program against one ABI and run it on a
system built with another, it doesn't "mostly work" — it crashes
immediately, typically with an error like *"Illegal address access"*
in `Exec_B3_OpenResource`. If you ever see that crash, your code is
probably fine; your toolchain and your AROS system disagree about the
ABI.

Everything in this guide uses `deadwood2/AROS`, so you'll be fine.

---

## What we're actually going to build

Two things, both from the same source tree:

1. **A cross-compiler** (`x86_64-aros-gcc`). Your normal `gcc` produces
   Linux programs. This one runs *on* Linux but produces *AROS*
   programs. That's all "cross-compiler" means: it runs on one system
   and targets another.
2. **A "hosted" AROS.** AROS can run as a normal Linux process — it
   opens as a window on your desktop, but inside that window is a real,
   complete AROS. This is the fastest possible test loop: compile,
   copy the binary in, run it. No virtual machine, no reboot.

---

## Step 1 — Install the build dependencies

AROS's build system needs a fair pile of tools and libraries. Install
them all in one go:

```bash
sudo apt install gcc g++ make git flex bison gawk python3 python3-mako \
                 libx11-dev libpng-dev genisoimage cmake curl nasm \
                 autoconf automake libxext-dev liblzo2-dev libxxf86vm-dev \
                 libsdl1.2-dev byacc yasm xorriso mtools
```

You don't need to know what each of these does. But one deserves a
special mention: **`cmake`**. It's easy to assume you don't need it —
nothing obvious in AROS uses it — but one component deep inside the
build (cunit) does. Without cmake, the build runs for a long while and
then dies partway through. It's in the list above, so you're covered;
this note exists for the day you set up a new machine from memory.

*(Fedora users: the same steps work with `dnf`, but the package names
differ slightly.)*

## Step 2 — Get the source and build the cross-compiler

First, create a home for everything AROS-related and clone the source:

```bash
mkdir -p ~/Aros/arosbuilds
cd ~/Aros/arosbuilds
git clone https://github.com/deadwood2/AROS.git AROS
```

The source tree ships with a build script that handles all the
configure/make ceremony for you. Copy it up next to the source and run
it:

```bash
cp ./AROS/scripts/rebuild.sh .
./rebuild.sh
```

The script shows a numbered menu. Choose:

```
1) toolchain-core-x86_64
```

This builds the cross-compiler. **Expect 20–40 minutes** depending on
your machine. Go make coffee; there's a longer wait coming in step 3
anyway.

When it finishes you'll have a new directory,
`~/Aros/arosbuilds/toolchain-core-x86_64/`, containing
`x86_64-aros-gcc` and friends. (You'll also see a
`toolchain-core-x86_64-build/` directory — that's just the scaffolding
used during the build. Ignore it.)

## Step 3 — Build the AROS system itself

Run the same script again, and this time pick the debug build of the
hosted system:

```bash
cd ~/Aros/arosbuilds
./rebuild.sh
```

Choose:

```
2) core-linux-x86_64 (DEBUG)
```

**Expect a long wait.** This compiles the entire operating system —
kernel, libraries, Workbench, the lot.

Why the DEBUG variant? As a developer you *want* the debug output.
When your program misbehaves, hosted AROS prints diagnostics to the
Linux terminal you started it from — that terminal becomes your
window into what the OS thinks is happening.

## Step 4 — Put the cross-compiler on your PATH

So you can type `x86_64-aros-gcc` from anywhere instead of spelling
out the full path every time:

```bash
echo 'export PATH="$HOME/Aros/arosbuilds/toolchain-core-x86_64:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

Verify it worked:

```bash
x86_64-aros-gcc --version
```

If that prints a gcc version string, your toolchain is live.

## Step 5 — Boot AROS on your desktop

The built system lives deep inside the build output. Start it like
this:

```bash
cd ~/Aros/arosbuilds/core-linux-x86_64-d/bin/linux-x86_64/AROS
./boot/linux/AROSBootstrap
```

A window opens — and that window is AROS, booted straight into
Workbench. Congratulations: you're running an Amiga-family operating
system as a Linux process.

Leave the terminal you started it from visible; that's where debug
output appears.

---

## Know your way around the build output

You'll come back to these paths constantly in the next guides, so
here's the map. Everything lives under `~/Aros/arosbuilds/`:

```
~/Aros/arosbuilds/
    AROS/                             ← the source code you cloned
    rebuild.sh                        ← the build script
    toolchain-core-x86_64/            ← the cross-compiler (on your PATH now)
    toolchain-core-x86_64-build/      ← build scaffolding, ignore
    core-linux-x86_64-d/              ← the built AROS system
        bin/linux-x86_64/AROS/        ← "AROS root" — the OS itself
            boot/linux/AROSBootstrap  ← the program that boots it
            C/                        ← drop your compiled programs here
            Development/              ← headers & libs you compile against
```

The two you'll use daily:

- **`Development/`** — when you compile a program (guide 3a), you point
  the compiler here with `--sysroot`. This is what guarantees you're
  building against the same ABI your AROS was built with.
- **`C/`** — copy a compiled binary into this directory and it's
  runnable by name from the AROS Shell, just like on a classic Amiga.

## A 60-second AROS Shell primer

Open a Shell inside AROS and you'll notice it's not bash. The most
disorienting difference for a Linux user: **you change volume by
typing its name with a colon**, not with `cd /path`:

```
RAM:       ← switches to the RAM Disk
System:    ← switches to the System volume
```

And any program in `C:` runs by simply typing its name. That's enough
to get around for now; there's more shell lore in guide 3b.

---

## Checklist before moving on

- [ ] `x86_64-aros-gcc --version` prints a version string
- [ ] `./boot/linux/AROSBootstrap` boots AROS into Workbench
- [ ] You know where `Development/` and `C/` live

All three? Then you're ready for [guide 2: AROS One in
VirtualBox](02-arosone-virtualbox) — a full AROS installation in a VM,
which you'll need because hosted AROS has no networking.

---

## Troubleshooting

**The system build dies partway through (something about cunit).**
cmake is missing. `sudo apt install cmake`, run `./rebuild.sh` again.

**A program crashes with "Illegal address access" in
Exec_B3_OpenResource.**
ABI mismatch. Either the binary was compiled without `--sysroot`
pointing at your `Development/` directory (covered in guide 3a), or it
was built against the wrong AROS tree entirely. Recompile correctly —
the code itself is likely fine.

**`x86_64-aros-gcc: command not found` after step 4.**
Open a new terminal (or re-run `source ~/.bashrc`) — PATH changes only
apply to shells started after the edit, or run `echo $PATH` and check
the toolchain directory is actually in there.
