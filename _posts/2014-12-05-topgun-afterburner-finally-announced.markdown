---
layout: post
title: External DI management with Afterburner.FX
date: '2014-12-09T00:00:00.000+01:00'
author: Matthieu BROUILLARD
tags: JavaFX adambien afterburner
---

Finally, yesterday [Adam Bien announced](http://www.adam-bien.com/roller/abien/entry/introducing_afterburner_fx_topgun_edition) the afterburner community branch that he called [topgun](https://github.com/AdamBien/afterburner.fx/tree/topgun).

There you'll find for the moment 3 [enhancements](https://github.com/AdamBien/afterburner.fx/network) that I contributed:

- [named annotation support](#named-annotation-support)
- [injection for interface](#interface-injection)
- [ability to plug-in external DI framework (CDI, guice, spring, ...)](#external-di-framework-support)

## Named annotation support

This addition was the first [Pull Request](https://github.com/AdamBien/afterburner.fx/pull/30) I submitted to afterburner.fx.

It is very simple and allows for injection of values which cannot be directly tied to a java attribute because of java naming conventions.

{% highlight java %}
@Inject
@Named("user.dir")
private String workingDir;
{% endhighlight %}

It can be also necessary to use the `@Named` if you use [program argument injection]({% post_url 2014-09-20-javafx-application-arguments-injection_20 %}) and that you have complex argument names.   

## Interface injection

This addition, tracked behind [PR-34](https://github.com/AdamBien/afterburner.fx/pull/34), allows the resolution of injection point that use an interface instead of a class type.

I provided it while thinking on how to plug external DI framework ; I thought first why not allow the already existing JRE service provider, [ServiceLoader](https://docs.oracle.com/javase/8/docs/api/java/util/ServiceLoader.html), to work with afterburner.fx.

Finally, if you have attributes declared using interface, whith this addition they will be resolved via the [ServiceLoader](https://docs.oracle.com/javase/8/docs/api/java/util/ServiceLoader.html) by looking at your declared services in `META-INF\services`. 

## External DI framework support

Finally the third contribution, [external DI support](https://github.com/AdamBien/afterburner.fx/pull/37).

It allows you to use afterburner.fx with your preferred DI framework and not the internal afterburner.fx one. 

Adam already thought about such kind of features by allowing to plug in afterburner an external function acting as instance provider for injection points & FXML controllers. This contribution extends this by enhancing a little bit this concept by defining a real InstanceProvider contract allowing to:

- provide Object instances (that's the first goal isn't it ;-) )
- bypass afterburner.fx internals for:
    - resolution of injection points
    - caching/scoping
    
Using afterburner.fx with your preferred DI framework is now possible and easy! Here is what you need to do in order to plug CDI (weld-se implementation). You have then the idea if you want to plug other DI framework.

{% highlight java %}
Weld weld;
WeldContainer container;

weld = new Weld();
container = weld.initialize();

Injector.setInstanceSupplier(new Injector.InstanceProvider() {
    @Override
    public boolean isInjectionAware() {
        // CDI realizes injections
        return true;
    }

    @Override
    public boolean isScopeAware() {
        // CDI knows about scopes
        return true;
    }
    
    @Override
    public Object instanciate(Class<?> c) {
        return container.instance().select(c).get();
    }
});
{% endhighlight %}

Thank you Adam for providing this stuff to the community.

@[AGFA healthcare](http://www.agfahealthcare.com) we are still evaluating if we will finally use this or if we will go for a pure CDI solution, but if you use it with any external DI, please let me and Adam know about that. 