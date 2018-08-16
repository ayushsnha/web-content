---
title:       "NTFS Write Support GSoC - Week 6"
author:      "coderTrevor"
type:        blog
date:        2016-07-06
draft:       false
promote:     false
sticky:      false
url:         /blogs/ntfs-write-support-gsoc-week-6
aliases:     [ node/12970 ]

---

<p>This week was rewarding, because I got some things done that I&#39;ve been wanting to do for a while.</p><h3>Assigning Clusters</h3><p>I wrote some code which can assign clusters by updating the $BITMAP file. As I mentioned last week, this is half of the equation for extending the allocation size of a non-resident file. I&#39;m still working on the other half, which involves storing the assigned clusters in data runs.</p><h3>Resident Eval</h3><p>I realized that modifying the data runs would involve making the same changes I needed for resident attributes, so I focused on making these changes during the second half of the week. I was finally able to implement overwriting resident files, the feature that I&#39;ve left as TODO since I started.</p><h3>Discovery!</h3><p>The most interesting bug I encountered during development involved failing to align the attribute end marker to an 8-byte boundary. When I wasn&#39;t doing this, Windows would say the file was corrupt and would refuse to read it. This problem took less than a day to find and fix. Finding it just involved visual inspection of the file record in WinHex.</p><p>When I did this, I found the kind of thing that really interests me: something undocumented! At least, I can&#39;t find any accurate explanation of this number in any of the documentation I have. After the 0xFFFFFFFF which indicates the end of the attribute list, is this 32-bit number:</p><p><img src="/sites/default/files/imagepicker/49142/File_Record_End.png" alt="File Record End Number"  class="imgp_img" width="680" height="583" /></p><p>Forensic Computing A Practitioner&#39;s Guide by Tony Sammes and Brian Jenkinson at least acknowledges the presence of this number, but this book has the only mention I can find. Curiously, the authors refer to it as a CRC when it obviously isn&#39;t, since it comes up unaltered in all but the system-reserved MFT entries.</p><p>Is this number important? What does it mean? Does Windows pay attention to it at all? I consider these interesting questions but for now, I&#39;m taking a &quot;when in Rome&quot; approach which means putting the number there when I change the file record's size, as Windows does. In the future I may perform some more experimentation and searching into what this number does and when it isn&#39;t there, but it&#39;s almost always present at the end of a file record:</p><p><img alt="Search results of magic number" class="imgp_img" height="337" src="/sites/default/files/imagepicker/49142/Search_Results.png" width="682" /></p><p><br><br><h3><a href="https://www.reactos.org/forum/viewtopic.php?f=2&t=15599">Discussion</a></h3></p>