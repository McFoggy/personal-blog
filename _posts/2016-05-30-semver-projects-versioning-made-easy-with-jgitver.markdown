---
layout: post
title: Automate & simplify projects semver compatible versioning with jgitver
date: '2016-05-29T14:30:00.000+01:00'
author: Matthieu BROUILLARD
tags: git jgitver version semver maven gradle versioning
---

> For the impatient go directly to the [jgitver](https://github.com/jgitver/jgitver), [jgitver-maven-plugin](https://github.com/jgitver/jgitver-maven-plugin) or [gradle-jgitver-plugin](https://github.com/jgitver/gradle-jgitver-plugin) sites to have usage & explanations on how to simplify project versioning using git.
>  
> The others can continue reading...

I am always trying to find ways to ease & automate tasks in my day to day activities.

One thing that I became a big fan of, is to have a clean and meaningfull SCM history in the projects I work on.

Recently, inside my company, I came to another team meeting where I heard they had to implement some dedicated workflows in order to trace _"mark as merged"_ activites on POM file to justify that no real merge was done.

This highlighted, yet another time, a big problem that I always had with SCM & build tools: **the need to modify the project descriptors just to do changes in the project version**.  
Those version changes introduce problems to my eyes:

- polluting commits with version changes (2 at least with maven-release-plugin)
- unecessary merges & potential conflicts on project descriptors
- manual edition of project descriptors for work in branch to avoid continuous integration conflicts

Several people already worked on simplifying this topic, and one of the most popular google answers would be the ones coming from Axel Fontaine blog:

- "[maven release on steroid](https://axelfontaine.com/blog/maven-releases-steroids.html)" 
- and "[Maven Release Plugin: The Final Nail in the Coffin](https://axelfontaine.com/blog/final-nail.html)".

I also noticed the work of [Brian Demers](https://github.com/bdemers/maven-external-version/) that allows to externally provide to maven the version to use for the projects you are building.  

Then recently, people also started to use `git describe` in order to build project versions. One example that come to my mind is David Bernard usage on metrics-influxdb gradle [build](https://github.com/davidB/metrics-influxdb/blob/master/build.gradle#L12).

One **BIG pitfall** of `git describe` is that the version produced are not compliant with standard build resolution mechanism when dealing with version ranges and so-on.  
As an example, with `git describe` approach, you will first tag your project with `1.0.0` for a given commit then the next commit will receive the version `1.0.0-1` using `git describe`. Unfortunately, following semver version resolution most of the build tools will consider that `1.0.0-1 < 1.0.0`.

Having those approaches in mind ; I had the feeling that we could provide something acting like `git describe` but more oriented and integrated with build tools and respecting semver versioning.

That's why I started [jgitver](https://github.com/jgitver/jgitver-maven-plugin) and its associated plugins:

- [jgitver-maven-plugin](https://github.com/jgitver/jgitver-maven-plugin)
- [gradle-jgitver-plugin](https://github.com/jgitver/gradle-jgitver-plugin)

[jgitver](https://github.com/jgitver/jgitver-maven-plugin) will control project version using:

- **git tags**, both annotated & lightweight tags
- **commits**, especially distance & IDs
- **branch name**, sanitized to be semver compatible


Using conventions over configuration principles, jumping into jgitver is no-brainer ; for example for maven, the following is enough:

~~~~xml
<build>
...
  <extensions>
    <extension>
      <groupId>fr.brouillard.oss</groupId>
      <artifactId>jgitver-maven-plugin</artifactId>
      <version>X.Y.Z</version>
    </extension>
  </extensions>
...
</build>
~~~~

to have your project automatically versionned like the following

![CSSFX tab in ScenicView](/images/jgitver-maven-like.gif)

Moreover [jgitver](https://github.com/jgitver/jgitver-maven-plugin) allows you to fine grained control how the versions are calculated if the defaults do not suit your needs.

As a conclusion, If you,

- want to simplify your projects build & release,
- do not support [maven-release-plugin](http://maven.apache.org/maven-release/maven-release-plugin/) anymore,
- want to keep your project history clean
- have a consistent and automatic semver compatible naming convention for your project versions

have a look at [jgitver](https://github.com/jgitver/jgitver-maven-plugin) and its [maven](https://github.com/jgitver/jgitver-maven-plugin) or [gradle](https://github.com/jgitver/gradle-jgitver-plugin) integrations.

Any feedback, comment &/or PR is highly appreciated of course.
