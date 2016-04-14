---
layout: post
title: Easy versioning maven projects with git
date: '2016-04-14T11:30:00.000+01:00'
author: Matthieu BROUILLARD
tags: maven git version
---

Everyone knows that [maven-release-plugin](https://maven.apache.org/maven-release/maven-release-plugin/) is a little bit intrusive when it comes to keep a clean git commit history.

I have searched for a long time for clean ways to handle that correctly.

The more I thought about this problematic the more I convinced myself that versioning (meaning keeping history) IS the role of the SCM and that version naming of artifacts (in the sense of [Semantic versioning](http://semver.org/)) should be extracted/calculated
from the SCM without polluting the SCM history.

Hopefully, GIT provides a very clean way to give a commit a good "version" useable for distribution ; it is `git describe`.

## Getting version out of the SCM

Using the following command: `git describe --tags --always --dirty=-dirty` 

Having the following history

~~~~
$ git log  --graph --abbrev-commit --format=format:'%h - %s%d'
* 2dc72d0 - apply new version scheme for maven-external-version-plugin usage (HEAD)
* b740eee - add project to source control (tag: 1.0, origin/master)
~~~~

you will get as output of `git describe --tags --always --dirty=-dirty`: 

~~~~
1.0-1-g2dc72d0
 ^  ^    ^
 |  |    |- - - -  commit ID (if not on a commit with a tag)
 |  |- - - - - - - number of commits ahead of last found tag (if not on a commit with a tag)
 |- - - - - - - -  value of closest tag found in history, starting from current branch
~~~~

or if the current checkout points to a commit that has a tag, like the following:

~~~~
$ git log  --graph --abbrev-commit --format=format:'%h - %s%d'
* 57d5893 - ignore automatic generated file for maven-external-version execution (HEAD -> master, tag: 1.1)
* 2dc72d0 - apply new version scheme for maven-external-version-plugin usage
* b740eee - add project to source control (tag: 1.0, origin/master)
~~~~

you will get `1.1` as a result

~~~~
$ git describe --tags --always --dirty=-dirty
1.1
~~~~

## Maven integration

Now that we know that we _just_ have to tag our git project to have a meaningfull version for our maven project we still have to find a way to use that computed version as our maven POM version.

For that, we can use [maven-external-version](https://github.com/bdemers/maven-external-version) plugin.

So now by adding the mugin to your POM

you can call `mvn install -Dexternal.version=$(git describe --tags --always --dirty=-dirty)`

~~~~
$ mvn clean install -Dexternal.version=$(git describe --tags --always --dirty=-dirty)
[INFO] Scanning for projects...
[INFO] About to change project version in reactor.
[INFO] component: org.apache.maven.version.strategy.SystemPropertyStrategy@1a1da881
[INFO] Updating project.build.finalName: fxweb-1.0-1-g2dc72d0
[INFO] new version added to map: com.agfa.apps:fxweb:0.0: 1.0-1-g2dc72d0
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building fxweb 1.0-1-g2dc72d0
[INFO] ------------------------------------------------------------------------
[INFO]

...

[INFO] --- maven-install-plugin:2.4:install (default-install) @ fxweb ---
[INFO] Installing D:\dev\projects\agfa\samples\fxweb\target\fxweb-1.0-1-g2dc72d0.jar to d:\dev\mlr\com\agfa\apps\fxweb\1.0-1-g2dc72d0\fxweb-1.0-1-g2dc72d0.jar
[INFO] Installing D:\dev\projects\agfa\samples\fxweb\target\dependency-reduced-pom.xml to d:\dev\mlr\com\agfa\apps\fxweb\1.0-1-g2dc72d0\fxweb-1.0-1-g2dc72d0.pom
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 3.531 s
[INFO] Finished at: 2016-04-14T12:13:06+02:00
[INFO] Final Memory: 18M/214M
[INFO] ------------------------------------------------------------------------
~~~~

## And now?

Now I think I will fork [maven-external-version](https://github.com/bdemers/maven-external-version) in order to provide another version strategy that will automatically calculate the version using JGIT so that you will not have to provide any additional arguments to your maven build.