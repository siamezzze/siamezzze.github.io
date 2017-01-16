---
layout: post
title: APK, images and other stuff.
---
2 more weeks of my awesome Outreachy journey have passed, so it is time to make an update on my progress.

I continued my work on improving diffoscope by fixing bugs and completing wishlist items. These include:

## Improving APK support ##

I worked on [#850501](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=850501) and 
[#850502](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=850502) to improve the way diffoscope handles APK files.
Thanks to Emanuel Bronshtein for providing clear description on how to reproduce these 
bugs and ideas on how to fix them. 

And special thanks to Chris Lamb for insisting on providing tests for these changes! 
That part actually proved to be little more tricky, and I managed to mess up with these tests (extra thanks to Chris for 
cleaning up the mess I created). Hope that also means I learned something from my mistakes.

Also, I was pleased to see [F-droid Verification Server](https://verification.f-droid.org/) as a sign of F-droid progress 
on reproducible builds effort - I hope these changes to diffoscope will help them!

## Adding support for image metadata ##

That came from [#849395](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=849395) - a request was made to compare image 
metadata along with image content. Diffoscope has support for three types of images: JPEG, MS Windows Icon (*.ico) and PNG.
Among these, PNG already had good image metadata support thanks to `sng` tool, so I worked on .jpeg and .ico files support.
I initially tried to use `exiftool` for extracting metadata, but then I discovered it does not handle .ico files, so I decided 
to use a bigger force - ImageMagick's `identify` - for this task. I was glad to see it had that handy `-format` option I could use
to select only the necessary fields (I found their `-verbose`, well, too verbose for the task) and presenting them in the defined
form, negating the need of filtering its output.

What was particulary interesting and important for me in terms of learning: while working on this feature, I discovered that, 
at the moment, diffoscope could not handle .ico files at all - `img2txt` tool, that was used for retrieving image content, did
not support that type of images. But instead of recognizing this as a bug and resolving it, I started to think of possible
workaround, allowing for retrieving image metadata even after retrieving image content failed. 
Definetely not very good thinking. Thanks Mattia Rizzolo for actually recognizing this as a bug and filing it, 
and Chris Lamb for fixing it!

## Other work ##

### Order-like differences, part 2 ###

In the previous post, I mentioned Lunar's [suggestion](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=848049#27) to use 
hashing for finding order-like difference in wide variety of input data. I implemented that idea, but after discussion with
my mentor, we decided it is probably not worth it - this change would alter quite a lot of things in core modules of diffoscope,
and the gain would be not really significant.

Still, implementing that was an important experience for me, as I had to hack on deepest and, arguably, most difficult
modules of diffoscope and gained some insight on how they work.

### Comparing with several tools (work in progress) ###

Although my initial motivation for this idea was flawed (the workaround I mentioned earlier for .ico files), it still might be
useful to have a mechanism that would allow to run several commands for finding difference, and then give the output of those 
that succeed, failing if and only if they all have failed. 

One possible case when it might happen is when we use commands 
coming from different tools, and one of them is not installed. It would be nice if we still used the other and not the 
uninformative binary diff (that is a default fallback option for when something goes wrong with more "clever" comparison).
I am still in process of polishing this change, though, and still in doubt if it is needed at all.

## Side note - Outreachy and my university progress ##

In my Outreachy application, I promised that if I am selected into this round, I will do everything I can to unload the 
required time period from my university time commitements. I did that by moving most of my courses to the first half of the 
academic year. Now, the main thing that is left for me to do is my Master's thesis.

I consulted my scientific advisors from both universities that I am formally attending ([SFEDU](http://sfedu.ru/index_eng.php) 
and [LUT](http://www.lut.fi/web/en/) - I am in double degree program), and as a result, they agreed to change my Master's thesis 
topic to match my Outreachy work.

Now, that should have sounded like an excellent news - merging these activities together actually mean I can allocate much more
time to my work on reproducible builds, even beyond the actual internship time period. 
That was intended to remove a burden from my shoulders.

Still, I feel a bit uneasy. The drawback of this decision lies in fact I have no idea on how to write scientific report based 
on pure practical work. I know other students from my universities have done such things before, but choosing my own topic means 
my scientific advisors can't help me much - this is just out of their area of expertise.

Well, wish me luck - I'm up to the challenge!
