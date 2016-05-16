# The stack

# Theory of computing

Finite state machines
Regular expressions as an example
Determinism and non determinism
Turing machines and Alan Turing
Stacks, programs, algorithms
Halting problem and infinite loops

# Digital logic, discrete mathematics

* Binary
* Hexadecimal
* AND, OR, NOT
* Transistors / D-latches

# Computer internals and architecture

How does a computer actually work


## CPU and instruction sets

* Note about special registers and operations, how context switching works

* Instruction pipeline
* ALU
* Memory pyramid

## Practical computing


Van Neuman Machine

Harvard architecture

Little man computer

CISC, RISC



# Networking



## Fundamentals


Packets and frames
Mac addresses, IP addresses, ARP




## Layer 2 networks and broadcast domains


Switching and hubs
ARP storms and stale ARP
Gratuitous ARP


## Layer 3 networks


Routing and routing protocols
CIDR notation
Gateways
Route tables
BGP, RIP, OSPF
Autonomous systems


## Layer 4 transport protocols


TCP vs UDP
TCP sessions and overhead
SYN/ ACK handshakes
Data gram anatomy
Socket bindings
Passing data in buffers and kernel space


Layer 5 - session
Layer 6 - encryption
Layer 7 - application


Useful protocols to know
HTTP
DHCP
SNMP
Conceptual FTP (similarities to HTTP)
Note about SSL

# Operating System Theory

# Kernel

Contexts, resources, hardware, threads, user space, kernel space

Pages and virtual memory

# Linux

Start with disclaimer about other operating systems, largely similar but opaque and proprietary. Linux being open source makes it great to study, and it's ubiquity makes it highly practical to understand.

Choose Gentoo as a case study due to quality of documentation

# Linux kernel

Building it
Customizing config options
Overview of different types of options
Notable features
Netfilter, IPtables
Cgroups and namespaces (footnote about containers)
Driver support, file system formats
Scheduling algorithms
Modules vs builtin
Kprobes and Uprobes (footnote about systemtap)
Kexec


Loading the operating system



# Boot methods


PXE
Steps, including DHCP, TFTP, potentially HTTP
Tangent about iPXE
Undi format


Disk
LILO
Syslinux / ISOLINUX (footnote on PXELINUX)
Grub


Vmlinuz and initramfs


## Kernel booting

Decompressing itself
Loading initial objects into memory
Discovering devices
Potentially running initramfs
Setting up the root volume


# Linux file system structure



Etc
Boot
Usr
Lib
Var
Tmp
Mnt
Run


Pseudo and special file systems
Tmpfs
Dev
Proc
Sys


Files and folders, inodes, symlinks, hardlinks



Union file systems



Squashfs, overlayfs



# Init

Different Init systems, history, inittab, runlevels

Runit, systemd, upstart, openrc

Systemd

Daemons

Popular daemons like Apache, nginx, ssh, MySQL, Redis, Memcache


# Realtime systems

# System administration

## Testing infrastructure and systems

## Configuration management

## Disposable and immutable infrastructure


# Containers

## Fundamentals

### chroot

### cgroup

### namespaces

## lxc

## docker



# Programming languages

Start with C

Static, dynamic, interpreted, native

Compiler linker, runtime linker,  ELF binaries'


byte code interpreter, JIT compilers, FLEX

Turing completeness

Specific ones ( C, C++, Java, Python, ruby, go)

Garbage collection

Reference counting

# Data structures and algorithms

Queue, stack, graph, tree, hash table

Hash functions

Search

Np hard


# Concurrent and parallel programming

Concurrent clusters (sun grid engine, openmp, etc)

Frameworks

# Markup languages

# Web programming

# Data centres

What are they

Ping, power, pipe
Colo retail vs wholesale


Power distribution


At the data centre level - substations, gensets, power distribution units and wips
Diesel fuel


# Server hardware

* Sensors
* Backplanes
* Baseboards
* Raid cards
* Riser cards
* Nodes vs chassis
* Blades
* Components (drives, NICs)
* IPMI
* DMI, smbios


BIOS and EFI

# Network hardware

## Cable media types

* Types of optics
* Types of copper
* Twinax, molex
* CAB, etc

## Internals of a switch

router (fabrics, linecards)

# Cloud



# Embedded systems


Raspberry pi, micro controllers


# Application Architecture

# Databases and state


ORM
Models
Caching

# Encryption and security principles

## Theory

Cryptography
Entropy
Number spaces and languages


## Ciphers

Block ciphers - AES
RSA


# Artificial intelligence and machine learning


Neural networks
Regression
Statistics
Genetic algorithms


# Data mining and warehousing



Kafka
Hadoop
Map reduce
Columnar data stores




# Organization architecture



CI frameworks
Testing types
Collaboration tools
Bug tracking


Management ?



Shit shield
Problems larger than one person
Organizing
Onboarding
Recruiting
Agile, kanban, waterfall


