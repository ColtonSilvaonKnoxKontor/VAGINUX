# VAGINUX
VAGINUX is an Operating System that uses the BrainFuck variant called VaginaFuck with extensive set of syntax and Linux Kernel in x86. Linux Kernel provides us much of the drivers needed to access hardware, filesystem and process management, while the next layer of this OS, the `init` (written in c++) is the core interpreter that executes the command that is written entirely in VaginaFuck script.

# Bootloader
VAGINUX already includes Simple Bootloader X86 (SBX86) that can boot Linux Kernel, although it does not let you add additional command parameters. It also supports multi-boot that let's you add more `.img` disk files, tested in Linux based Distribution, Temple OS and Kolibri OS. This bootloader is tested in QEMU emulation, but not natively in bare physical hardware. It currently only supports ext4 filesystem access.

# Required Linux Kernel
VAGINUX already includes the Linux Kernel extracted from antiX Linux.

Optionally, you can use the Linux Kernel from any distributions as long as the kernel points to the `/sbin/init` to initiate the VaginaFuck shell interface. You can obtain it by copying the `vmlinuz` file from the `/boot` directory. The minimal the kernel, the better it is, since you don't need networking, cryptographic, advanced security, and most useless peripheral drivers. You only need analog/digital video and audio, mouse and keyboard + framebuffer support for this.

# Required Software
If using QEMU, use `qemu-system-i386` from terminal. If other virtualization software, use 32bit support.

To run VAGINUX, just do `qemu-system-i386 -drive format=raw,file=alpha.img`. For multi-boot feature, just add `-drive blah blah blah`. `.iso` is not supported here due to SBX86 being implemented only with raw image support.

For bootloader compilation, a certain [GCC](https://github.com/lordmilko/i686-elf-tools) architecture of i686 is required to produce ELF binary meant for bare-metal. 

# VaginaFuck Features
1. Unlike linear array model for BrainFuck, VaginaFuck supports dynamic 64-bit of memory cells.
2. Supports Data Types; Floats, Integers, Strings and Bools.
3. Direct Access to system calls, IO and graphics.
4. Added arithmetic and extensive set of mathemathic operations.
5. UTF8 character support.
6. Serial Console enabled.

# Comparison from Existing BrainFuck written OS
1. BrainFuck has limited memory access, VaginaFuck has a LOT.
2. BrainFuck kernel is written by scratch, hence limited system access, VaginaFuck uses Linux Kernel to let the Linux handle the work under the hood. It can provide you already what you need, even the Networking support.
3. BrainFuck is a byte-level, VaginaFuck is an object-level.
4. To output Hello, You have to write a BrainFuck like this one: `++++++++++[>+++++++>++++++++++>+++>+<<<<-]>++.>+.+++++++..+++.>++.<<+++++++++++++++.>.+++.------.--------.>+.>.`, VaginaFuck reduce the need to manipulate individual cells, thus lessening the per character byte usage, just like this: `z +++*++* ++++*+++* ++++*+++* ++++*+++* ++++*+++* $ O`. The `*` operator helps lessen the need of imputting more `+` in which printing a japanese character 世界 using only UTF-8 supported brainfuck in a nutshell takes about a thousands of `+`.
