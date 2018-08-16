---
title:       "Newsletter 69"
author:      "Z98"
type:        news
date:        2010-03-03
changed:     2013-02-23
draft:       false
promote:     true
sticky:      false
url:         /newsletter-69
aliases:     [ node/209 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>Trap Handling</li>
# <li>ACPI</h2>
# <li>Windows Driver Headers</li>
# <li>ReactOS at Chemnitz Linux Day</li>
# </ul>

---
<h2>Trap Handling</h2>
<p>One of the ways low level communication with hardware is handled is with interrupts and exceptions.  Code still has to be written to deal with these and that code is in the trap handler.  The original trap handler code in ReactOS was pure assembly, which to an extent is inevitable due to the very needing to carry out operations that the C language does not specify a method for. Such operations include manipulating the stack and reading and writing specific registers, which are by definition platform specific and whose inclusion would lessen C's cross platform nature.  As part of the effort to port ReactOS to the ARM platform, the ARM team began rewriting the code to provide at least a very thin abstraction layer in C, with all the architecture specific components being written in assembly intrinsics.  Assembly intrinsics are generally compiler specific macros that indicate which assembly instruction one wants to use in C code.  This helped increase maintainability and readability and resulted in several bugs being fixed in the process. This was not absolutely necessary for the ARM port, but the ARM team decided doing so would have long term benefits for the project as a whole.  Unfortunately, the use of GCC assembly intrinsics rendered MSVC being unable to compile the code.  Previously the assembly files could at least be compiled into binary objects that MSVC could then work with, but the transition to C code made this impossible.  Here Timo Kreuzer stepped in and changed the intrinsics back to using raw assembly.  This may seem as a slight step backward, but the C framework is still in place and the only things that was changed was the usage of GCC assembly intrinsics.  In the process Timo also fixed a few segment and flag settings in the assembly code and converting how function parameters are passed through registers from a GCC specific method to a cross-compiler one. In theory, this will allow even Microsoft's assembler to be used to compile the assembly.</p>
<h2>ACPI</h2>
<p>The Advanced Configuration and Power Interface is a standard that details power management specifications for computers.  ReactOS support for the standard was originally started by Samuel Serapion, who is ostensibly also the other newsletter writer and contributor to the x64 port.  Sam took the reference implementation of ACPI provided by the standards committee and ported it to ReactOS.  However, an issue with how hardware IDs were represented in the code resulted in the code not working.  Cameron Gutman took another look at the code recently and found the issue and now the ACPI components work.  However, the code in ReactOS is still incomplete and a few I/O Request Packets (IRPs) that ACPI generates are still unhandled.  Still, Cameron has resolved the major blocker in getting Sam's initial work actually working.</p>
<h2>Windows Driver Headers</h2>
<p>There has been some minor back and forth and cooperation with the mingw64 project in the past, but about five days ago Kai Tietz approached Amine Khaldi, the bug report wrangler, and asked if ReactOS would be interested in a collaberation on Windows driver headers.  For the most part ReactOS' headers are correct or at least more complete, albeit slightly disorganized. Amine and Kai hope to properly separate the information in the headers and providing correctly structured ones. This work will be done in a branch to avoid major disruptions to development in trunk. Timo Kreuzer and Aleksey Bragin also chimmed in and Timo suggested auto-generating a set of public headers for drivers, much like how Microsoft generates the headers for its SDK and WDK.  There may also be instances where some work is needed to make the headers compatible with mingw64, but the ultimate result would be a driver development kit for Windows separate from that released by Microsoft.  There are still a few issues with using GCC to build native Windows drivers, mostly in association with structured exception handling support, but at least it would be a start.</p>
<h2>ReactOS at Chemnitz Linux Day</h2>
<p>Several members of the ReactOS team will be at the Chemnitz Linux Day expo running from March 13 to March 14.  They will be there to promote the project, answer questions, and mingle with the open source community.</p>