---
title:       "Newsletter 20"
author:      "Z98"
type:        news
date:        2007-03-18
changed:     2013-02-23
draft:       false
promote:     true
sticky:      false
url:         /newsletter-20
aliases:     [ node/161 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>A Third Introduction</li>
# <li>Reactions to 0.3.1</li>
# <li>WinDBG</li>
# <li>DirectX/win32k/ReactX</li>
# <li>Website Translations</li>
# <li>Sound</li>
# <li>Events in SVN</li>
# <li>Bugzilla</li>
# </ul>

---
<h2>A Third Introduction</h2>
<p>Good day to all. The name's Z98 and I'm one of two release managers for ReactOS. The source files and QEMU images were generated by me, so if there are any mistakes, you know who to complain to. Today I wrote most of the newsletter and I will be helping samwise52 in the future.</p>
<h2>Reactions to 0.3.1</h2>
<p>Reactions have varied in regard to 0.3.1, though one response was consistent. The difficulty in getting it to work on real hardware. As mentioned many times, 0.3.1 was branched in the middle of a kernel rewrite. The resulting instabilities weren't totally unexpected. Also, there was supposed to be alpha blending in 0.3.1, but apparently someone broke that and no one noticed before the release. Still, 0.3.1 drew a lot of publicity from many people and sites, which helped to raise awareness for ReactOS. One thing that I believe should be clarified is the goal of ReactOS. While officially it's stated to be XP/2003 compatibility, in reality, work is also done to make it Vista compatible. Many Vista specific implementations have already been stubbed and will also be worked on as more things stabilize. So ReactOS will not become an implementation of an outdated Windows version. It will continue to follow Windows as best it can.</p>
<h2>WinDBG</h2>
<p>Support for WinDBG was mentioned in issue #18, and we'll discuss it in more detail here. For some time, Alex Ionescu has been working on supporting the use of WinDBG in ReactOS. Most of this work was done in a branch of the source but recently they were merged into trunk. Alex has reported that it is now possible to use WinDBG to connect to ReactOS, set breakpoints, display trace information, print DPRINT output, and step through the code. The use of WinDBG deprecates KDBG, GDB, and other old settings for printing to screen and file. Still, the old debugging system will remain in place for continued usage. Also, with WinDBG, ReactOS would be using the same tools Microsoft uses to write Windows. These should help greatly in finding bugs as well as reduce some of the hidden bugs in the debug systems.</p>
<h2>DirectX/win32k/ReactX</h2>
We're slowly getting there, especially in the 3D department.&nbsp;GreatLord's use of the 0.3.1 branch while testing certain aspects of win32k for his Direct3D work tripped the PSEH loop that delayed the 0.3.1 release, which is now fixed. So far, work on DirectX is stalled because of more bugs found in win32k. One specific issue arose in creating surfaces. GreatLord's made a lot of progress on fixing that and things are improving. As these get sorted out, we'll see speed increases as well as more stability. A name was also chosen for the ReactOS DirectX clone. As many people probably suspected, ReactX or ROX(shorthand) was <a href="http://www.reactos.org/forum/viewtopic.php?t=3552">chosen</a>.&nbsp;A logo will be chosen sometime after 0.3.2 is branched, so keep on posting those ideas. <br/>
<h2>Website Translations</h2>
<p>Over the past few months, several people have asked for permissions to translate parts of the ReactOS website. At times response to those requests have been slow, creating some frustration. Frik85 has another system in the works but while he's getting that set up, I(Z98) have created a temporary measure. <br/>
<br/>
<a href="http://www.reactos.org/wiki/index.php/Website_Translations">http://www.reactos.org/wiki/index.php/Website_Translations</a><br/>
<br/>
For people who do not have translation accounts, you can post translations following the instructions on this site. That page is still a working progress, but it should at least provide a temporary solution to translation work.</p>
<h2>Sound</h2>
<p>Some of you might have noticed in the <a href="http://www.reactos.org/en/about_roadmap.html">Roadmap</a>&nbsp;&quot;Some initial support for audio.&quot;&nbsp; Well it's getting there. Developer Andrew Greenwood aka Silverblade is hard at work implementing the <a href="http://www.microsoft.com/whdc/archive/csa1.mspx">Kernel Streaming</a> APIs that are the mainstay of the Windows XP sound architecture. Although this architecture is changed in Vista, Kernel Streaming is still at its base. The major difficulties of the Windows sound stack include the use of C++ and COM&nbsp;kernel mode, needless to say filled with countless pitfalls and possible problems that will stress not only the system but also the programmers themselves. </p>
<p>This still isn't a user visible change, as your sound card drivers will still NOT work. But as ancient wisdom holds, even the longest journeys begin with a single step. This is a very important part for the future user friendly ReactOS.<br/>
</p>
<h2>Events in SVN</h2>
<p>Trunk has been incredibly unstable recently because of kernel changes and other commits. One major improvement was to allow two devices to share a driver, thus negating the need to load two drivers. This saves memory and helps increase speed. A bug in the improvement broke trunk for a while but Herv&eacute; Poussineau managed to fix it. Another major issue that's cropped up is with named pipes. For those that don't know what named pipes are, they're used to communicate between processes. They are also used to communicate with the PnP manager. Needless to say, the breakage is something of a serious problem. Whatever is causing it is also the root of the VMware problems. The devs aren't sure what the cause is, though some speculate it to be a timing issue.&nbsp; A third effect of this issue is causing ReactOS to hang for about a minute before the explorer shell is fully initialized.&nbsp; And just recently, libcntpr got broken.<br/>
<br/>
One noticeable improvement in trunk is better rendering in win32k. People that tested 0.3.1 may have noticed gaps or other inconsistencies, but these have seen major improvements.</p>
<ul>
    <li>amunger - fixed typos. </li>
    <li>cwittich - fixed some issues with the msvc backed. </li>
    <li>dcote - fsrtl lib tests. </li>
    <li>dgorbachev - committed several translation patches and several other bug fixes. </li>
    <li>ekohl - worked further on desk.cpl </li>
    <li>fireball - fixed critical bugs, fixed build issues, svn maintenance, also working to get freeloader to boot Windows 2003. </li>
    <li>greatlord - continues work on DirectX support&nbsp;and GDI. </li>
    <li>hpoussin - fixed compiler warnings, fixed build issues, fixed critical bugs. </li>
    <li>kjkhyperion - added tracing to the PSEH library. </li>
    <li>ion - fixed critical bugs in kernel exception handler, freeloader and furthered windbg support. </li>
    <li>jimtabor - fixed build. </li>
    <li>mbosma - committed work on download! scripting. </li>
    <li>silverblade - committed a large volume of work on kernel streaming and audio support, much work still remains. </li>
    <li>tretiakov - merged clipboard branch, worked on win32k. </li>
    <li>weiden - committed hello world mmc implementation. </li>
    <li>winesync - automatic merging of wine dlls. Advpack, avifil32, comctl32, cryptdll, imm32, oleacc, lz32, mapi32, objsel, olepro32, sensapi, uxtheme, mpr. </li>
</ul>
<h2>Bugzilla</h2>
<p>57 bugs were active.<br/>
23 new bugs reported.<br/>
24 marked fixed.<br/>
3 duplicate, 1 works for me, 1 later, 1 invalid.<br/>
Oldest bug marked fixed is 703, &quot;ReactOS is a fat pig. Minimum mem reqs exceeded.&quot;</p>
<p>&nbsp;</p>