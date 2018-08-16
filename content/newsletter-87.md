---
title:       "Newsletter 87"
author:      "Z98"
type:        news
date:        2011-08-30
changed:     2013-02-20
draft:       false
promote:     true
sticky:      false
url:         /newsletter-87
aliases:     [ node/228 ]
news:        [ "newsletter" ]

# Summary:
# <ul>
# <li>Google Summer of Code Progress</li>
# <li>Kernel-Mode Test Suite</li>
# <li>lwIP Integration</li>
# <li>Themes</li>
# <li>Font Driver</li>
# <li>Thomas Krenn Open Source Promotion</li>
# </ul>

---
<h2>Google Summer of Code Progress</h2>
<p>Of the six projects ReactOS entered as part of the Google Summer of Code, four are well on track to completion.  Each of the completed projects will greatly help increase ReactOS' usability and provide a foundation for future enhancement.  Many of the successful projects will help make ReactOS more usable from both a stability and UI perspective.  The deadline for final submissions is already over and the project and Google are currently evaluating students.  The project would like to thank Google for providing our students with the opportunity to contribute to ReactOS and we hope to learn from our mistakes this year to increase our success rate in the following years to come.</p>
<h2>Kernel-Mode Test Suite</h2>
<p>The framework for running tests in kernel mode has been completed by Thomas Faber and he is currently wrapping up a few additional tests for the suite.  The kernel mode test suite will be capable of testing functions that are only accessible from kernel mode, generally ones that drivers rely on.  This is one area that the Wine project's test suite does not cover as their objective is to run applications, not drivers.  The ReactOS project however aims to be a drop in replacement for all of Windows and compatibility with Windows drivers is a must.  A kernel test suite will also provide presentable proof of undocumented Windows kernel behavior.  One of the questions from third parties that has always dogged the project is how developers know how certain parts of the Windows operating system behaves.  The test suite can be run against both Windows and ReactOS and the results compared.  Based on these outputs, the ReactOS developers can determine where ReactOS deviates and fix these deviations in our code.  As such, the test suite provides documented examples of our research along with a demonstrable benchmark of what the project is trying to achieve.  The next step is for Thomas to integrate the framework into the automated testing system the project currently runs.  As the tests were designed from the outset for this purpose, Thomas does not expect this to be difficult.</p>
<h2>lwIP Integration</h2>
<p>The conversion of the lwIP networking library into a driver by Claudiu Mihail has been effectively completed and the results have been merged into trunk.  While testing, Cameron Gutman found the network performance to be considerably more stable and the overall feel to be much faster.  Cameron tested a wide variety of programs, both servers and clients.  The test programs included the abyss web server, chargen, Opera, and telnetd, all of which continued to run even under heavy load.  The system eventually died due to a leak in win32k's memory pool, the cause of which has also been fixed.  Despite major difficulties initially with dropped packets and improperly handled shutdown of TCP connections, Claudiu overcame the issues and we are now benefiting from the fruits of his labors.</p>
<h2>Themes</h2>
<p>While working on implementing theme support, Giannis Adamopoulos learned that Microsoft indirectly used side by side classes for button and control themes.  SxS classes are a variation of the SxS assemblies more people are aware of, a mechanism used by Microsoft to provide backwards compatibility shims for various libraries and the like.  Giannis is unsure if anyone besides Microsoft even makes use of SxS classes, as it is remarkably esoteric.  It effectively allows two classes of the same name to be present in an application, with a manifest file informing the operating system which of the two classes it should use.  While ReactOS recently added support for SxS assemblies, it does not yet have support for SxS classes and some more work will need to be done.  As such, Giannis was forced to create a hack in ReactOS to permit the existence of two classes with the same name.  One version of the class is the default, draw with no themes class.  The other is the class that tells ReactOS how to draw a particular theme.  Despite the complications this development caused, Giannis has been able to get a mostly working theme system implemented and produce a ISO that testers have been putting through the paces.  If all goes well, the next ReactOS release may well see theme support.</p>
<h2>Font Driver</h2>
<p>Timo Kreuzer had little difficulty getting the font driver to a working state on Windows XP, though more remains to be done before it will run on ReactOS.  For his driver, Timo used the freetype font engine as a basis, but freetype uses a different hinting system than Windows.  Font hints are used to help improve rendering to make text look nicer onscreen.  As a consequence, while ReactOS will be able to actually use hints provided in font files once the font driver is merged in, fonts designed for use in Windows may look a bit blurry compared to fonts tailored to the freetype engine.</p>
<p>As mentioned above, development of the driver took place on Windows XP due to ReactOS missing the mechanisms to actually make use of the driver.  Timo's Summer of Code project was limited to just producing the font driver, but he has also been working on getting ReactOS ready.  Currently the driver loads and is also able to load font information into memory and creating a table to organize them.  Realization, rendering of the glyphs into bitmaps, and a version of the TextOut function that can actually use the rendered glyphs are still missing.  Realization involves querying the font driver for information about the font and font mapping, the linking a logical font to a physical font, the information received by the font driver plus some additional bits and pieces.  Rendering of the glyphs is the responsibility of the driver, but the OS needs to provide some mechanisms to help manage the memory where the bitmaps are stored.  For TextOut, please refer to <a href="../en/newsletter_55.html">newsletter issue 55</a> for an explanation of what is needed, along with a more detailed explanation of what a font engine requires.  Completion of this work will likely take a few more months, though the font driver itself is effectively finished.</p>
<h2>Thomas Krenn Open Source Promotion</h2>
<p>During the LinuxTag convention in Germany, Matthias Kupfer registered the project for a contest run by server provider Thomas Krenn AG.  The contest prize was money that can be put towards purchase of a server system from the company, and the ReactOS project ultimately placed 5th and has received 700EUR.  The project intends to use the money to purchase a new build/test server, which will help alleviate the load on the current machines.  The project would like to thank Thomas Krenn AG for running the contest and their support for the open source community.</p>
<p>&nbsp;</p>