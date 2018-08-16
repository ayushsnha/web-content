---
title:       "ReactOS Repository migrated to GitHub"
author:      "Colin Finck"
type:        article
date:        2017-10-03
draft:       false
promote:     false
sticky:      false
url:         /project-news/reactos-repository-migrated-github
aliases:     [ node/53513 ]

---

<img src="/sites/default/files/imagepicker/1249/thumbs/GitHub-Logo.png" alt="GitHub Logo" style="float: right; margin: 20px" class="imgp_img" width="400" height="105" />
<p>Today, the ReactOS Source Code has been migrated from a central Subversion instance to a decentralized Git repository. Together with that, ReactOS joins the list of projects using the popular <a href="https://github.com/reactos/reactos">GitHub</a> service for developing software. We expect that this move greatly improves the way we collaborate on ReactOS development and reduces the barriers for newcomers. Just fork our repository on GitHub, commit your changes and send us a Pull Request!</p>

<p>Migrating a source code history of more than 20 years that had seen multiple version control systems was not a straightforward task. Deciding on a decentralized version control system has not been either. First discussions already started <a href="https://reactos.org/pipermail/ros-dev/2009-January/011035.html">back in 2009</a>, when neither Git nor Mercurial were able to fully convert a large SVN repository like ours and Git’s Windows support was still neglected. Things improved massively over the years, with GitHub and Git for Windows emerging as reliable tools for software development. But the ReactOS Project still took advantage of some Subversion features, so only a smooth migration using a two-way SVN-Git mirror was attempted <a href="https://reactos.org/pipermail/ros-dev/2016-May/017845.html">in 2016</a>. This failed miserably, however important lessons were learned for a future complete migration to Git. The tipping point was reached <a href="https://reactos.org/pipermail/ros-dev/2017-February/018111.html">in early 2017</a> when a majority of ReactOS developers spoke out in favor of moving to Git. Finally, the ReactOS Hackfest in August offered a forum to try out things and discuss every little detail of the planned migration. And this is what got us here today!</p>

<p>The development documentation is still in the process of being rewritten to account for the Git migration. You may currently find outdated information here and there. However, most of that is on the Wiki, so you are more than welcome to help us!
The SVN Repository has been turned read-only and will be kept online for a while at the last revision r76032. Our <a href="https://git.reactos.org/">Git mirror</a> now mirrors the GitHub repository. If you have already been using our old Git mirror, please note that you have to do a fresh clone of our new repository (from either GitHub or the mirror) as the old and new ones are incompatible.<br>
<a href="https://jira.reactos.org/">JIRA</a> continues to be used for bug tracking, <a href="https://build.reactos.org">BuildBot</a> for continuous integration, and <a href="https://code.reactos.org">FishEye</a> as a code browser.</p>

<p>I would like to thank all the people who have helped with this migration, be it on IRC, the mailing lists or at the Hackfest! Special thanks also go to the KDE Project for their excellent <a href="https://github.com/svn-all-fast-export/svn2git">svn-all-fast-export</a> tool that was used for the conversion. If you are ever in a similar situation, have a look at my <a href="https://github.com/ColinFinck/reactos-git-conversion-scripts">conversion scripts</a> as well as the <a href="https://svn.reactos.org/project-tools/trunk/git-tools/">Git helpers for our infrastructure</a>.</p>