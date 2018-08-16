---
title:       "Newsletter 72"
author:      "Z98"
type:        news
date:        2010-05-15
changed:     2013-02-23
draft:       false
promote:     true
sticky:      false
url:         /newsletter-72
aliases:     [ node/213 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>General Developments</li>
# <li>Message Queue</li>
# <li>Handle Mishandling</li>
# <li>PnP and UniATA</li>
# </ul>

---
<p>Just a heads up, I'm out of the country for the next two months and will have unreliable net access, so do not expect any newsletters until late July.</p>
<h2>General Developments</h2>
<p>A few weeks back Aleksey Bragin directed the developers to take a look at a long list of regressions that had accumulated.  Individually the problems were for the most part minor, though a few were near show stoppers.  However, cumulatively they represented a major perception problem in ReactOS' progress.  He thus asked the developers to commit some time to fixing them and slowly the list of issues has been reduced.  At the same time he requested that developers refrain from committing changes that might introduce new regressions, so the overall rate of commits was reduced.</p>
<p>The servers hosting the progress recently underwent some upgrades and movement, which caused some downtime.  An upgrade to a newer version of php actually broke parts of the site but that was resolved quickly. Upgrading the operating system on a main server caused more troubles, which is why the website, mailing lists and SVN services were down around the middle of the week.<br>
Most of the server movement is done now and you can enjoy speed improvements on the BuildBot, Doxygen and ISO sites. Furthermore, the movement enables us to add another build machine at no cost in the upcoming days.</p>

<h2>Message Queue</h2>
<p>Michael Martin managed to squash a series of bugs after committing a fix related to message handling in ReactOS.  Win32 applications operate in a loop that receives and handles messages sent to it from the OS in reaction to user interaction and system events.  These messages are essentially handled by a message pump and it handles sending the messages to the procedure handling them in an application.  There are a type of messages known as non-queued messages that bypass both the system maintained queue of messages and the thread specific queue of messages, being directly routed to the message handling procedure of an application.  In ReactOS these messages were not treated like this and ended up going through the message pump instead of being sent directly to the message handling procedure.  Michael corrected ROS' behavior and along with a few other fixes managed to singlehandedly take out six bugs in a single commit.</p>
<h2>Handle Mishandling</h2>
<p>While testing Adobe Acrobat 6, Timo Kreuzer ran across a rather interesting and very serious bug.  To help improve performance the Win32 subsystem will attempt to defer costly transitions to kernel mode by batching drawing commands so that it can issue them en mass instead of doing context switches for them individually.  Unfortunately in the batch code a table that held pointers to GDI regions was being indexed incorrectly.  The value being used for indexing was actually the byte offset into the array, not the index offset.  The end result was the OS lost track of the regions in the batch and leaked the handles associated with them.  This of course resulted in massive resource leakage and ultimately crashed the OS due to other issues in the Win32 subsystem.  The fact that this problem could cause a crash is actually non-obvious as all this code was in user mode.  To crash the OS requires failures in kernel mode, so somewhere down the stack the OS was not properly dealing with the leakage.</p>
<p>When Timo fixed this issue, another one popped up.  Whereas the OS was losing track of handles and regions before, it now ends up prematurely deleting them.  This is again in Adobe Acrobat 6 and effectively rendered it unusable thanks to the mis-draws that occurred.  Collectively, these two issues together are severe enough that one has to wonder why they were not exposed earlier. For the time being Timo has disabled this part of the batch code until he can find the root cause of all this.</p>
<h2>PnP and UniATA</h2>
<p>When UniATA was made the default driver for SATA and PATA drives, a minor mistake was made where channel enumeration was skipped because the PCIIDE driver was not being run anymore.  This caused the Plug and Play system to not be told the existence of these channels and thus would not load the drivers for them.  Since these channels are what connects drives to the rest of the system, not having their drivers loaded and running meant being unable to talk to the disks.  Or at least that is what would have happened had ReactOS' PnP system worked correctly. Ironically, the non-conformance of the storage stack to a PnP model meant the drivers involved did not particularily care. ReactOS simply installed the UniATA driver for both the drive controller and the channel and it worked, albeit not because it should, but because the rest of the stack didn't know any better. Cameron Gutman noticed this problem as part of his PnP work and corrected it, though there are few if any visible effects from his fix. At the very least it will help ease the transition to a more PnP compliant storage stack.</p>