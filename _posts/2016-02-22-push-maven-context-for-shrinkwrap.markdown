---
layout: post
title: correctly propagate maven context for shrinkwrap
date: '2016-02-22T12:30:00.000+01:00'
author: Matthieu BROUILLARD
tags: maven arquillian shrinkwrap
---

When using shrinkwrap in your tests, you might need to have correct maven settings defined.

If you set manually the settings to use as defined by [shrinkwrap resolvers](https://github.com/shrinkwrap/resolver#system-properties) you might have no problem.

But honestly as we are running our tests with maven, it is easier to let shrinkwrap use the contextual values, isn't it?
So you plug the `shrinkwrap-resolver-maven-plugin` that does exactly that

~~~~
<plugin>
    <groupId>org.jboss.shrinkwrap.resolver</groupId>
    <artifactId>shrinkwrap-resolver-maven-plugin</artifactId>
    <version>${shrinkwrap.version}</version>
    <executions>
        <execution>
            <phase>pre-integration-test</phase>
            <goals>
                <goal>propagate-execution-context</goal>
            </goals>
        </execution>
    </executions>
</plugin>
~~~~

and you discover that it does not work...

The solution is simple, the plugin populates properties with a namespace which is by default `maven.execution.`, thus filling:

- `maven.execution.user-settings`
- `maven.execution.global-settings`
- `maven.execution.pom-file`
- `maven.execution.offline`
- `maven.execution.active-profiles`

If you change it to what expects shrinkwrap `org.apache.maven.` then everything is fine.
Final configuration is then

~~~~
<plugin>
    <groupId>org.jboss.shrinkwrap.resolver</groupId>
    <artifactId>shrinkwrap-resolver-maven-plugin</artifactId>
    <version>${shrinkwrap.version}</version>
    <executions>
        <execution>
            <phase>pre-integration-test</phase>
            <goals>
                <goal>propagate-execution-context</goal>
            </goals>
            <configuration>
                <namespace>org.apache.maven.</namespace>
            </configuration>
        </execution>
    </executions>
</plugin>
~~~~
