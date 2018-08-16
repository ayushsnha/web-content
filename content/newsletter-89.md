---
title:       "Newsletter 89"
author:      "Z98"
type:        news
date:        2011-12-15
changed:     2013-02-20
draft:       false
promote:     true
sticky:      false
url:         /newsletter-89
aliases:     [ node/230 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>RPC</li>
# <li>Memory Manager</li>
# <li>CPU Microcodes</li>
# <li>Setup</li>
# <li>S/G DMA</li>
# <li>Path Fixes</li>
# </ul>

---
<h2>RPC</h2>
<p>Remote procedure calls underline a lot of things in Windows, especially services.  Thus the Remote Procedure Call Runtime (rpcrt4) library is fairly important to the continued operation and stability of ReactOS.  It is also a component in which the project is heavily reliant on Wine for.  Unfortunately, just because Wine code works on Linux does not mean it will work on ReactOS, and attempts to sync rpcrt4 in the past have produced monumental headaches, breakages, and difficulties in ReactOS.  Thomas Faber, the Google Summer of Code student responsible for the new kernel mode test suite, has found what he believes to be the main cause for RPC failures on ReactOS.  When a process makes a remote process call, it gets a handle to represent the call. This handle is supposed to uniquely identify each RPC operation, and this uniqueness was supposed to be guaranteed by a random number generation. Wine relied on the random number generation implemented in /dev/urandom on Linux, which obviously does not exist in ReactOS. In addition, the NT function Wine's code called to generate a number was not even implemented on ReactOS, so the same value could be returned multiple times when rpcrt4 asked for a unique ID for different RPC operations. This confused the operating system and applications and did a very thorough job of breaking everything that depended upon RPC. Thomas has dealt with this particular issue, which hopefully means future attempts to sync rpcrt4 with Wine's code does not prove anywhere as disruptive in the future.</p>
<h2>Memory Manager</h2>
<p>Many testers have been complaining about the ReactOS installation hanging when attempting to copy the mshtml.dll file. Attempts to diagnose the issue have led developers deep into the bowels of the memory manager and exposed a rather wide range of issues. The cause of the hang was due to heap data structures being zeroed out, resulting in the heap manager losing track of blocks of memory while mshtml.dll was being copied. Without that information, the heap manager was unable to get to the next block of memory and was stuck in an infinite loop. Possibly a third or more of the development team have tried their hand at debugging this issue, with most coming to the same conclusion: the memory manager rewrite begun in ARM3 needs to be completed. Thomas was one of the participants and at first thought setupapi, the code that handles installation of drivers, libraries, and associated files, was at fault. This was due to the rather messy state and complexity of setupapi's code and Thomas suspected bugs were resulting in memory overflows and the like. To test this theory, Thomas implemented his own heap manager and ran setupapi on top of it. The result however effectively exonerated setupapi, as the assertions that triggered were ones free from setupapi side effects. This suggested a much deeper problem inside of the memory manager. Even more frustration, the failures seemed to be timing dependent, which suggested some kind of race condition.</p>
<p>Cameron Gutman also participated in the great MSHTML hunt but was diverted by a patch submitted by Nikolay Myltsev dealing with paging. In general, the amount of virtual memory an application has claimed and the amount of that memory actually in resident in the physical memory are two very different things. In Windows, the working set represents recently touched pages of a process' virtual memory, and an upper and lower limit exists for all processes. Windows will attempt to keep the working set resident in physical memory, assuming there is enough free space to do so. The purpose of the working set is to prevent a single application from eating up all physical memory and the memory manager will balance the memory demands of different processes by evicting pages as needed from different process working sets. Pages that have been changed while in physical memory need these changes written back out to disk before their block of memory can be reused. When the time came to evict pages of a working set, ReactOS would search for enough clean, unmodified pages to reclaim to satisfy the needed amount of space. What it did not do was attempt to write out changes from dirty pages if there were not enough clean pages to satisfy a memory request. This failure would eventually result in the working set balancer running out of pages it could evict and crash the operating system. After massaging Nikolay's patch and adding a few of his own changes, Cameron committed the work and the balancer now actually writes out changes from dirty pages, permitting them to be reclaimed.</p>
<h2>CPU Microcodes</h2>
<p>This topic requires a slight segue into CPU design and manufacturing. Most programmers understand that processors conform to an Instruction Set Architecture, specifying what instructions and operations the processor can execute. What many people are unaware of or do not think much about is the fact that most processors do not directly execute instructions as specified by the ISA. Instead, there is a lower level of commands known as microcodes that control the operation of the processor to produce output conforming the an ISA. These microcodes are for the most part completely undocumented as AMD and Intel treat them as trade secrets. Modern CPU designs however are extremely complex and hardware bugs can slip in which do not correctly obey a microcode. Fixing such an issue in hardware would require changing the design, but modern CPUs can actually be patched with updated microcode to compensate for these errors. AMD and Intel both provide these patches freely to operating system developers as binaries, but the process in which Windows applies them is effectively undocumented. Pierre Schweitzer looked into the issue and had some thoughts as to how ReactOS may do it, but the issue is of fairly low priority right now. It is however a rather interesting problem and highlights some of the complexity that can be encountered at the very lowest levels of the hardware/software interface.</p>
<h2>Setup</h2>
<p>When installing ReactOS on low memory systems, a design decision from long ago could significantly increase the time it took to copy ReactOS. The compressed cab file that basically holds ReactOS needs to expanded during the first stage installation step. The original developer of the cab expander never accounted for the possibility that the cab might be bigger than the present physical memory. As such, every time the expander needed to begin extracting a new file from the cab, it would start from the beginning of the cab and search sequentially until it reached the desired block. This had the very unfortunate consequence of constantly forcing ReactOS to page in and out the same data over and over again. Cameron noticed this behavior and modified the cab expander to keep track of where the expander was in the cab file. This way, the expander does not need to start reading from the beginning again to resume expanding the cab, providing much nicer file access behavior from the memory manager's perspective.</p>
<h2>S/G DMA</h2>
<p>Traditional DMA operations require a single continuous block of memory to transfer data to. Unfortunately there is not always a big enough block available. Scatter/Gather DMA works around this by taking smaller chunks, storing the location of the chunks, and writing data to those chunks. The underlying support for S/G DMA was already present in ReactOS but the API that presented the functionality was not present. Cameron went and implemented that API, allowing drivers, mostly for network cards, that depend on S/G DMA to start working in ReactOS.</p>
<h2>FreeLoader Memory Optimization</h2>
<p>A while back Timo Kreuzer noticed that freeldr was being rather inefficient in its memory usage. There are effectively two types of data freeldr needs memory for, data that is passed onto the kernel, and data that is only needed by freeldr to finish booting the OS. Originally freeldr allocated a single chunk of 4MB and intermixed the two types of data. This forced the entire 4MB to be preserved for the kernel to read. Finding this a bit wasteful, Timo split the single chunk into two different heaps, one for each type. This allowed freeldr to free up all the memory irrelevant to the kernel, helping decrease the initial memory footprint. A minor change, but small ones like this can together build up to make big differences in the long run.</p>
<h2>Path Fixes</h2>
<p>Many of the regressions the new loader caused were traced back to a variety of almost but not completely correct behavior in functions dealing with paths. Since the loader does a lot of file accesses during its operation for loading executables and libraries, it makes extensive use of path related functions. Alex Ionescu took the time recently to go in and rewrite many of the core functions, dealing with string encoding differences and conversions, shortcut parsing, and correct error codes for failures. Together these seem to have squashed many more loader related regressions and will hopefully also allow more applications in general to work correctly.</p>
<p>&nbsp;</p>