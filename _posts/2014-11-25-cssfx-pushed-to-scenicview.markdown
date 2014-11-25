---
layout: post
title: CSSFX pushed to ScenicView repository
date: '2014-11-25T12:15:00.001+01:00'
author: Matthieu BROUILLARD
tags: CSSFX ScenicView JavaFX
---

After some modifications and finally having found few minutes to do it, I finally pushed a [pull request](https://bitbucket.org/scenicview/scenic-view/pull-request/6/integration-of-cssfx-into-scenicview) into [ScenicView](http://fxexperience.com/scenic-view/).

This integration brings:

* an automatic monitoring of CSS source file for the JavaFX application connected to ScenicView
* the detection uses CSSFX defaults (see [details](https://github.com/McFoggy/cssfx/blob/master/src/main/java/org/fxmisc/cssfx/impl/URIToPathConverters.java))
  * maven layout
  * gradle layout
  * jar execution from build/target directory
* a read-only view in ScenicView of the discovered CSS and their mapped source files
![CSSFX tab in ScenicView](/images/2014-11-25-ScenicView-v8.0.0.png)

Monitor the PR and/or follow announcements to see when it comes life.