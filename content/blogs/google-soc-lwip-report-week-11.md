---
title:       "Google SoC lwIP Report Week 11"
author:      "zhu48"
type:        blog
date:        2016-08-08
draft:       false
promote:     false
sticky:      false
url:         /blogs/google-soc-lwip-report-week-11
aliases:     [ node/16071 ]

---

<p>This past week, I have primarily focused on thread-safety.&nbsp;</p>
<p>Three weeks ago, I discovered that lwIP's core code is not thread-safe. When left unmodified, each lwIP thread will access several unprotected global linked lists as well as use a set of global variables to process any and all incoming packets. One option to solve this problem was to modify the core code so the global data was protected from concurrent access. I attempted to do this at the end of week 8, but ultimately put aside the multi-threading issue in favor of a massive rewrite and restructuring of my driver. At a certain point in debugging my rewrite, I started encountering multi-threading issues more and more. Thus, I went back to try and solve the problem again.&nbsp;</p>
<p>The first multi-threading issues I ran into actually had nothing to do with lwIP. My own TCP_CONTEXT structs were being modified by two different threads at once, leading to crashes from dead pointer dereferences. At first, I wrote an elaborate lock to achieve serialization. This lock would acquire a kernel mutex stored in the Address File, test some variables in the TCP Context, and then set an ownership variable if the tests passed. If not, the mutex would be released and the test would start over. I came up with this elaborate system to prevent a Context from being disassociated while it was being worked on. Later on it became apparent that this ws unnecessary. All I had to do was acquire a kernel mutex stored in the TCP_CONTEXT iteself for all operations on the Context, including disassociation. Soon into the testing of this Context mutex, I ran into lwIP multi-threading problems again.</p>
<p>During the three weeks I had known about the multi-threading issue, I had had a lot of discussions with Art about how to approach the problem. The suggestion of his I ultimately ended up implementing was to modify the semaphore implementation (which is ultimately left up to the library's user anyways, since atomic operations are platform-dependent) so that no two lwIP threads could run at the same time. On thread startup, each lwIP thread would acquire a global kernel mutex. Every time a thread waited on something, it released the mutex before the wait and reacquired the mutex after the wait. Whenever a thread performed an lwIP library call, it would acquire the global kernel mutex before the call and release it after the call. Kernel mutexes are recursive-safe, so I did not need to worry about dead locking due to lwIP's system of hooks and callbacks. This system was actually relatively simple to implement and was fairly robust. When most of the initial bugs were ironed out, I could run almost 60 (very simple and short-lived) client threads in my VM at once without crashing.&nbsp;</p>
<p>While all the mutexes functioned correctly when there weren't any problems, they would frequently result in deadlocks when I needed to perform IRP cancellation. During cancellation, After some investigateion, I found it was caused by a variety of factors. I wasn't properly blockign IRP cancellations while performing my own cancellations, my implementations of TDI_SEND and TDI_DISCONNECT handlers were flawed, and there were scattered mistakes in various places in my code that I had ot caught becuase the multi-threading issue was a bigger problem. I'm am still in the process of trying to fix all the problems with multi-threading.&nbsp;</p>
<p><a href="https://www.reactos.org/forum/viewtopic.php?f=2&amp;t=15702">Discussion</a></p>
