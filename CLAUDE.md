# os

Freestanding i386 kernel. No libc, no dependencies beyond clang, ld.lld and qemu.

- `make` links `kernel.elf`; `make run` boots it; `./check.sh` is the only test.
- Cross-compiles with stock Apple clang via `-target i386-unknown-none`. No
  cross-toolchain needed, do not add one.
- No interrupts and no paging on purpose, see the README. Anything that needs a
  background tick has to bring the IDT and PIC with it.
- `check.sh` asserts on raw VGA memory through the QEMU monitor, because a
  headless `screendump` renders black even when the kernel is running fine.
