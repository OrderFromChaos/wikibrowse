# wikibrowse

In the days leading up to April, I've been writing a research proposal. One of the most daunting bits about it was all the research that I needed to do before even writing the proposal - there were so many areas to explore and the paths between them take effort to find.

Wikipedia is a platform which by its nature keeps track of these paths. This is through hyperlinks inside of the articles. I've previously found some great code people have written for taking advantage of this, like [Xefer's Wkipedia thing](https://xefer.com/WIKIPEDIA) (which shows that all articles [lead back to philosophy](https://en.wikipedia.org/wiki/Wikipedia:Getting_to_Philosophy)).

I'm interested in getting a clearer idea of the meta-structure of physics, so I wrote up this code using Python and the magnificent Selenium library. It does require classification by hand, but it's set up to make this classification process as easy as pressing a button.

**wikibrowse** works using three text documents:
1. wikipedia_physics
    (This document keeps track of all of the positively identified physics articles. It is also the foundation for how new articles are selected.)
2. wikipedia_physics_rejected
    (This is fairly self-explanitory.)
3. wikipedia_finished
    (Once you have classified all the links on a Wikipedia page, the article URL goes here. Then, when the code is run for the next time, it will select the first non-finished article URL in wikipedia_physics.txt.)
(These can be changed to whichever subject you want, of course; you put in the original seed article and do the classification.)

If you get some distance into a wikipedia article but get frustrated at how long it's taking, just type "quit" into the classification input, and it will save all the links you've put down so far and quit the process. Next time, the checking process will skip over all the links you have already classified.

Using this code, I was able to pretty quickly classify a ton of physics-related articles. I've attached the text document for my progress if you'd like to see it.
