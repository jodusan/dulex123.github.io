---
layout: post
title: Starting with DL frameworks
category: posts
---

***Notice: This 3min post can potentially save you ~14hrs of wasted time,
depending on your level of knowledge.***

___

*Dear Diary,  
I have lost around __16 working hours__ on trying to set up caffe deep
learning framework on Ubuntu 16.04. I am currently on the 9th level of
dependency hell, and there is no light at the end of the tunnel. There is
no tunnel either. Because you have held broken packages.*

# Doing it yourself
Hands-on experience tops every other learning method. Therefore it makes
sense to start working with higher-level frameworks/libraries as soon as
you feel you've conquered nuts and bolts of
[machine learning foundations][ml-immersion].

But before you dive in and type all the research papers away, process of
installation can get awfully bad.

It can get n-wasted-hours bad. Or even worse, you somehow manage to
scrape it together, but you feel bad because you've just entered fifteen
semi-familiar commands and partially bloated your system since five
didn't work.

# The time-consuming way
Install everything by reading official docs. Sometimes building from
source. lol

Questions I forgot to ask myself while I did this:  
 - Do you really want all that extra software on your host machine?  
 - What if you wanted to do same experiment/project somewhere else?  
   Installing everything again?  
 - If so what about host-specific configs needed on other machines?


# Docking the install
[Docker][docker] is this fantastic tool that efficiently allows you to isolate
your dev environments from the host machine(s).

I have started using it just a few days ago, so I am no expert but I already
see many benefits.

Docker terminology:  

- **Dockerfile**  
    A text document that has all commands needed to build _the image_. It's
    the recipe on how to create the image and what dependencies to install.
- **Image**  
    Docker *image* is built by dockerfile. It's an immutable template that has
    all dependencies and can be instantiated as _container_.
- **Container**  
    The container is a lightweight, self-contained environment that is instantiated
    from an image and can be accessed via terminal/GUI. You can easily setup shared
    folder between host and container, open ports for ipython notebooks or just use
    terminal directly.


Having clear idea of the concepts above, following through this
[awesome README][readme] you'll get it up and running in no time.

You may also find useful this [beautiful cheatsheet][cheatsheet] that has many
useful docker commands which can help your docker-fu.



[docker]: https://www.docker.com/
[cheatsheet]: https://github.com/wsargent/docker-cheat-sheet/blob/master/README.md
[ml-immersion]: /2016/machine-learning-immersion
[readme]: https://github.com/saiprashanths/dl-docker
