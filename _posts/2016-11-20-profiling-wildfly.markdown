---
layout: post
title: profiling wildfly with netbeans
date: '2016-11-20T14:00:00.000+01:00'
author: Matthieu BROUILLARD
tags: wildfly profiler netbeans
---

As it always takes me some time to retrieve the correct configuration to do it, let's put it clearly in a blog post for future usage.

So let's go, here are the modifications to allow an agent (here netbeans profiler) to be activated at wildfly startup, notice that library versions must be adapted to the correct versions:

```
REM somewhere at the begiining of standalone-conf.bat  (or .sh)

SET WILDFLY_DIR=POINT_TO_WILDFLY_INSTALL_DIR
SET NETBEANS_DIR=POINT_TO_NETBEANS_INSTALL_DIR

SET "JAVA_OPTS=%JAVA_OPTS% -Xbootclasspath/p:%WILDFLY_DIR%\modules\system\layers\base\org\jboss\logmanager\main\jboss-logmanager-2.0.4.Final.jar;%WILDFLY_DIR%\modules\system\layers\base\org\jboss\log4j\logmanager\main\log4j-jboss-logmanager-1.1.2.Final.jar"
SET "JAVA_OPTS=%JAVA_OPTS% -Djava.util.logging.manager=org.jboss.logmanager.LogManager"
SET "JAVA_OPTS=%JAVA_OPTS% -agentpath:%NETBEANS_DIR%\profiler\lib\deployed\jdk16\windows-amd64\profilerinterface.dll=%NETBEANS_DIR%\profiler\lib,5140"

REM now locate the following line and comment it
REM set "JAVA_OPTS=%JAVA_OPTS% -Djboss.modules.system.pkgs=org.jboss.byteman"

REM to allow org.jboss.logmanager classes to be also visible by all classloaders
SET "JAVA_OPTS=%JAVA_OPTS% -Djboss.modules.system.pkgs=org.jboss.byteman,org.jboss.logmanager"
```

You're done, start wildfly using : `standalone.bat`.