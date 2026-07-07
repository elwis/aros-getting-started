---
title: "3b. Coding on AROS itself"
nav_order: 5
---

# Editing and compiling directly inside AROS One

*Status: draft — being written and verified.*

---
layout: default
title: Developing Natively on AROS x86_64
nav_order: 11
---

<!-- TODO: write full guide, verify gvim install source and steps -->

# Developing Natively on AROS x86_64

Notes from getting a real coding workflow going directly on an installed AROS One system — no cross-compiling from Linux required, just a shell and an editor on the machine itself.

## GCC Is Already There

If you selected "Install Development Software" during InstallAROSOne, you already have a native compiler. AROS ships a GCC toolchain specifically patched for the AROS ABI — the official build repository targets **GCC 6.5.0** by default, with some newer builds using later versions (8.3.0 has also been used). It lives under `System:Development/` and works exactly like you'd expect from the shell:

```
gcc myfile.c -o myfile
```

No cross-compiler, no `--sysroot` juggling needed for this — that's only necessary when building *for* AROS *from* another OS. Compiling natively on AROS itself, the toolchain already knows what it's targeting.

## Editing: MUI-Vim 9.2

The Vim available for AROS x86_64 (`vim_9.2-x86_64-aros-v11.lha`, by Ola Söder / sodero) is **not a straight upstream port** — it's a from-scratch integration with a full MUI GUI on top of the Vim core. Worth knowing before you go looking for behavior that exists in "normal" Vim but might not be wired up the same way here.

### No `:terminal`, no `+job`

Checking `:version` inside this build confirms it plainly:

```
-job
-terminal
```

Both compiled out. This means the usual trick of splitting a shell into the bottom half of the editor (`:terminal`) simply isn't available in this build — not a configuration issue, just not compiled in. If you're coming from a Linux/tmux-heavy workflow and reflexively reach for a split terminal instead of Alt-Tabbing (which doesn't really exist as a concept on AROS/Amiga-style window management anyway), this is the one habit that has to change.

### The workaround: `:!`

Without a live terminal buffer, the practical substitute is running commands through Vim's shell-escape and reading the result back:

```vim
:!gcc % -o %:r
```
Compiles the current file (`%`) to a binary with the same name minus the extension (`%:r`).

```vim
:!gcc % -o %:r && %:r
```
Compiles **and** runs it in one go — press any key afterward to return to the buffer.

Bind it to a function key in your `.vimrc` for a one-tap compile-and-run loop:

```vim
nnoremap <F5> :!gcc % -o %:r && %:r<CR>
```

It's not a live split-pane shell, but it removes the actual friction (leaving the editor, finding the file again) without needing a feature that isn't there.


## Summary

- Native `gcc` is available out of the box with the Development install — use it directly, no cross-compilation needed
- This Vim build is a MUI-native port, not a 1:1 upstream compile — check `:version` before assuming a feature exists
- No terminal/job support here — `:!command` and `:read !command` are the practical substitutes for a split shell

