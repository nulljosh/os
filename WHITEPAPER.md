# os Technical Whitepaper

**v0** | September 2026

A small x86 kernel written from scratch. It boots in QEMU and drops you at a
shell prompt.

## Scope

The point is the smallest thing that is honestly a kernel: it owns the
machine, talks to hardware directly, and takes commands. Everything that a
real OS adds for concurrency is deliberately absent.

| Piece | File |
|---|---|
| Boot | `boot.S`: multiboot1 header, stack, jump to `kmain` |
| Kernel | `kernel.c`: VGA text, keyboard, clock, shell |
| Link | `linker.ld`: flat ELF32 at 1 MB |
| Check | `check.sh`: boots it and asserts the banner reached VGA memory |

## Design decisions

- **No bootloader, no ISO.** QEMU's `-kernel` loads a multiboot ELF directly.
- **32-bit protected mode, no paging.** Flat segments are enough for a shell.
- **No interrupt table.** The keyboard is polled on port `0x60`; `time` reads
  the CMOS clock instead of counting timer ticks. That trades a background
  tick for about a hundred fewer lines. An IDT, paging and long mode come in
  when something has to run while the shell waits for a key.
- **VGA text mode.** Writes go straight to `0xB8000`.

Commands: `help` `clear` `echo` `time` `reboot`.

## Build

```sh
brew install lld qemu
make run
./check.sh
```

## License

MIT 2026, Joshua Trommel
