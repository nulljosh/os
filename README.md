<img src="icon.svg" width="80">

# os

![version](https://img.shields.io/badge/version-v0-blue)

A small x86 kernel, written from scratch. Boots in QEMU and drops you at a prompt.

| Piece | Where |
|-------|-------|
| Boot | `boot.S` — multiboot1 header, stack, jump to `kmain` |
| Kernel | `kernel.c` — VGA text, keyboard, clock, shell |
| Link | `linker.ld` — flat ELF32 at 1 MB |
| Check | `check.sh` — boots it and asserts the banner reached VGA memory |

## Build

```sh
brew install lld qemu
make run      # boots to the shell
./check.sh    # boot check
```

Commands: `help` `clear` `echo` `time` `reboot`

## Architecture

<img src="architecture.svg" width="600">

Loaded straight by QEMU's `-kernel`, so there is no bootloader and no ISO to build.
It stays in 32-bit protected mode with no paging, and never installs an interrupt
table: the keyboard is polled on port `0x60` and `time` reads the CMOS clock rather
than counting timer ticks. That trades a background tick for about a hundred fewer
lines. Long mode, an IDT and paging go in when something needs to run while the
shell is waiting for a key.
