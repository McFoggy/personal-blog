---
layout: post
title: force annotation on classes
date: '2016-11-26T11:00:00.000+01:00'
author: Matthieu BROUILLARD
tags: java apt annotation processing
---

Recently I had the need to ensure that an annotation was set on each classes of a project.
The idea was to make the build fail if the annotation was missing on some types.

Let's consider that we have a `@Responsible` annotation for that purpose

``` java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.SOURCE)
public @interface Responsible {
    String value();
}
```

that can be used like this

``` java
@Responsible("Matthieu Brouillard")
public class SomeService {
}
```

Now the question is: _how to make the build fail?_

When we talk about annotations and build phase, immediately we think about ["java annotation processing"](http://docs.oracle.com/javase/8/docs/api/javax/annotation/processing/package-summary.html).
It is very easy using the JDK provided API to parse elements that have some annotations, but how can we process elements that are NOT annotated.

Still the ["java annotation processing"](http://docs.oracle.com/javase/8/docs/api/javax/annotation/processing/package-summary.html) framework can be used using the _universal processors_ feature:

> If there are no annotation types present, annotation processing still occurs but only universal processors
> which support processing all annotation types, "*", can claim the (empty) set of annotation types. [[javadoc](http://docs.oracle.com/javase/8/docs/api/javax/annotation/processing/Processor.html)]

Knowing that it becomes easy to define a processor that will check all elements and retain only those that have no `@Responsible`  

``` java
@AutoService(Processor.class)
public class ResponsibleAnnotationProcessor extends AbstractProcessor {
    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment env) {
        List<ElementKind> typeKinds = Arrays.asList(ElementKind.ENUM, ElementKind.INTERFACE, ElementKind.CLASS);
        // let's gather all types we are interested in
        Set<String> allElements = env
                .getRootElements()
                .stream()
                .filter(e -> typeKinds.contains(e.getKind())) // keep only interesting elements
                .map(e -> e.asType().toString()) // get their full name
                .collect(Collectors.toCollection(() -> new HashSet<>()));
        Set<String> typesWithResponsible = new HashSet<>();

        annotations.forEach(te -> {
            if (Responsible.class.getName().equals(te.asType().toString())) {
                // We collect elements with an already declared ownership
                env.getElementsAnnotatedWith(te).forEach(e -> typesWithResponsible.add(e.asType().toString()));
            }
        });

        allElements.removeAll(typesWithResponsible);
        allElements.forEach(cname -> processingEnv.getMessager().printMessage(
                Diagnostic.Kind.ERROR,
                cname + " must be annotated with @" + Responsible.class.getName() + " to declare a ownership")
        );
        return false;
    }

    @Override
    public SourceVersion getSupportedSourceVersion() {
        return SourceVersion.latestSupported();
    }

    @Override
    public Set<String> getSupportedAnnotationTypes() {
        // We want to process all elements
        return Collections.singleton("*");
    }
}
```

which when used can make the build fail with the following message:

```
[ERROR] COMPILATION ERROR :
[INFO] -------------------------------------------------------------
[ERROR] fr.brouillard.oss.app.ServiceTwo must be annotated with @fr.brouillard.oss.annotation.Responsible to declare a ownership
[INFO] 1 error
```

The full demo project is accessible on [github](https://github.com/McFoggy/force-annotation-demo): [https://github.com/McFoggy/force-annotation-demo](https://github.com/McFoggy/force-annotation-demo)
