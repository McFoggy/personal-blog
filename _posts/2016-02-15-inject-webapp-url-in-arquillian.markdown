---
layout: post
title: Inject webapp URL into arquillian integration tests
date: '2016-02-15T22:00:00.000+01:00'
author: Matthieu BROUILLARD
tags: java junit arquillian tests webapp
---

I really like [arquillian](arquillian.org) for testing my jee projects.

Until now, when I needed to test webapp REST endpoints, I used `arquillian-rest-client-api` plus one of its implementation _(jersey or resteasy)_ and my tests were being injected with a WebTarget pointing to the webapp under test like that:

{% highlight java %}
@Test
public void some_test(@ArquillianResteasyResource("endpoint") WebTarget target) {
    ...
}
{% endhighlight %}

Recently in [xhub4j](https://github.com/McFoggy/xhub4j) project, to test JAX-RS components (a ClientRequestFilter & a WriterInterceptor), I needed only the webapp URL under test to build the WebTarget by myself.
[Arquillian](arquillian.org) is smart enought to also provide such injection via `@ArquillianResource`:

{% highlight java %}
    @ArquillianResource URL deploymentURLAsClassAttribute;
    
    @Test
    public void a_test(@ArquillianResource URL deploymentURLAsParameter) {
        assertNotNull(deploymentURLAsClassAttribute);
        assertNotNull(deploymentURLAsParameter);
        
        assertThat(deploymentURLAsParameter, is(deploymentURLAsClassAttribute));
        assertThat(deploymentURLAsParameter.toString(), startsWith("http://"));
    }
{% endhighlight %}
