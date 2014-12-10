---
layout: post
title: CSSFX in ScenicView master
date: '2014-12-10T22:00:00.000+01:00'
author: Matthieu BROUILLARD
tags: JavaFX ScenicView
---

Today [Jonathan Giles](http://www.jonathangiles.net/) merged my [CSSFX Pull Request](https://bitbucket.org/scenicview/scenic-view/pull-request/6/integration-of-cssfx-into-scenicview) into
ScenicView [main branch](https://bitbucket.org/scenicview/scenic-view/commits/9c93e111e09380a2f42e35cfd7a19573c48649ab).

I am quite satisfied with that and hope that a lot of developer will benefit from this addition.

Now what are the next steps?

- kill [CSSFX project](http://www.fxmisc.org/cssfx)? probably! The project would have lived only for few weeks on its own, but that's life...
- move CSSFX unit/integration tests into ScenicView
- open enhancements request on ScenicView project
  - enhance [ScenicView documentation](http://fxexperience.com/scenic-view/help/) with CSSFX stuff
  - modify CSSFX to manage removal of CSS (use case is when CSS are pushed/removed to manage theme colors for exemple)
  - propose ScenicView re-factorings: (tag `ScenicView`)
    - introduce a kind of plug-in mechanism (at least clear APIs)
    - change client/modules communication with something like a simple bus with command pattern
    - change poll mechanism (ScenicView looping and asking for modifications every XXX millis) to a push one (monitored applications push interesting changes)
  - allow dynamic assignment of CSS file to a remote non resolved CSS resource
  
For those who want to test and cannot wait for an official build, I am providing a ScenicView jar built from the main branch after the merge: [here](/public/scenic-view-8.0-cssfx.jar)

Do not hesitate to comment the next steps before I open the issues in bitbucket...