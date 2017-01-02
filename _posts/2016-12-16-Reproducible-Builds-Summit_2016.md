---
layout: post
title: Reproducible Builds Summit 2016
---

This is the second week of my internship period for the Reproducible Builds project of Debian, and its is very fortunate that exactly then I was starting to dig more into the project, I got the opportunity to take part on Reproducible Builds Summit II, taking place in Berlin. People from different projects who take part in reproducible builds effort, gathered here to discuss their vison of the project, share knowledge and brainstorm ideas that would benefit us all.

For me, it was the invaluable chance to learn a lot about the project, hear from people who had worked on it from the beginning and those who use it.

Most part of the summit we spent in sessions - talks, discussions or hacking sessions made in a group of 3-8 people. Topics discussed varied from definition of reproducible builds to bootstraping, from writing documentation to .buildinfo files. In every session, we were discussing the important concepts, identifying problems and looking for ways of resolving them. 

One of topics that got the most of my interest was .buildinfo files, ways of collecting them and using them for reproducibility checks.
The idea behind .buildinfo files is the need to record all the useful information about the particular build process: what source files were used, what was the compiler flags, enviroment variables etc. during the build, as well as hash of the resulting artifact(s). It is expected that comparassion of these files will help to identify if the software builds reproducible, and, if not, what affects the build.
As useful as they are, the use of .buildinfo comes with several questionsand ideas:

 * To actually make them useful to users (or, most likely, developers), we will have to publish them somewhere and, ideally, integrate them into packaging system. That would allow us to check if the package has reproducibility issues. But how to ensure it? There were ideas like having "trusted builders" and check if there are any buildinfos signed by them, or just accept the package if it has enough "right" buildinfo.
 
 * What should and should not be included in the .buildinfo files? From the one hand, we want them to record as little information as it is necessary: only what is really necessary to reproduce the build. On the other hand, the reality is that there is stillplenty of non-reproducible software, and that software can capture different things it should not really capture during the build: timestamp, buildpath, locale, enviromental variables. To effectively debug these cases, we will need much more info, so it is wise to include as many information as it is safe to publish in the buildinfo files. Essentialy that boils down to whether main purpose of buildinfo files is to ensure reproducibility or debug unreproductibility.

* Right of revocation. We are planing to publish buildinfo files, make assumptions based on them, probably have some concepts of "trusted builders". But it is possible that we will face the need of making different statements about state of the package. It can be something like "I was able to reproduce the software yesterday, but today the building process gives different result" or "I submited the .buildinfo, but then I found a problem with my test environment - please ignore my last submission". Whatever the case, we cannot just remove the uploaded buildinfo file; instead, we should submit the newfile with different results. But that situation has to be handled correctly by the system that uses these files for reproducibility checks (that one got a really interesting discussion on the binary transparency session; we found this problem resembling certificate transparency issue).

 * We want to include everything that is affecting the build into buildinfo files, but there is a obvious limit to what information can we safely publish. In theory, sensitive information like the hostname or number of CPU cores should not affect the build, but in reality, that can happen. Should we make this information public and, if not, how to handle its absence?


It is worth noting that .buildinfo files were actively discussed even in the sessions that were, at a first glance, not connected to them. It shows how important the idea is and how many applications can it have.

Of course, I also paid a lot of attention to discussions around diffoscope, the tool I am going to focus my efforts on in the following several months. It got pretty much attention and it was inspiring to see how useful it is to many different projects. There was a session on that we discussed the future of diffoscope, what people expect of it and what features should be added. That gave me several new ideas on what should I try to do to improve that tool. There were also quite a lot of progress done in-between the sessions, during the hacking times. Sadly, I was not part of that progress, although now I am set to fulfilling the feature request for markdown output.

In general, I am very grateful for all the participiants for being friendly and open for all sort of question and discussion. I often had to ask people to explain some concept that I did not know, and they always took the time to explain it to me. The community of Reproducible Builds project is really great and welcoming!
Of course, that also reminded me of how little I still know. I wish I came here more prepared and was able to add something up to the effort. I do hope at least my questions were useful in fueling up the discussion and seing the problem from the different angle.

I tried to tell about what I saw and what I learned in a clear, meaningful way, but, in fact, I am just super-excited now, happy to meet all these great people and motivated to work on the project and be a part of community. Everything was awesome about these three days, and all I saw here inspired me greatly.
My sincere thanks to all organisators and participiants of this event!
