---
title:       "ReactOS 0.3.12 Released"
author:      "fireball"
type:        news
date:        2010-10-20
changed:     2013-02-23
draft:       false
promote:     true
sticky:      false
url:         /reactos-0312-released
aliases:     [ node/300 ]
news:        [ "news" ]

---

<h2>The ReactOS team is proud to announce the release of ReactOS 0.3.12.</h2>
<p>This is a huge release for the team, not just with regards to the number of improvements which this release holds but in terms of the leap forward architecturally, stability wise and in bringing some of the more modern aspects of the NT kernel into ReactOS. </p>
<p>It's been almost a year since the last release and whilst this is understandably excessive, it was required to stabilize the OS due to the nature of the work which was undertaken. Focus at the start of this release was on a single area, the trap handler mechanism, which resulted in a complete rewrite of this area. This brought with it the need for more changes which escalated into many areas getting an overhaul and many new technologies being developed and brought into the core. What resulted from this was a vastly more modern kernel containing code which had been exercised significantly less than the code it replaced. This triggered a large testing phase to bring the stability and compatibility to levels above that of the previous code. </p>
<p>During the preparation of this release, 259 bugs were fixed, including 61 regressions some of which originated from ReactOS 0.3.7. Ten of those bugs are more than 3 years old, with the oldest fixed bug being #969 (5 years old). </p>

<p>A heavily cut down list of some of the more major changes which have been going on in the past year is as follows: </p>
<ul>
<li>Memory Manager - The memory manager continued to see much work as the ARM team replaced each component piece by piece whilst also maintaining the functionality of the old manager.  Although 0.3.12 does not completely switch over to the new manager, what is obvious are the speed, stability and compatibility improvements of this new model. </li>
<li>NMI support - ReactOS can now handle NMIs with a Red Screen of Death, useful for capturing hardware errors detected by the CPU or Bus. Additionally, support for 3rd party NMI callbacks has been implemented, which is useful for certain server systems. Finally, support for generating a crash dump during an NMI is partly implemented, which can help when a machine is frozen or hung and an external NMI dump switch is used. </li>
<li>Trap Handler Rewrite - Almost all CPU faults, trap, exception, and system call code is now written in C instead of Assembly. Many legacy and/or deprecated code paths have been disabled and performance-heavy debugging paths disabled by default. Additionally, the x64 and ARM ports now share much more of this code. Finally, the code is much cleaner and can take advantage of compiler optimizations to generate the best possible code for the CPU instead of writing hand-crafted assembly that was specific only to certain CPU models. Work is ongoing to remove even more of the last remaining Assembly routines. </li>
<li>EMS - Support for Emergency Management System (or Headless) has been partially implemented. The boot flags documented by Microsoft are supported, and certain debug output is sent to the serial port as expected. Work is ongoing to provide the EMS logging capabilities and to move the existing legacy KDBG debugger over EMS. SAC (Special Administration Console) driver work is also in progress to compliment this. </li>
<li>PnP Compatibility - Various improvements have been done to increase hardware support and support for loading 3rd party drivers. </li>
<li>ACPI Improvements - The ARM team has implemented the basic drivers required for supporting batteries and 3rd party UPS/battery drivers, including support for the ACPI Composite Battery specification. This support is not currently enabled in this release because ACPI is still undergoing work. </li>
<li>New PCI-X driver - The ARM team has been slowly working on the new PCI bus driver. Previously, ReactOS was using a very simple and mostly stubbed PCI bus driver which lacked support for many real-world PCI bus features, PCI-to-PCI bridges etc.  With this new driver, compatibility on real hardware, not just virtual machines, should improve significantly, along with performance. </li>
<li>SxS support – Side-by-side code was added, along with loading and finding manifest files. It’s an important step forward to be compatible with modern applications which use this technology. </li>
<li>Pool Corruption Fixes - Perhaps the most serious of these suspected leaks were fixed thanks to combined efforts of key ReactOS developers utilising advanced methods including a customized version of QEMU virtual machine. </li>
<li>Timer and message handling rewrite - Incorrect handling of non-queued messages led to deadlocks in some applications which the message handling rewrite resolved. The timer implementation rewrite is also completed by this release which fixes many timer-related problems, most known is the “need to move mouse in order to download in FireFox”. </li>
<li>x64 build - While the x64 port is still in an early stage regarding the functioning of the kernel, most of the generic compilation issues are resolved and necessary core functionality implemented. These efforts have been merged back into trunk, so that trunk can be compiled for x64 target. With the help of automatic builds, possible breakages can now quickly be detected and resolved. Don't expect it to boot to GUI though!</li>
</ul>
<p>The <a href="../wiki/index.php/ChangeLog-0.3.12">changelog</a> for 0.3.12 is also markedly different from previous releases, with an emphasis on conveying an understandable and concise summary of major changes in the release.  Thus instead of duplicating that summary here, we invite you to peruse its <a href="../wiki/index.php/ChangeLog-0.3.12">contents</a> and see what has been accomplished. </p>
<p>Whilst the ReactOS team has still been attending many public events and conferences in various different countries, we’ve been out of the news due to what may appear as a quiet patch or a lull in activity. We hope this release will go some way to show that we’ve been busier than ever behind the scenes. </p>
