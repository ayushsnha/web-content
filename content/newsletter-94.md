---
title:       "Newsletter 94"
author:      "Z98"
type:        news
date:        2013-02-27
changed:     2013-02-28
draft:       false
promote:     true
sticky:      false
url:         /newsletter-94
aliases:     [ node/346 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>Site Migration</li>
# <li>Visual Studio</li>
# <li>Dependencies</li>
# <li>Boot Modes</li>
# <li>VPC Serial</li>
# <li>CSRSS</li>
# <li>Desktops</li>
# <li>A Reflection</li>
# </ul>

---
<h2>Site Migration</h2>
The project recently migrated from its old site to one using the Drupal content management system. Work on this migration had been going on in some form for the last year, with a major milestone being the migration from Bugzilla to JIRA. The migration of the site proper however was nowhere as smooth as many people, including myself, desired. One of the root causes for the bumpy migration was my fault actually, when I sent an email claiming that the site's content was at a state where it should not be a blocker to migration. What I should have stated was that the site's static content should no longer be considered a blocker, as the newsletters and news items were either not migrated over or migrated over incorrectly. Translations were also completely missing and translation procedures had not been hashed out yet. Aleksey Bragin however interpreted my email as an indication that the new site was ready and switched over about two weeks before I was expecting it to happen. This resulted in a mad rush to reimport the news items while also trying to work out what permissions translators needed to allow them to do translations. A variety of other issues crept in, ranging from login issues to high site latency, that have been or will be addressed. Static content also still need fleshing out, mostly in the form of screenshots for specific graphical tools and filling in a few holes in instructions and broken links. The old site however is still accessible so no content was actually lost and translators can pull in translations for newsletters and the like from there. Significant amounts of the static content however were either written from scratch or heavily modified, so there is still a fair amount of work to do. If there is a lesson to be learned from this migration, it would probably be, triple check that everything is actually ready before initiating a major migration. Here's hoping the project will not need to go through another such migration for many, many years.

<h2>Visual Studio</h2>
Being able to develop and build ReactOS in Visual Studio has been long wished for by the developers. The first major step to achieve this was accomplished with the migration to CMake and its ability to generate solutions. CMake's support for some functionality needed by ReactOS however was missing and Amine Khaldi took it upon himself to both patch CMake itself and go through the CMake files that describe ReactOS' source code to make sure the correct build and link flags were used and preprocessed assembly code and resource files were handled correctly. The result is the ability to build an install CD that will boot to the shell and even debug user mode components using Visual Studio. There are a few C++ components that do not yet compile and resolving that will likely involve some code changes. Hopefully soon it will be possible to remotely debug ReactOS itself using Microsoft's toolchain.

<h2>Dependencies</h2>
Another major achievement by Amine and one that truly deserves praise was the effort he made to clean up dependencies amongst ReactOS' various components. Previously, attempting to modify any header file in ReactOS would more often than not result in a complete rebuild of the entire project. This was due to headers being indirectly dragged in due to their inclusion in other headers with the end result being source files would pull in far more header files than they really should. Not only does this make incremental builds next to impossible, it produces a heavier load on the machine due to needing to read more files from disk and into memory, resulting in an overall slower build. Amine manually searched through the various components, separating out headers into discrete units and only including those that a component truly depended on. The end result is a much faster build that uses less memory and causes less disk I/O as well as the ability to do incremental builds instead of constantly having to rebuild the operating system from scratch. The cleaned up dependencies also should make it much easier for Visual Studio to build its IntelliSense database.

<h2>Boot Modes</h2>
Hermès Bélusca-Maïto spent some time recently sorting out correctly booting ReactOS into the variety of safe modes it is supposed to support. Most people that have ever had their computers crash or shut down on them will likely remember the menu that comes up where Windows asks if you wish to boot into one of the safe modes because of the unclean shutdown. What functionality gets turned on is dependent on what arguments are passed to the kernel by the bootloader. In addition to the baseline options, there is an additional options menu where people can select what functionality they want enabled. It is possible to choose some options, exit, and then go back into the menu if one changes one's mind. In ReactOS however, selected options were simply concatenated onto a string. Going back to select different options would not overwrite the previous string, meaning the kernel would be passed multiple boot parameters of which only the first batch would be processed. Hermès changed the way the bootloader stored selected options and it is only when the user finally chooses to boot the OS that the string holding the arguments gets constructed, resolving the above problem.


<h2>VPC Serial</h2>
VirtualPC's usage was never very high when compared to the other virtual machine software out there, but a few ReactOS developers continue to play around on it. One major issue however was the inability to get debug output from VPC as ReactOS failed to detect its serial port. Hermès also looked into this problem, finding that the test ReactOS used to detect serial ports did not work on VPC. He implemented a new test that would work on VPC in addition to other VM platforms, though he preserved the old test in case problems arise in the future. An interesting data point would be whether Linux could detect the serial port on VPC, as that would help indicate whether there was a problem with VPC's serial emulation or if ReactOS' test for serial ports is not truly universal.

<h2>CSRSS</h2>
Alex Ionescu originally started a cleaner implementation of CSRSS, the user mode side of the Win32 subsystem. This implementation however is not yet fully plugged in, and Hermès decided to see what needed to be done. The first was to update the various system DLLs like kernel32 and ntdll to work properly with the new CSRSS implementation. As the infrastructure for proper communication between the DLLs and CSRSS did not exist previously, the current method by which the DLLs exchange data with the CSRSS is not compatible with the newer and more correct CSRSS implementation. Another item that Hermès is working on that heavily relies upon CSRSS is the console. One thing he noticed was the inability to control settings like text and background color, which he traced to a problem in setting and retrieving console configuration values in the registry. Quite a bit of work remains before Alex's CSRSS can actually be turned on in trunk.

<h2>Desktops</h2>
When Giannis Adamopoulos noticed that Eric Kohl was working on the lock workstation dialog, he saw that the actual functionality to lock a desktop was missing, which turned out to be trivial to implement. In addition, Giannis has made great strides in implementing the ability have multiple desktops. This form of multiple desktops is very different than the virtual desktops many users coming from Linux-centric desktop environments may be familiar with. In Windows, multiple desktops act as a layer of security and isolation and are employed to help ensure proper separation of multi-user environments. The next step is to make CSRSS aware of the existence of multiple desktops, as right now attempts to create consoles always results in them being created in the default desktop, which will involve some coordination between Giannis and Hermès. While there is more work to be done, Giannis achievements to date have been very impressive and deserving of any praise that the community would like to heap upon him. Many modern applications that are more security conscious have begun making use of desktop objects to create sandboxes and for them to work, ReactOS will need a proper desktop implementation.

<h2>A Reflection</h2>

When the newsletters first started, they served as very brief and simple summaries of development that had happened, often times simply collating SVN logs to show what parts had been worked on. When I first joined, the newsletters had grown a bit and discussed specific work by developers, though the sections were still fairly sparse on details. Today in addition to short summaries, newsletters also have sections that focus heavily on a few specific improvements and try to lay out both the work done and background explaining why the work was necessary. This change reflects both my own improved understanding of ReactOS and the direction where my interests lay with respect to ReactOS development. Several problems arise however. First, the time and effort it takes to prepare such detailed sections is significant. Readers can attest to the fact that the gap between newsletters has grown longer and longer, even if they enjoy some of the more in-depth sections. Second, and this is an extension of the first problem, the number of in-depth sections a single newsletter can have is limited. There is simply not enough time to write more than one or two in-depth sections without delays of a month or more. As such, the amount of development actually covered in the newsletters shrinks accordingly, with other news either being deferred and becoming out of date or never getting mentioned. Finally, my own interests have considerably narrowed with respect to what I wish to explore inside ReactOS and what I believe best serves the community.

It is from those points that I am going to slightly shift the way I present information to the community. First and foremost, newsletter sections like that detailing the implementation of the new PSEH library will no longer happen. Future newsletters will be much briefer in discussing individual developments while hopefully not leaving out important background in the process. Hopefully with shorter sections, newsletters will be released on a more regular basis and cover more of what is happening in ReactOS development. Second, I intend to produce an additional series of updates separate from the newsletters. These will likely be much longer, much more technical, and might well take much longer to produce as well. These articles will take a much deeper dive into ReactOS code itself, highlighting not just development but the current state of the code and some of the tricks behind their implementation. It is my hope that these deep dive articles can also serve as an introduction to programmers who are not yet experienced enough to walk through ReactOS' code base alone, but can self-teach themselves if the appropriate hints are given. And my sincerest condolences to the translators that will have to figure out how to translate such technical pieces.