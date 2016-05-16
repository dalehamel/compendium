# A computer boots

## Hardware initialization

The signal to boot may come from a physical switch that is connected to the power subsystem of the computer, or from a management subsystem such as the BMC (more details in another section on BMCs / IPMI). [@test]

As the power on signal has been received, the the primary power of a server's PSUs will begin to feed the motherboard and attached devices. If the power is AC, then a transformer will adjust the voltage to 5V, the operating voltage of the motherboard. In some telco data centers, DC 5V is supplied directly. [@opsschool]

This starts the process of hardware initialization. The elecritity from the PSU energizes the circuits of the motherboard, which is the primary PCB, which houses the PCI/PCIe busses, CPU, memory, I/O connections for perephiral/satellite devices. As this occurrs, the satellite devices, such as hard drives, and add-in cards will typically receive power directly from the PSU (though in some cases, such as USB, the power and data links are aggregated), and begin their own initialization process. The motherboard performs an initialization sequence. All chips (ICs) on the motherboard will initialize themselves to a pre-defined boot-up state. The most important of these - the CPU - will initialize its registers and fetch a program from a special, read-only memory chip called an EEPROM.

## BIOS

This program is called the BIOS (note: UEFI is becoming more prevalent, but is a similar idea, it's just a more standardized approach to interfacing with motherboard devices. BIOS stands for Built In Operating System. It is essentially a small kernel developed to perform very few tasks related to configuring the other subsystems on the motherboard, and performing initialization to load the actual operating system. The BIOS understands various I/O sources, including network devices, removable devices, ATA/SCSI disk interfaces, and other PCI add-in cards that may also present boot sources.

The BIOS has a pre-defined boot order, which governs which order it will scan these devices. This is configurable, to allow selection of which hard drives or network interfaces, etc the system should attempt to boot from first. Once a viable boot devices has been probed, the BIOS will hand over control of the system to that device. Generically, this manifests itself in the form of some kind of 'bootloader'.

## Bootloaders

The job of a boot loader is to fetch the operating system from somewhere, and place it into memory. Once in memory, a special 'boot' instruction is executed. While implementations of this instruction sequence differ, the gist is that the first address of the first code page of the data image of the operating system kernel are set as the intstruction register of the CPU. This is the very first context switch the system will perform during its up time. The BIOS may unload itself from memory, or this may be performed by the kernel (implementations vary). From this point on, the kernel has complete control over the system, and whatever early boot code that is within the kernel begins to execute.
In terms of specific boot sequences, there are two that are particularly popular: booting from a hard disk, and booting from a network, and there are more esoteric approaches used by embedded devices.

Hard disk booting is relatively simple. When the BIOS probes the various hard drives, it looks for an executable program in a special format within the first 512 bytes of the drive (if MBR, different procedure of similar concept for GPT partition table). If found, the program will specify where to fetch the actual boot loader. In the case of GRUB, this is 'grub stage 1', which will point to the binary for grub stage 2 on the boot partition of the hard drive. Any bootloader works in a similar way, including LILO, SYSLINUX, and ISOLINUX (a SYSLINUX variant usually used for creating bootable CD/DVD images).

In contrast, network booting is a bit more involved. An option rom on the NIC (Network Interface Controller), or UEFI extension, will perform a network boot sequence, typically PXE or iSCSI. PXE is effectively the BOOTP extension of DHCP. The option rom will send a broadcast DHCP discover packet. If a DHCP server is on the network, it will respond with a DHCP offer containing a payload of data, including the network address the NIC should use to communicate with other devices on the network. Importantly, the BOOTP protocal specifies a standard set of DHCP option flags including the name of the ROM to load, and the address of the boot server to load it from. This ROM may be PXELINUX (the network variant of SYSLINUX), or another firmware such as the popular (and superior) iPXE. The NIC will fetch the specified from from the specified location, tyically via the TFTP protocal. Thus, PXE is effectively DHCP + BOOTP + TFTP. Once loaded, this firmware behaves very similarly to GRUB, and other disk-based bootloaders. It will fetch and load the kernel into memory from a network source, and hand over control to the kernel.

Embedded devices behave in a similar way, but typically use a very custom bootloader (or, more increasingly, das U-BOOT) and have effectively no BIOS. Instead, these systems may be required to be flashed (have it installed to an EEPROM) with their OS ahead of time, or hardcoded to boot from a specific device such as an SD card in the case of the popular Raspberry Pi and many other embedded Linux boards.

## The kernel

For the purposes here, we will refer only to Linux, but the principles are the same with other operating systems. When the kernel begins execution, it will perform its initialization sequence. This will set up various kernel data structures, such as thread pools, the scheduler, kernel threads or 'kthreads' with specific duties will be spawned, and memory will start to be initialized. The kernel will discovery devices on the system such as I/O buses, and begin to scan those and enumerate other devices on these subsystems. Once the kernel has finisihed initializing itself and the hardware, it will look to discover filesystems, and locate the init process to be spawned.

In some cases, an initramfs (initial ram-based filesystem) is supplied along side or embedded into the kernel. If this is the case, this filesystem typically contains an executable 'init' script often bundled with 'busybox' - a statically compiled swiss-army knife containing many core unix utilities. This init script can perform arbitrary logic to discover and set up the root filesystem and prepare to execute the 'real' init process. Upon completion, this script will execute a 'pivot\_root' call, which calls exec on the real init process, and roots the file system hierarchy at a specific mount point.

## Init

This spawns the first process of the system - PID 1. Init will never stop running until the system shuts down, and becomes the main controlling userspace process of the system. The job of init is to manage userspace processes, typically called services or daemons. Requests to spawn/start new proccess are issued to init, and these processes may be queried for status, or shut down through init. The job of init is to keep track of these processes, and be their parent process.

There are several implementations of init processes for linux. The most traditional is the standardized sysV interface, and the similar openrc, each which utilize a runlevel-based lexical order execution of shell scripts, each responding to standard commands like 'start' and' 'stop'. More recently, alternatives like Upstart have been developed and subsequently abandoned by canonical for Ubuntu, and systemd has become the de factor 'future' of init processes on Linux.

## Services / Daemons / user input

Services are children of init that provide a useful function or application. On a server, this may include applications like SSH, HTTP, FTP, and other protocol servers. Really, any long-running process that performs some sort of useful application may be spawned by init.

Init may even spawn processes looking for user input, such as launching a getty against a terminal or serial interface. This will wait for user input, and provide a means of logging in to the operating system and creating a session. Once logged in, a user may interact with and spawn processes or send signals to init.




