---
title:       "Newsletter 82"
author:      "Z98"
type:        news
date:        2011-04-09
changed:     2013-02-23
draft:       false
promote:     true
sticky:      false
url:         /newsletter-82
aliases:     [ node/223 ]
news:        [ "newsletter" ]

# Summary:
# <ul
# <li>NewCC Status</li>
# <li>Patch Auto-Tests</li>
# <li>CLT Followup</li>
# <li>New Developers and Contributors</li>
# <li>Links</li>
# </ul>

---
<h2>NewCC Status</h2>
<p>A while back Art Yerkes merged in a significant part of his new common cache implementation into trunk.  This merge was intended to lay the foundation for the eventual replacement of the current common cache, which will happen once Art clears up some inconsistencies in how the sections data structure is treated by the memory manager and the common cache.  Sections management is the responsibility of the memory manager but provide functionality the common cache relies on.  The old Cc had the tendency to reach in and directly manipulate sections directly instead of asking the memory manager for the resources it needed.  This was extremely brittle and the difficulty of untangling this interdependency is one cause for the delay in integrating NewCc.  This brittleness is currently manifesting itself in problems with ReactOS crashing while running unit tests, an issue Art wants to resolve before the next merge into trunk.</p>
<p>Conceptually, the common cache is supposed to ask the memory manager for resources to use in order to cache files.  It receives these resources in the form of section objects and is not supposed to make any assumptions about the underlying pages that make up the resources the memory manager provides.  The section object mentioned above and the code for interacting with its members was rewritten to avoid NewCc trying to manipulate it directly, as this created a circular dependency with the memory manager suddenly needing the common cache to help maintain a data structure that it was supposed to provide to the common cache.</p>
<p>Another limitation in the old Cc implementation has to deal with mapping offsets.  This limitation is actually filesystem dependent as the layout of a filesystem's datastructure on disk dictates whether one encounters the problem.  Specifically, the old Cc could not map offsets greater than 4GB.  For filesystems like FAT that keeps all of its data structures at the start of a disk, running into this problem required having a partition large enough that more than 4GB of filesystem metadata was needed.  For something like ext3 that spreads file metadata across the disk, just having a partition larger than 4GB will break things.  NewCc resolves this issue and Art used Matt Wu's ext3 driver to confirm the fix.  Art's branch is actually capable of installing and booting on an ext3 formatted partition, something many people have asked for in the past.  This and other improvements will start benefiting the rest of us once the crash exposed by the unit tests are resolved and Art does the next merge of his code.</p>
<h2>Patch Auto-Tests</h2>
<p>One of the issues with ReactOS development is the time it takes to build from scratch and run tests.  The buildslaves the project maintains takes approximately 10 minutes to do a clean build and another hour to run tests.  Even the developers who have access to comparable hardware tend not to want to tie down their machines for an hour just to make sure a commit is free of regressions.  While the buildslaves will automatically test all commits made to the repository, the team prefers that regressions never pollute are caught early instead of forcing testers to waste time hunting for them hundreds of commits after the fact.  As such, Olaf Siejka put together a new builder system that takes patches and applies them to the latest revision in trunk and issues a build and test.  Getting the system up and running was remarkably troublesome due to the lack of decent tools for applying patches on Windows, or more specifically accommodating the behavior of the patching tools like refusing to work without DOS line endings in text files.  The system however is now up and running for use by developers though the interface is still a bit cumbersome.  Developers need to submit patches as attachments to bug reports in order for the patch tester to see them as direct submission has not been implemented.  Olaf is also making tweaks to result publishing and comparison with the automated regression testing for actual commits.  VirtualBox, the virtual machine software being used for the patch auto-test system, also has a poor history of supporting serial output, complicating dealing with test results, though the regression tests also suffer from this issue.  Future work will focus on making the system easier to use but for the time being ReactOS developers now have an additional tool to help them and one less excuse for committing patches that break functionality.</p>
<h2>CLT Followup</h2>
<p>Several members of the ReactOS German contingent once again attended Chemnitzer Linux-Tage this year, showing off ReactOS and giving out approximately 170 CDs for people to try.  Over two thousand people visited the convention so it was a great place to demonstrate ReactOS.  The rush to get 0.3.13 out the door was mostly so that the team would have an updated release to demo and those that attended were able to get a copy of the release before its general release.  According to the attending developers, the people who knew about ReactOS and those that did not were about split even.  The developers fielded questions and even had the chance to chat with IRC regulars while there.  The amusement highlight of the trip would probably be all of the convention presenters like the ROS team sleeping in a gym in order to save money.  The "I wish I had been there" highlight is probably the free food they got, which Timo Kreuzer took the time to gush about.  Photos taken by the developers can be seen here.  Links to both the versions of ReactOS released at CLT and pictures are located at the bottom.</p>
<h2>New Developers and Contributors</h2>
<p>Olaf Siejka has long been one of the primary testers before working with Colin Finck to put together the current batch of build and test machines.  He actually gained commit access a while ago, though Olaf apparently missed the message from Colin notifying him of this and it was not until Aleksey Bragin went to do the same thing that this oversight was noticed.  Besides helping with the new build systems, Olaf has taken up some of the burden of committing translations, merges, and various other miscellaneous patches.  Rafal Harabień is the other newcomer and before gaining commit access was producing patches at a prodigious rate.  He has mostly been crawling through the source code to find bugs wherever he can.  He has already squashed several long standing bugs so hopefully the turnaround time for future reported issues will decrease significantly with his help.</p>

<h2>Links</h2>
<p class="icon_p">
  <a href="http://downloads.sourceforge.net/reactos/ReactOS-0.3.13-CLT2011.7z">
    <img alt="" src="http://reactos.org/media/pictures/2007/livecd.png"><br>
    <strong>ISO Image of the ReactOS CD giveaway</strong>
  </a>
</p>

<p class="icon_p">
  <a href="http://flickr.com/photos/tags/clt2011">
    <img alt="" src="http://reactos.org/media/pictures/2006/screenshot.png"><br>
    <strong>CLT2011-tagged photos at Flickr</strong>
  </a>
</p>