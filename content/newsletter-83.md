---
title:       "Newsletter 83"
author:      "Z98"
type:        news
date:        2011-04-29
changed:     2013-02-23
draft:       false
promote:     true
sticky:      false
url:         /newsletter-83
aliases:     [ node/224 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>GDI Memory Usage</li>
# <li>New USB Drivers</li>
# <li>Accepted Summer of Code Projects</li>
# <li>Another Developer</li>
# </ul>

---
<h2>GDI Memory Usage</h2>
<p>As part of his work rewriting the Graphics Device Interface (GDI) handle manager, Timo Kreuzer ran across what can only be described as an atrocious waste of memory.  The memory allocation granularity for when objects were created was an entire page, 4KB, regardless of whether an object needed that much space to store its attributes.  This thus wastes a considerable amount of memory and also eats up memory addresses.  Timo theorizes this is part of the reason the task manager appears to be leaking entire pages of memory at a time.  Win32k possesses a caching mechanism for these allocations that does not appear to be recycling freed pages so over time all of a system's memory can be exhausted.</p>
<p>Fortunately Timo has developed a solution for these issues.  He has implemented pools of memory for each object attribute type on a per process basis.  Whenever an object is created or destroyed memory is allocated or recovered from these pools.  The pools are organized into sections, which range from a single page of 4KB up to 64KB.  Each pool starts out with one section and more can be added as the need arises, but the amount of memory actually provided for each object creation is no larger than the actual amount needed for each attribute.  A bitmap is used to indicate free slices of memory in each pool, making finding available space much faster than the previous method of asking for entire pages for every attribute of an object during its creation.</p>
<p>Besides the speed increase and the elimination of wasted memory, the pools also eliminate the need for the old caching mechanism.  The pools themselves act as a cache of sorts, holding a reserve of memory quickly available for use during object creation.  Thus Win32k does not need any additional mechanisms for holding onto old chunks of memory to reuse for new objects.</p>
<h2>New USB Drivers</h2>
<p>Johannes Anderwald has worked on a wide range of components in ReactOS, from the kernel to Win32k to sound and now to USB.  He recently took an interest and proposed to Michael Martin that the USB drivers be rewritten in C++ instead to make them easier to maintain and review.  Michael agreed, hence the flurry activity on the new EHCI driver.  Their current objective is to bring it to feature parity with the old EHCI driver Michael wrote and which is now abandoned before moving onto the usbhub driver that is responsible for sending I/O request packets to the EHCI driver.</p>
<h2>Accepted Summer of Code Projects</h2>
<p>ReactOS received six slots for the Google Summer of Code and the selected projects were announced <a href="../en/news_page_66.html">here</a>.  At the time I was busy with real life matters and did not have time to provide much background on any of the projects.</p>
<h3>TCP/IP Driver</h3>
<p>ReactOS has gone through a few iterations of the TCP/IP portion of the network stack.  The latest one used an oskit library for TCP/UDP support, but integrating that library cleanly into ReactOS proved difficult.  A primary consideration for any library that we use is the ability to easily update it when the other project makes new releases.  If the code must be tightly integrated in order to work, the task becomes much more difficult.  The lwIP library is primarily designed for embedded use, making it very lightweight and effectively standalone.  This will hopefully make it much easier to integrate cleanly into the ReactOS network stack.</p>
<h3>Explorer_New</h3>
<p>Work on the new shell was started several years ago by Thomas Bluemel and was continued to some extent by Andrew Hill.  Much of the missing functionality is due to the incomplete shell32 library, whose completion is basically a prerequisite for a correctly working explorer_new.  Thus working on explorer_new also means working on shell32.  A lot of groundwork is already in place for this project, but getting through the home stretch will still require quite a bit of effort.</p>
<h3>Theme Support</h3>
<p>Theme support has actually been one of the more often requested features for ReactOS, often by people who think having a better looking UI will attract more people.  It also happens to be one of the less well documented components of Windows, which made correctly implementing it difficult.  Giannis Adamopoulos has done quite a bit of research on the topic however and believes he can get a working implementation going</p>
<h3>Audio Stream Mixing</h3>
<p>While Johannes Anderwald has made great progress in implementing the sound stack for ReactOS, there still remains a lot of work to do in controlling multiple audio streams.  The need to do this is quite common, especially if one has audio notifications enabled in the OS or applications.  Right now ReactOS handles this very poorly, so having an audio mixer would make the system that much more usable</p>
<h3>Kernelmode Testsuite</h3>
<p>This was something that Amine Khaldi pushed for, as a test suite able to deal with kernel functionality would hopefully help make catching breakages that can break the ability of ReactOS to boot or cripple the OS faster and easier.  It will also be interesting to see how this gets implemented, as a failing test is much more liable to crash the OS.</p>
<h3>GDI Font Driver</h3>
<p>A while ago I wrote a newsletter detailing the issues with ReactOS' font rendering.  Those interested in the details can go <a href="../en/newsletter_55.html#sec0">here</a>.  To sum it up, the rendering codepath is completely wrong in ReactOS and we ignore a lot of the hinting information font glyphs provide.  A proper font driver would remedy that issue and remove one more major wart in the Win32 subsystem's architecture.</p>
<h2>Another Developer</h2>
<p>Gabriel Ilardi gained commit access a while ago but was away from the IRC channel for several weeks so this fact slipped my mind while I prepared the previous newsletter.  Either way, please welcome the semi-newest developer to join our ranks.</p>