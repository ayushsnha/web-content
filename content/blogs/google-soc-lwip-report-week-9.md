---
title:       "Google SoC lwIP Report Week 9"
author:      "zhu48"
type:        blog
date:        2016-07-25
draft:       false
promote:     false
sticky:      false
url:         /blogs/google-soc-lwip-report-week-9
aliases:     [ node/14458 ]

---

<p>Last week ended with my realization that lwIP was not thread-safe, and me reading up on various ways to get around that.&nbsp;</p>
<p>Last weekend, I spent a lot of time tinkering with lwIP's core code to see how hard it would be to make it thread-safe. I ultimately failed to actually make the library thread-safe, but I did learn a lot of things about lwIP that I hadn't known before I started digging into the source code in so much depth. Now I know, for example, that lwIP kept several singly-linked lists of TCP protocol control blocks - one for bound PCBs, one for connected PCBs, one for listening PCBs, and one for timed-waiting PCBs. I also got a very good look at lwIP's locking and signalling system, which a previous programmer named Cameron Gutman had implemented using Windows Event Objects.&nbsp;</p>
<p>Armed with my failed attempt at adding locks to lwIP core and my new-found knowledge of lwIP's inner workings, I had a long discussion with my mentor, Art Yerkes, over the next few days about how to proceed. He thought that the simplest thing to do, and the best way to start, would be to add a big global lock to serialize lwIP calls. It would have seemed like a great starting place for me too, except that I learned that it didn't work. Besides my own driver's calls into lwIP, lwIP also received a call from somewhere else every time an incomming packet arrived. This call would start a thread that could potentially walk all of the global PCB lists and possibly modify them, and would definitely call into my driver.&nbsp;</p>
<p>Knowing this, Art suggested a way to serialize all lwIP threads instead by modifying the semaphore implementation. Every time a thread was supposed to wake up, it would need to acquire a big global lock. Every time a thread slept, it would release that lock. This way, every lwIP thread would only be able to run when no other lwIP thread was running. I played with implementing this lock while continueing to discuss details with Art. When implemented as a kernel spin lock, this lock degraded performace so much that I got impatient waiting for ReactOS to install. It could not simply be implemented as anything else, because network drivers at this level regularly move into and out of Dispatch Level, where only spin locks are allowed. I would have to come up with a craftier solution.&nbsp;</p>
<p>At this point, however, Art made me aware of some bigger issues with my driver. In my driver, there is a data structure - the Address File - that stores active IP addresses. This structure is meant to represent just an IP address - a 32-bit address and a 16-bit port number. In my driver, I incorrectly used the Address File as the data structure representing some user-mode sockets - the listening socket on the server side, and all client-side sockets. This mistake had to be fixed before I could really proceed onto worrying about serializing some other code. Since I had to change a lot of my driver code anyways, I took the opportunity to do a massive cleanup, which took me all the way to the end of this week.&nbsp;</p>
<p><a href="https://www.reactos.org/forum/viewtopic.php?f=2&amp;t=15677">Discussion</a></p>
