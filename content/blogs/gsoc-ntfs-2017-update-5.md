---
title:       "GSoC NTFS 2017 Update 5"
author:      "coderTrevor"
type:        blog
date:        2017-07-13
draft:       false
promote:     false
sticky:      false
url:         /blogs/gsoc-ntfs-2017-update-5
aliases:     [ node/46976 ]

---

Last week I got back to writing B-Tree code. Sadly, the week flew by before I could come up with anything screenshot-able.

I've been updating the B-Tree code to accommodate index allocations, and trees of arbitrary depth. I'm mostly done with this but I still have to finish the code that saves an index buffer to the index allocation. Once I do that, I should be able to demonstrate creating dozens of files in a directory. I got so close but fell a little bit short of that last week. After I get that working, it should just be a "little bit" more code and effort to support creating all the files you'd ever want or have space for.

I hope to show you some nice screenshots next week! :)