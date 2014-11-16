---
layout: post
title: CSSFX integrated into ScenicView
date: '2014-11-16T14:05:00.001+01:00'
author: Matthieu BROUILLARD
tags: CSSFX ScenicView JavaFX
---

After a discussion with [Jonathan Giles](http://jonathangiles.net/) I tried a first integration of [CSSFX](http://www.fxmisc.org/cssfx/) into ScenicView.
Result is very positive. I could integrate, for demo purpose, quite easily [CSSFX](http://www.fxmisc.org/cssfx/) into [ScenicView](http://fxexperience.com/scenic-view/).

This integration makes monitoring the CSS changes of application working without any change in the application.

run ScenicView, run your application and you directly have [CSSFX](http://www.fxmisc.org/cssfx/) monitoring your CSS sources.

You can test it by downloading this [modified version of ScenicView](/public/scenic-view-8.0-cssfx.jar).

I have not yet officially push any changes to the [ScenicView repository](https://bitbucket.org/scenicview/scenic-view) because I want first to be sure how [CSSFX](http://www.fxmisc.org/cssfx/) has to be integrated into ScenicView. I'll discuss this integration in the next days/weeks with Jonathan & the other project members.