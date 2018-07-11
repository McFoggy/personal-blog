---
layout: post
title: jgitver gradle plugin new DSL features
date: '2018-07-11T07:45:00.000+01:00'
author: Matthieu BROUILLARD
tags: jgitver gradle git dsl maven
---

Yesterday I released version `0.5.0` of [jgitver grdale plugin](https://github.com/jgitver/gradle-jgitver-plugin) bringing a new DSL functionnality to control branch policies of jgitver.

Given a project where people work in feature branch and where releases are performed from master branch, the following gradle configuration would be a perfect match

```groovy
jgitver {
    policy {
        pattern = '(master)' 
        transformations = ['IGNORE']
    }
    policy {
        pattern = 'feature_(.*)' 
        transformations = ['REMOVE_UNEXPECTED_CHARS', 'LOWERCASE_EN']
    }
}
```

When working in a branch called `feature_Security_Addon` could lead in having project version computed like `1.2.1-3-securityaddon`.

In above DSL extension, each `policy` closure expects 2 parameters:

- _pattern_: a regexp with capturing group
- _transformations_: an array of [BranchNameTransformations](https://github.com/jgitver/jgitver/blob/master/src/main/java/fr/brouillard/oss/jgitver/BranchingPolicy.java#L127) names to be applied

Retrieve configuration capabilities on the [Configuration](https://github.com/jgitver/gradle-jgitver-plugin#configuration) section of the [readme](https://github.com/jgitver/gradle-jgitver-plugin).

Like never, jgitver needs your support, if you enjoy what it does please:
- [like it](https://github.com/jgitver/gradle-jgitver-plugin) on github
- blog or [tweet](https://twitter.com/?lang=en) about it
