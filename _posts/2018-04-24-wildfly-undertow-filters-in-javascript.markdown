---
layout: post
title: javascript wildfly/undertow in your toolbox
date: '2018-04-24T10:30:00.000+01:00'
author: Matthieu BROUILLARD
tags: wildfly undertow javascript nashorn
---

Several times in the past, on some client installations of our Healthcare application (> 1000 hospitals across Europe), we had to correct some behaviors using undertow filters.  
Fortunately, some filters are already included out of the box inside wildfly including `rewrite`, `response-header`, `request-limit`, `gzip`...  
But what happens if you need more like:

- modifying request headers for example to change/adapt/fix a media-type
- correcting incoming request or updating response to fix an issue in a JSON message

Wouldn't it be great if you could dynamically add a powerfull filter written in plain javascript?

That's the main purpose of the [undertow-jsfilters](https://github.com/McFoggy/undertow-jsfilters) project I have created: to write filters in javascript.  
The javascript filters are interpreted via [Nashorn](http://openjdk.java.net/projects/nashorn/) and just need to be declared as wildfly/undertow [custom-filter](https://wildscribe.github.io/WildFly/10.1/subsystem/undertow/configuration/filter/custom-filter/index.html) using CLI commands or by updating the standalone.xml.

```xml
<subsystem xmlns="urn:jboss:domain:undertow:3.1">
    <buffer-cache name="default"/>
    <server name="default-server">
        <http-listener name="default" socket-binding="http"/>
        <host name="default-host" alias="localhost">
            <location name="/" handler="welcome-content"/>
            <filter-ref name="server-header"/>
...
            <filter-ref name="jsfilter-version"/>
            
        </host>
    </server>
    <filters>
        <response-header name="server-header" header-name="Server" header-value="WildFly/10"/>
...
        <filter name="jsfilter-version" class-name="fr.brouillard.oss.undertow.JSFilter" module="fr.brouillard.oss.undertow.jsfilters">
            <param name="fileName" value="D:/dev/projects/personnal/oss/undertow/js-filters/src/test/js/jsfilters-version-header.js"/>
            <param name="refresh" value="true"/>
        </filter>
    </filters>
</subsystem>
```

Do not hesitate to visit the [project page](https://github.com/McFoggy/undertow-jsfilters) & open [issues](https://github.com/McFoggy/undertow-jsfilters/issues) on purpose.