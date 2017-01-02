---
layout: post
title: Getting to know diffoscope better
---
I apologize to all potential readers of this post for not writing a comprehensive "Introduction" post with details of the project I am taking part in during my internship, as well as some story about how I ended up there.

Let me just say that I was a Debian fan for years when I discovered it is taking part in Outreachy as one of organisations. Their [Reproducible Builds](https://reproducible-builds.org/) effort has a noble goal and a bunch of great people behind it - I had no chances not to get excited by it. Looking for a place where my skills could be of any use, I discovered **diffoscope** - the tool for in-depth comparassion of files, archives etc. My mentor, Mattia Rizzolo, supported my decision, so now I am concentrating my efforts on improving diffoscope.

As my first steps, I am doing small (but hopefully still somewhat important) job of fixing existing bugs. It helps me to better understand how diffoscope works, as well as introduces me to the workflow of opensource development.

During December, I have done several small contributions, mostly fixing bugs.

### Test data and jessie-backports ###

First of them could be somewhat called cleaning up after my own mistake, although that mistake wasn't trivial. During the application period, I have fixed a bug with diffoscope failing while comparing symlinks to directory. That was a small change, but I included some tests for that case anyway.

...And that actually caused problems. With these tests, I included test data: two folders with symlinks. All was good in unstable version of Debian, but in jessie-backports, that commit caused build to fail. After some digging, I discovered the problem was caused by build process including copying that data. That was done using *shutils* Python module, and older version of that module, included in jessie, could not handle copying symlinks to directory properly.

Thanks to my mentor for giving me a hint on how to resolve this: using temporary folders and creating these symlinks at runtime. That way, we ensured tests run without problems during build process on jessie.

**What have I learned:** A great deal, actually. I spent too much time on that one, but I learned how to build packages, what happens during *dpkg-buildpackage* run and what *debhelper* tools are for. I also learned a bit about what *chroot* is and how to use it for testing.

### ICC profile files and file type recognizing regexp ###

Another one was also about failing tests and, therefore, failing build. Failing tests were all due to ICC files were not recognized by diffoscope. Turned out libmagic got an update which changed the description of ICC profile files. Diffoscope was relying on regexp applied to file type description to recognize the file, so I changed regexp to reflect the changes in libmagic.

**What have I learned:** How diffoscope "recognizes" file types. Got me thinking: maybe there is a better way? That regexp-based approach is doomed to cause problems with every file type description change. I have this question still lingering in my mind - maybe I will come up with an idea later.

### Order-like difference in text files ###

Next, I decided to do something a bit bigger and fullfilled a feature request. That request was for detecting order-like difference in text files (when files has the same lines, but in different order). I did it by collecting "added" and "removed" lines in *diff* output in lists, sorting and then comparing them.

Sadly, I forgot about one particular case - when one of the files is missing the newline at the end of file. I was kindly reminded of that quite soon in comments on the bug-tracker (thanks *danielsh*!) and have already fixed that.
I also recieved feedback on how better implement it deeper in the diffoscope - not using the results of diff, but rather comparing sum of hashes of the lines directly in the difference module. I am yet to try that.

**What have I learned:** That a call to *diff* is actually the slowest part of the diffoscope run when done on two big text files. Could it help somehow in speeding it up? I don't know yet.

I also learned to comment on bugs in Debian bugtracker and was surprised by how much feedback I got. Thanks to my mentor for pushing me to do that - I definetely need to overcome my fear of communications to be more effective!

### Random FTBFS ###

There was also a very nasty bug that caused diffoscope to [fail to be built from source randomly](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=848403), failing with non-informative *Fatal Python error: deallocated None*. It already seemed strange when it was first reported; It got only more strange when suddenly that bug ceased to be reproducible. We hoped that would mean that bug was caused by some external tool, and was fixed there. Turns out it was not that easy. I tested this on two separate computers and on virtual machine; I used different versions of diffoscope. Well. Seems like that bug is still somehow tied to diffoscope version and not some external tool version - I still can do git checkout 64 and be able to reproduce the bug (still randomly, though).

Although I spent quite a lot of time on that one, the only result was the information about connection between bug apperances and diffoscope version. I still wasn't able to get to the root of the problem - hopefully, someone else will be able to, given the information I found.

**What have I learned:** *git-bisect*! Thanks to my friend for pointing me to it, that tool came handy in that situation. Also, got some experience in catching nasty bugs like that (pity that no experience in squashing them).

I had some extra time commitements in December, one of them (Reproducible Builds Summit II) connected to my internship and one (my exam session in university) not. In January, I should be able to allocate more time to that work - I hope it will help me achieve more significant results.

Many thanks to Mattia Rizzolo, Chris Lamb, Holger Levsen and all other folks of Reproducible Builds project - I cannot stress enough how important your support is to me.

Wish you all a great 2017!
