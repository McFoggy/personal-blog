---
layout: post
title: a look into JEE/CDI Conversation scope
date: '2017-01-27T16:30:00.000+01:00'
author: Matthieu BROUILLARD
tags: JEE CDI scope Conversation ConversationScoped
---

Recently I had the need to investigate how CDI [Conversation](http://docs.oracle.com/javaee/7/api/javax/enterprise/context/Conversation.html) & [ConversationScoped](http://docs.oracle.com/javaee/7/api/javax/enterprise/context/ConversationScoped.html) were working.

As I faced issues at first and didn't found self working samples, I decided to write some demos/tutorials/samples.

As always the work is publicly available from [oss.brouillard.fr](http://oss.brouillard.fr/projects/jee-samples/) which redirect to [my github](https://github.com/McFoggy) [jee-samples project](https://github.com/McFoggy/jee-samples).

You will find there the 2 first samples I created demonstrating:

- [JAXRS & Conversation](https://github.com/McFoggy/jee-samples/blob/master/conversation-jaxrs/README.md)
- [JEE WebServlet & Conversation](https://github.com/McFoggy/jee-samples/blob/master/conversation-servlet/README.md)  
    ![Conversation servlket demo](/images/2017-01-27_conversation-servlet.png)
    
Feel free to give me some feedback.