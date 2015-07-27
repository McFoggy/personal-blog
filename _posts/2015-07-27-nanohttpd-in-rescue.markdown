---
layout: post
title: NanoHttpd to the rescue
date: '2015-07-27T13:00:00.000+01:00'
author: Matthieu BROUILLARD
tags: java web javascript cors
---

Quite often now I have the need to quickly start local web server to serve some static content.

Also, it occures that I need some [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) support in this server.

To fullfill those needs, I use a slightly modified version of [NanoHttpd](https://github.com/NanoHttpd/nanohttpd/pull/207) in which I enabled some basic [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) support.

As I useally work on Windows (ouhhhh), I then have a batch file that references the built webserver.jar (the one with all dependencies).

{% highlight batch %}
@ECHO OFF
SET JAVA_HOME=D:\tools\jdks\1.8
SET PATH=%JAVA_HOME%\bin; %PATH%
SET WEB_SERVER_JAR=D:\dev\projects\oss\nanohttpd\webserver\target\nanohttpd-webserver-2.2.0-SNAPSHOT-jar-with-dependencies.jar

java -jar %WEB_SERVER_JAR% %*
{% endhighlight %}

After that, wherever I am on my disk I can freely do some:

- `c:\somewhere>web .` to launch a webserver on default port (8080) serving files located under _c:\somewhere_ folder
- `c:\somewhere>web . -p 9090` to launch a webserver on port 9090 serving files located under _c:\somewhere_ folder
- `c:\somewhere>web . -p 9090 --cors` to launch a webserver on port 9090 serving files located under _c:\somewhere_ folder and providing basic [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) support
