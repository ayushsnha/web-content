---
title:       "Newsletter 29"
author:      "Z98"
type:        news
date:        2007-07-21
changed:     2013-02-23
draft:       false
promote:     true
sticky:      false
url:         /newsletter-29
aliases:     [ node/169 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>ReactOS 0.3.3 RC1</li>
# <li>Shell Work</li>
# <li>Buildbot Down</h2>
# <li>ReactOS Build Environment 0.3.7</li>
# <li>Congratulations Steven Edwards</li>
# <li>State of the Newsletter</li>
# </ul>

---
<h2>ReactOS 0.3.3 RC1</h2>
<p>The release candidate was more or less a success, far more than what many of us expected.  When bug reports started coming in from people running it on real hardware, several of us, including myself, were quite surprised.  Having a stabilized kernel definitely helps.  The RC was released for several reasons.  First, ReactOS suddenly got a surge of attention over the past two weeks, resulting in people going and downloading our latest release.  That release was 0.3.1, one of the worst in memory.  Aleksey Bragin decided we needed to get another release out to avoid people getting the wrong impression about our progress.  As trunk was in very good shape, it was branched and quickly released.  One has only to check the Source Forge statistics for ReactOS to see how much traffic we've generated.  The feedback has also been very helpful and several bugs have been worked out.<span style="font-weight: bold;"><br/>
</span></p>
<p>Probably the foremost bug was the need to move your mouse in order to load a website in Firefox.  This bug was squashed very quickly, though some people in IRC jokingly said they would miss it.  The moving icons on desktop was also fixed, along with a few other drawing bugs.  But perhaps the biggest news is it is now possible to install Firefox 2 in ReactOS.  However, this requires a minor workaround as the installer is still running into an odd permissions bug.  While running the install, FF is likely to complain about any number of folders.  We know for sure that the chrome folder must be created manually.  Before installing Firefox 2, create the Mozilla Firefox folder in Program Files and then create the chrome folder inside that.  However, if the installer complains about others, you'll have to create them yourself and rerun the installer.  This workaround is only for the English language locale.  Others have tried this on different language settings and the result was a mess.  As a side note, Firefox 1.5 does not have this issue.</p>
<h2>Shell Work</h2>
<p>With a stabilized kernel, Thomas Weidenmueller has begun working on the shell components.  Many people have noticed the explorer-new folder in SVN and some have asked how they can use it.  Right now, they can't because ReactOS doesn't have a complete enough shell infrastructure to run that code.  His work was also partially responsible for getting the Firefox 2 installer to where it is today.  Besides the shell, Thomas has also been fixing various compiler warnings and other bugs.</p>
<h2>Buildbot Down</h2>
<p>Something recently happened that brought down the web, SVN, and build master servers.  This resulted in a short period of time when everything went down, but the site and SVN are back up.  While the machine that actually builds the SVN images (build slave) is up and running, without the build master, it's just sitting there.  Unfortunately, Aleksey Bragin's the only one with direct access to the machine and until he gets back from vacation, we're stuck.  Right now any build is being manually built and uploaded.</p>
<h2>ReactOS Build Environment 0.3.7</h2>
<p>Fortunately, the ReactOS Build Environment has stabilized and RosBE 0.3.7 for Windows NT has been formally released.  This BE makes use of mingw 4.1.3, binutils 2.17.50, nasm 0.98.39, and w32api 3.9.  The BE can be found on this <a href="http://www.reactos.org/wiki/index.php/Build_Environment">page</a>, though the UNIX version hasn't been updated yet.  Also, the GCC compiler included is only for C and C++.</p>
<h2>Congratulations Steven Edwards</h2>
<p>Steven is a former developer for ReactOS and acted as our liason to WINE.  Though he's not actively involved with ReactOS anymore, he does keep us apprised of WINE's activities.  This sometimes is not an enviable job, but he's a good sport about it.  Anyways, Steven recently got married to his high school sweetheart and we of the ReactOS team wish them the best.</p>
<h2>State of the Newsletter</h2>
<p>Samuel and I haven't been doing a very good job with the last few newsletters.  Most of the problem is us writing the pieces the day of release.  I actually took the time to work out this article the day before release and its quality is much better compared to several previous efforts.  Still, some of it revolves around us not really knowing what to write.  The focus of the newsletter is to inform people what's been happening in the ReactOS community and how development is going.  However, the ROS community is a small one compared to other projects and the number of developers is even smaller.  As such, sometimes we really don't have much to say.  For a time, the two of us toyed with doing more indepth looks at certain components, but neither of us are familiar enough with the components in question to write about them.  The newsletter will continue since it does serve as a way to pass on interesting developments, but we'll likely par down our own expectations of how much we have to stuff in it.  That'll hopefully turn this newsletter back into what it was meant to be and open up other ways of conveying more technical information.