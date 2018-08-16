---
title:       "Newsletter 90"
author:      "Z98"
type:        news
date:        2012-01-24
changed:     2013-02-20
draft:       false
promote:     true
sticky:      false
url:         /newsletter-90
aliases:     [ node/138 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>Wireless</li>
# <li>USB Status</li>
# <li>Shell32</li>
# <li>Filesystem Corruption</li>
# </ul>

---
<h2>Wireless</h2>
Cameron Gutman has spent the last month or so adding the pieces needed to support wireless network cards to ReactOS. A good portion of the work entailed writing the NDIS protocol driver (ndisuio) that handles sending wireless NDIS object identifier (OID) messages. These messages handle querying network drivers about their status and capabilities and also set the receiving mode the device should operate in. In addition to the kernel side, a user mode utility is needed to let end users actually make these requests. This utility is currently wlanconf on ReactOS and handles binding network adapters to ndisuio. Additional work was done in the DHCP service to deal with a rapid succession of requests to release and renew IP addresses and the TCP/IP component driver to make sure it used the correct OID message when checking the state of the network device.

In the runup to 0.3.14, Cameron merged his work into trunk and the mainline ReactOS trunk is now capable of joining open and WEP encrypted wireless networks, provided the network driver in question works correctly. More secure encryption like WPA and WPA2 require the operating system to do a complex handshake sequence that is not yet implemented. However, the current state of wireless support in Cameron's branch represents a major step forward in ReactOS' usability on modern systems.

<h2>USB Status</h2>
Johannes Anderwald has recently done more work with the USB stack started by himself and Michael Martin and completed two of the four host controller interface drivers needed to support the standards in use today. ReactOS currently has ohci for USB 1.1 and ehci for USB 2.0. The project currently needs uhci to support Intel's standard for USB 1.0 and xhci to support the new interface for USB 3.0. In addition to this, the human interface device driver for mice has also been completed and tested on Windows, but a variety of problems in ReactOS prevented it from working. A HID driver for keyboards has been started but is not yet complete. Support for mass storage devices will require additional drivers. Another item missing is support for composite USB devices such as a combined keyboard/mouse plug.

Cameron joined in at this point to help and dealt with a range of device registration and installation bugs, resolving the issues that prevented USB mice from working in ReactOS. He also fixed other issues in the USB stack itself, ranging from crashes to build fixes for the various components. Johannes expects that keyboard support should be fairly straightforward thanks to Cameron's fixes. Once the USB branch has been sufficiently tested and merged into trunk, ReactOS will take another solid step forward in terms of usability. This step however will likely be post 0.3.14.

<h2>Shell32</h2>
Rafał Harabień has been busy with work on the shell32 library, fixing issues ranging from icon loading, dialog windows, and various buffer/memory errors. Icon loading in property windows was using an incomplete reimplementation of code that already existed in shell32. Rafał removed the broken code and modified property windows to use the already existing implementation. Now property windows are able to load icons beyond just those defined in the registry. The "Open With" dialog window was also rewritten to show all applications defined in the registry. The dialog will also no longer add duplicate entries to the registry. A great deal of work remains to be done with shell32 before it and explorer_new can replace the current shell. The start menu for example is effectively undocumented and unimplemented and Rafał is unsure of how responsibilities are divided between the explorer shell and the library for file browsing.Until these issues are sorted out, ReactOS will remain with its current shell.

<h2>Filesystem Corruption</h2>
Pierre Schweitzer had recently rewritten the code in dir.c, causing an already existing problem to manifest as garbage being written to the disk. After some digging, Pierre noticed that a tester had encountered a similar issue and had provided a debug log. The log however pointed to a different function being the cause than Pierre's own investigations. Upon closer inspection, Pierre noticed both seemingly guilty functions were calling another internal function, one in the runtime library that dealt with paths. This function was one of several that Alex Ionescu was in the process of rewriting and fixing, but Alex had not yet finished the work. As such, the brokenness of the old code combined with Pierre's own changes was somehow causing the filesystem corruption. Pierre completed the work Alex had started and the corruption seems to have disappeared, but at this time Pierre remains unsure in what way the old code was broken to cause the filesystem driver to write garbage data.