# os

A small x86 kernel. Boots in QEMU to a shell.

    brew install lld qemu
    make run

Commands: `help` `clear` `echo` `time` `reboot`

- 32-bit protected mode, multiboot1, loaded by QEMU's `-kernel` (no bootloader, no ISO)
- VGA text mode, polled PS/2 keyboard, CMOS RTC clock
- No interrupts, no paging, no libc

Verify:

    ./check.sh
