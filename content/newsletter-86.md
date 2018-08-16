---
title:       "Newsletter 86"
author:      "Z98"
type:        news
date:        2011-07-26
changed:     2013-02-20
draft:       false
promote:     true
sticky:      false
url:         /newsletter-86
aliases:     [ node/227 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>MSVC Progress</li>
# <li>Network Fixes</li>
# </ul>

---
<h2>MSVC Progress</h2>
<p>The entirety of ReactOS can now be built using Microsoft's compiler, but running the resulting operating system is still a ways off. &nbsp;Timo Kreuzer has already squashed several bugs that arose, one such being corruption of filesystem metadata that caused the loading of freeldr to fail.  On x86/x64 platforms, part of the booting process involves the bootsector loading in the FAT table from disk, which needs to be loaded in a high enough address that reading freeldr into memory afterward would not overwrite the table.  Unfortunately the MSVC compiled version of freeldr is larger than the GCC version and did end up overwriting the FAT table, which confused the bootstrapper.  When it tried to query the FAT table to see if there was more to load, it got freeldr code instead and assumed there was more to be done.  When it tried to use the freeldr data as a guide for what to load, the bootstrapper failed and set an error, claiming that loading of freeldr itself also failed.  This was not the case as freeldr itself had actually completed loading.  Timo fixed this issue by changing the location where the FAT table got loaded to ensure it would not get overwritten with MSVC compiled binaries.</p>
<p>Another filesystem related issue was found in the named pipe filesystem driver.  The driver maintains a linked list of client control blocks (CCB) and access to that list is synchronized using a lock.  There however existed a way to access a CCB without that lock, which could result in a race condition.  Specifically, a codepath could have gone ahead and deleted a CCB in the list, but because a separate link existed for that CCB, that link would now point to stale data.  Timo's solution was to use a reference counter to keep track of how many instances of a link to a CCB existed to prevent the CCB from being deleted out from another codepath.  This issue also existed in the GCC build but was more reliably reproduced in the MSVC build.</p>
<h2>Network Fixes</h2>
<p>The brittleness of the ReactOS network stack is sometimes hard to grasp for non-developers, but the list of issues that was recently resolved by Cameron Gutman should provide a picture of just how broken parts of it are.  After a break, Cameron returned and turned his attention to the many failing network tests.  Several were traced back to broken semantics in event notification.  If a program informs the OS it wishes to wait on a particular event and the event has already been triggered or is triggered before the OS actually calls the wait function, the OS is supposed to immediately wake up the program the moment the wait call is made.  This was not done, which meant that programs that required the notification before they would do anything else would hang indefinitely.</p>
<p>Another issue with events dealt with improperly setting permissions for events.  Events are generally created with the EVENT_ALL_ACCESS permission level.  However when a program tried to access the event, the OS code attempts to access the event with the FILE_ALL_ACCESS permission level.  The two are not the same and FILE_ALL_ACCESS actually has several permissions EVENT_ALL_ACCESS does not have, resulting in a permission denied error.  With these fixed, event signaling should work now and applications that previously hung on network applications would get much further.</p>
<p>Beyond event notification of network activity, there were several major semantic errors in the handling of TCP connections by several parts of the network stack.  The recvfrom and sendto functions were treating TCP connections as if they were UDP connections.  Suffice it to say, this simply did not work at all.  The only reason this did not bite more people was few applications actually use the recvfrom and sendto functions with TCP connections, instead relying on the recv and send functions.  Ironically, recv did not work correctly with UDP connections, and again the saving grace was that most applications use recvfrom for UDP.  Fixing these should help deal with edge cases where applications break the norm.</p>
<p>One set of issues that thankfully is not entirely due to ReactOS' own code dealt with shutting down of connections.  TCP connections have a handshake protocol that includes a FIN packet to indicate it has finished transmitting data.  Calling shutdown(SD_SEND); would generate that FIN packet but should still leave the connection open for reads.  Unfortunately the oskittcp library did not do this and completely closed the socket in question, which would cause any read attempts that follow the shutdown call to fail.  Of course ReactOS code had to mirror this brokenness when by completely shutting down a socket when a read operation returned zero bytes.  This would prevent further attempts to transmit data on that connection after a read operation.  The final issue Cameron fixed was also due to oskittcp, specifically due to the library not signaling connection failures to the OS.  With that bug resolved, the ReactOS network stack passes considerably more network tests and hopefully is more usable in general.  At the same time, the issues in oskittcp are what is motivating the migration to the lwip library.</p>
<p>&nbsp;</p>