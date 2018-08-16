---
title:       "Newsletter 85"
author:      "Z98"
type:        news
date:        2011-06-20
changed:     2013-02-20
draft:       false
promote:     true
sticky:      false
url:         /newsletter-85
aliases:     [ node/226 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>Theme Support</li>
# <li>Debug Support</li>
# <li>Mountmgr Driver</li>
# </ul>

---
<h2>Theme Support</h2>
<p>Giannis has completed support for drawing in most non-client areas of a window such as captions, borders, and scrollbars.  Scrollbars turned out to be especially problematic as the code that handles calculations of how much to scroll is also responsible for redrawing the changes.  This has the very unfortunate consequence of duplicating a significant amount of drawing code inside the scrolling update function.  Maintenance of this code will likely be troublesome due to this, but currently Giannis is not aware of a way around the problem.  Next up will be implementation of user mode hooks, which will allow Giannis' theme drawing code to function in ReactOS.  Currently most of his testing is done in Windows.</p>
<h2>Debug Support</h2>
<p>The ReactOS Project in the past tended to create custom solutions for its various needs, few of which have aged well.  Two examples are the rbuild system for building ReactOS and the RosCMS used on the website.  Another tool put together in the project's earlier days was a custom debugging format called rsym.  Simplistically, rsym provided little more beyond the line number of a particular instruction and was in fact generated using the line number information GCC provides by default.  The original motivation may have been to keep the kernel small enough to fit in the early-boot memory space.  The stabs line number information GCC adds heavily inflates the size of the resulting binary and rsym was a way to hack in some line number information without the resulting binary having multi-megabytes worth of data added on.  On the other hand, a debug symbol format already existed that was very compact and provided much more than just line numbers.  That debug symbol format is DWARF and Art Yerkes has been working on adding support for it in kdbg, the kernel debugging tool used by our developers and the automated testing system.  Art has completed enough of the needed work to allow developers to now be able to get the value of arguments passed to functions using DWARF.  Once the rest is also done, the project can discard rsym entirely and have much better support for debugging the kernel and other parts of ReactOS without any major increases in binary size.</p>
<h2>Mountmgr Driver</h2>
<p>The NT5/6 era storage stack consists of several components, each with their own responsibility.  The mountmgr driver, as its name suggests, is responsible for the mounting of devices and registering them in the device namespace for the OS to use and present to the user.  In the NT4 architecture, this responsibility was in the kernel.  ReactOS for most of its lifetime followed the NT4 design but Pierre Schweitzer has been working to implement a mountmgr driver. One additional piece of functionality a genuine mountmgr driver brings is the ability to mount volumes as directories, much like on Linux. This functionality however requires a more advanced filesystem than FAT, so it will still be some time before ReactOS can take advantage of it.  The rest of the storage stack simply registers callbacks to the plug-n-play manager to be notified when new volumes have been mounted.  Other drivers in the storage stack also need to be updated to be aware of the mountmgr driver, but notifications for these drivers can also be faked if updating them is not possible.  Johannes Anderwald is especially looking forward to this work being completed as it is another piece of the puzzle for USB storage.</p>
<p>&nbsp;</p>