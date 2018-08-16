---
title:       "Newsletter 77"
author:      "Z98"
type:        news
date:        2010-10-27
changed:     2013-02-23
draft:       false
promote:     true
sticky:      false
url:         /newsletter-77
aliases:     [ node/218 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>0.3.12</li>
# <li>Alternate ReactOS Memory Manager Module (ARM3)</li>
# <li>CMake</li>
# </ul>

---
<h2>0.3.12</h2>
<p>As one can see from the front page, 0.3.12 has been released into the wild.  In many ways 0.3.12 provides more a transitional view of ReactOS development than presenting loads and loads of new features or functionality.  The 10 months between it and 0.3.11 provided a long window in which developers could drop in new features.  Unfortunately many of them promised to be extremely disruptive and the decision was made to disable the full functionality of several before shipping.  The biggest such piece is the new memory manager that will be detailed in a following section.</p>
<p>One category of changes that did survive, partially because there was no way to disable it and mostly because the code worked, was a series of rewrites by the ARM team that converted low level signal handling and interfaces to C code instead of assembly.  Not only did this vastly improve portability, a rather important consideration for the ARM team, but major bugs were also fixed in the process.  The end result was not only more correct handling of interrupts, system calls, and the like, but also better performance.  Various bugs in the old assembly code resulted in redundant operations being carried out and the cumulative effect was a much slower experience.</p>
<p>I would also like to thank the people who helped with the changelog, dealing with some of the lists that needed to be compiled and letting me focus on sorting through the commit log.&nbsp; It is my sincere hope we never go this long between releases again, if only so I do not have to wade through ten months worth of commit messages.</p>
<h2>Alternate ReactOS Memory Manager Module (ARM3)<br /></h2>
<p>The memory manager rewrite mentioned above was initiated by the ARM team and is in many ways a very remarkable effort.  Rewriting such a critical component of an operating system is a nontrivial task and promised all sorts of regressions, bugs, and general mayhem regardless of what approach the ARM team took.  To try to minimize this, the ARM team chose to slowly co-opt the responsibilities of the current memory manager piece by piece.  The initial implementation did nothing more than build up the data structures needed to keep track of memory usage and virtual to real memory address mappings.  Next ARM3 began taking responsibility for the allocation of specific types of memory such as the kernel pool.  In 0.3.12 the new memory manager is capable of managing all pool memory allocations, but even this was disabled due to the risk of regressions.  Since the branching of 0.3.12 the ARM team has committed fixes to all of the issues they are aware of and have had ARM3 take over for not only pool allocations but also initialization of the memory manager in the early stages of booting.  It should not be too much longer before ARM3 supplants the current memory manager, something the project is very much looking forward to.</p>
<p>The accomplishment of the ARM team is all the more remarkable due to the fact that disruption wise, there have been times when ROS was rendered far more unusable than the issues it suffered from ARM3's integration.  The bugs that were exposed during the rewrite on the other hand begs the question of how ReactOS managed to ever work with so many different pieces corrupting each other's data structures.  One especially egregious bug in the configuration manager code was corrupting the paged pool, a rather serious issue considering paged pool tends to hold data critical to the continued stability of the operating system.</p>
<h2>CMake</h2>
<p>The CMake effort continues to make good progress to the point where a working livecd can be created using it.  Not surprisingly some bugs were discovered in the conversion process, though I doubt any of the developers were expecting one as insane as what Amine Khaldi and the ARM team discovered in the library export declaration handling.  RBuild apparently allowed the exporting of non-existent DLL functions, with rather non-deterministic consequences for when someone tried to link to the generated library.  In addition, RBuild does not generate and link functions with the correct decorators.  This effectively generated the non-existent DLL functions since their decorators did not match that of the actual functions.  A side effect of the incorrect decorators and RBuild's poor handling actually resulted in the linking succeeding, though to the wrong export.  Combined with one other feature of RBuild, which was to link the DLL to itself when a function was not found, this should technically result in a very broken library.  That kernel32, the library where these issues were initially discovered, worked at all is due to the minor miracle of all these broken semantics coming together and not biting users of the library.  There is a special case for functions that a dll exports but is in fact importing from another library, but I will leave the curious to read up on what happens in that case themselves.  The bottom line is basically kernel32 was exporting several non-existent functions and linking to itself, issues that thankfully have been fixed in both trunk and the CMake branch.  Fortunately nothing else matching the issues described above has surfaced in the conversion and hopefully nothing will either.</p>