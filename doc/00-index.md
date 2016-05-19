# Foreword

The field of Computer Science moves very quickly. Innovation is not only present in the field, but is an absolute necessity for the technology industry that has grown around it. As a result, it can be difficult to stay up-to-date with all of the change that supports the field, and forces it to remain adaptive.

Much effort has been taken to provide resources to lower the barrier to entry. Various organizations, such as [code academy](https://www.codecademy.com/) have developed free curriculums providing tutorials on programming various computer languages. The [raspberry pi foundation](https://www.raspberrypi.org/resources/) has helped to reduce the cost of commodity computing hardware to help bring technology opportunities to those who would otherwise not have had the financial means. Indeed, just a few decades ago a personal computer would have costed thousands of dollars, have required skilled assembly, and been difficult to do much of anything with due to the small community of entheusiasts. Nowadays, what would have been considered a supercomputer by the previous generation of information technologists is available for about [cost of a cup of coffee](https://www.raspberrypi.org/products/pi-zero/), at just over the size of a small candy bar.

With all these programming and hardware resources abound, one resource that still seems missing is a manual of practical knowledge for how computers actually work. While there are text books, university courses, and indeed scattered resources available, there isn't a single accessible, up-to-date body of work available to guide an initiate in this area. The purpose of this publication is to fill that gap, and be that resource. This type of computer system knowledge is quite valuable. Many organizations fervantly pursue the mythical "full stack developer" - someone capable of not only understanding the nuances of the programming behind a high-level application, but also its overall architecture, and the underlying systems that support it.

This sort of systems expertise has traditionally belonged in the hands of system's architects, operations personale, system administrators, or so-called "power users". The contemporary, and perhaps more 'politically correct' terms for these specialists are now titles such as 'site reliability engineers', 'Dev/Ops', and as the progressive 'production engineer'. Within the industry, there has long been a divide between those with development and product experience, so called 'product developers', or 'pure devs', and those with operational and systems knowledge. This division can be unhealthy, inefficient, and indeed dangerous. Without a sufficient lack of understanding of the platform and systems that keep an application running, it can be difficult or impossible for a product developer to create efficient, secure, scalable code. Likewise, without an understanding of the nuances of the application itself, a system administrator can become too detached from a problem domain to be able to optimize it.

Thus, the important of blurring the lines of this divide have real practical advantages. The full-stack developer need not be a myth, or a unicorn. The benefits of full-stack knowledge are far reaching, and this publication aims to facilitate developers wishing to make the journey 'down the stack', as well as to provide a firm foundation to new initiates in the field of information technology. A broad range of topics will be covered, including theoretical and practical aspects of the operation of a computer system, and current 'hot topics' in the field. It is worth noting that [opsschool](www.opsschool.org) is another fantastic resource in this area, though sadly it is quite incomplete, out of date, and largely abandoned.

Please feel free to contribute to this open source publication if you have something to add; be it a correction, addition, or expansion of the material here.

# The stack

The term 'full-stack' is a bit unfortunate in it's ubiquity and ambguity. It's actual meaning has been obscured by inconsistent use, and its role as a buzz-word for recruiting and marketing purposes. To combat this, we'll use the traditional OSI stack model to denote the different layers of abstraction that make up "the stack". By abstraction, we mean that it takes something complicated, and provides a logical simplification of it.

The OSI model provides us with 7 distinct layers of abstraction ranging from the physical world to the virtual world of our application. It is particularly relevant in understanding computer networks, but the terminology it provides is a useful addition to our technical lexicon. It is worth noting up-front that one of the main deficiencies of this model is that it over-simplifies Layer 7 - the application layer. The application layer is can be significantly complicated, and indeed have its own 'stack' with additional layers of abstraction. These will be discussed in greater detail a bit later.

![osi model](img/osi.png) [@wikimedia]

Sufficed to say, we don't need to trouble ourselves with the nuances of each of these abstraction layers just yet. As each layer becomes relevant, we will describe it in greater detail later on. Right now, let's focus on layer 1 and layer 7, the very top and bottom of the OSI stack. The physical layer is the 'real world', ie, the actual physical appliances, devices, and media types that we can see, touch, and hold. Layer 7 is considered the 'application layer', and can be thought of as the purly logical or 'virtual world'. This is the space in which our code runs, and where the true power of information technology is unlocked. As we build our understanding of the stack, we'll develope a model for how these physical and virtual worlds interact.

## Furthur reading

A wonderful resource on this topic is 'Computer Organization and Design: the hardware/software interface'([@cod]), which is a proprietary textbook by a UC Berkely professor on computer achitecture.

# Automata Theory - The theory of computing

Before diving in on the hardware side of computer systems, it's important to build an understanding for the theory, and conceptual model of a computer system. This is a very broad and complicated topic, of which only the surface will be scratched here.

Maybe without realizing it, you probably already understand the gist of what a finite automaton is.

## Flow charts

Everyone has seen a flowchart before. If not, here's a joke flowchart explaining what flowcharts are from the popular nerd website [xkcd](www.xkcd.com):

![flow chart](img/flow_charts.png) [@xkcd]

The idea is pretty simple. A bunch of states are defined as 'boxes', and transitions between them are defined by connecting lines ^[often called 'edges' in formal circles]. At each state in the flow chart, the input (in this case, the answer to the question in the state box) governs which transition to follow, and thus which state to go to next. Using this methodology, it can become fairly simple to break down a complicated sequence of questions into a single, handy, flow chart!

For example, here's real a flowchart that helps to troubleshoot a non-functioning lamp:

![lamp](img/lamp_chart.png) [@wikimedia]

Now, let's imagine that we built a flow chart, and every state asked the question 'yes' or 'no', and we need only answer with 'y' (for yes) or 'n' (for no). As we answered the questions in each state of the flow chart, we could write this down into sequence of letters ^[conventionally called a 'string' of characters - think letters connected together by a piece of string to remember this], ie, 'ynyyny' for 'yes, no, yes, yes, no, yes'. From the starting state then, we can always end up in the same state of the flow chart using this string! If we think of our flow chart as a machine, we could conceptually write down our answers on a long piece of paper, wrapped that up into a cartridge, and treat it like a cassette tape ^[ in case you've never seen one before, a [cassette tapes](https://en.wikipedia.org/wiki/Compact_Cassette) was similar to a VHS, and preceeded CDs, much as VHS preceeded DVDs]. At each state, our flow chart machine would move advance the cassette tape by one character ^[kind of like a [ticker tape](https://en.wikipedia.org/wiki/Ticker_tape) in slow motion], and use that character to transition to the next state.

Our input tape and our flow chart machine are starting to get interesting now. For every one of the final states of the flow chart ^[where the flow chart has solved your problem for you], you could record a tape that gets you to that state, and built up a library of mix tapes that are all of the possible solutions to the problem. These tapes that lead to a solution are special tapes - the input they provide causes our flow chart machine to 'halt', meaning that when we play them back the machine will go throw a series of transitions, until it reaches the final state with the solution, and no longer needs to keep going.

Looking a bit more closely at those tapes, we can see that our machine understands a very simplistic language - strings of 'y' and 'n'. In computer science, we have a special term for a problem where 'yes/no' is a valid answer to a question - a 'boolean' problem. The letters 'y' and 'n' belong to the alphabet we as humans understand, that has 26 letters. However, we're being a bit wasteful here - we're only using 2 letters, and are leaving 24 unused! If we represented our answers with numbers, we could simply use two numbers - '0' for no, and '1' for yes. You might be thinking 'well, what about all those other numbers from 2 to 9'? Those are called decimal numbers ^[deca being the metric prefix for '10', think 'decade'], which is the 'alphabet' of numbers we are used to working with - we count from one until 10, and then we sort of 'roll over', and reuse the same symbols. If we constrain our 'alphabet' of numbers to only the two numbers '0' and '1', can are using a special number system that you might have heard of before called 'binary'! We'll talk a bit more about binary later, but for now all we need to know is that in binary the only valid numbers are '0' and '1', where '0' means 'no', and '1' means 'yes'.

Using this new binary alphabet, we can replace those cumbersome letters on our cassette tapes with more simple '0' and '1'. So our string from earlier 'ynyyny' becomes '101101'. ^[In fact, we need not even write on the tape at all anymore! We could simply punch a hole in the tape to represent a '1', and leave the tape unpunctured to represent a '0'. Not so long ago, this method of punching holes to represent zeros and ones is how [punch cards](https://en.wikipedia.org/wiki/Punched_card) were used to encode input for computers!]

We can actually simplify our flow chart machine quite a bit - we can turn it into a simple table! All we need to do is give each state a name or identifier. By convention, we'll just number them in the order they appear. Now, for each answer '0' or '1', we'll just say what state to go to next. From the lamp troubleshooting example earlier, let's number the different states:

0. Lamp plugged in?
0. Plug in lamp
0. Bulb burned out?
0. Replace bulb
0. Repair lamp

Now we can represent it as a table: ^[Note that "Lamp doesn't work" isn't a real state, as we unconditionally transition to "Lamp plugged in", which is why we have left it out. We have also left out the halting states here (the ones with no transitions), as there is no 'next state' for them.]

|Current state | Input | Next state |
|--------------|-------|------------|
|       0      |   0   |     1      |
|       0      |   1   |     2      |
|       2      |   1   |     3      |
|       2      |   0   |     4      |

This table is a pretty decent representation of our machine. It's not quite so nice to read, but it's a lot easier to draw, and helps us simplify how our machine works - as it reads the tape now, all it needs to do is look what state to transition to given its current state, and the input from the tape. Let's formally call this our 'state transition table', as it tells our machine how to transition between states for each input it might get.

So, to recap, we now have a special machine that solves problem for us - our flow chart machine. It can understand a special language called binary, where '0' corresponds to 'no', and '1' corresponds to 'yes'. Knowing the current state, and the next character read from the input tape, our machine can look up what state to go to text using a state transition table. We can give it a sequence of these, which will take us from our starting state to a state where our problem is solved and the machine 'halts'. What we have done here is built something that now meets the formal definition of a Finite Automaton!

## Finite Automata

Our flowchart machine from the previous example was a machine with a certain number of states, which we consider to be a [Finite State Machine (FSM)](https://en.wikipedia.org/wiki/Finite-state_machine)

In formal terms, there are two types - Deterministic Finite Automatons (DFAs), and Nondeterministic Finite Automatons (NFAs). We won't bother too much with the specifics here, but will point out a couple of the differences:

* Every DFA defines at most one transition for each state, and each possible input character
* Every NFA can be represented as a DFA

In our flow chart machine, we had only two possible input characters - '0' and '1', and at each state we transitioned to exactly one other state. So, what we built was a simple DFA!

Describing our flow chart machine in [formal terms as a DFA](https://en.wikipedia.org/wiki/Deterministic_finite_automaton) ^[don't get thrown off by the math notation here, this is 'theory', which has very little to do with real math. We will use some very basic [set theory](https://en.wikipedia.org/wiki/Set_theory) though]:

* The set of all of the states in our flow chart machines can be called $Q$
* The numbered states from earlier will each be denoted by a $q$ (lowercase), and their number. For instance, state 1 will be $q_1$.
* We'll call the starting state $q_0$ by convention
* Our binary language, containing only '0' and '1', can be called $\Sigma$ ('sigma')
* Our state transition table from earlier, which tells us how to transition between states, can be called $\Delta$ ('delta')
* Our halting states are the ones with no transitions from them, which in our case is $q_3$ and $q_4$
* The set of all halting states will be called $F$, such that



### Example: Regular expressions

Determinism and non determinism
Simulation
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

A great reference is Computer Organization and Design [@cod] (textbook)


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
