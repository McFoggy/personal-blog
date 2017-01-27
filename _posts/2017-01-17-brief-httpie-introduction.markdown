---
layout: post
title: brief introduction to HTTPie
date: '2017-01-17T18:00:00.000+01:00'
author: Matthieu BROUILLARD
tags: httpie curl REST JEE
---

[HTTPie](httpie.org) is really a very cool CLI command tool to query REST services.

Calling services is as simple as issuing some commands like:
  `http httpbin.org/ip`

For development on your local machine there are even cool shortcuts in the syntax
  `http :8080/myapp`

will do a GET http call to [http://localhost:8080/myapp](http://localhost:8080/myapp)

The syntax is very user friendly, let's remember some usefull:

- headers, use __":"__
- query params, use __"=="__

Using the above, calling `http httpbin.org/get X-Blog:header-value exQP==someValue` will result in

```
HTTP/1.1 200 OK
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: *
Connection: keep-alive
Content-Length: 299
Content-Type: application/json
Date: Tue, 17 Jan 2017 15:27:39 GMT
Server: nginx

{
    "args": {
        "exQP": "someValue"
    },
    "headers": {
        "Accept": "*/*",
        "Accept-Encoding": "gzip, deflate",
        "Host": "httpbin.org",
        "User-Agent": "HTTPie/0.9.9",
        "X-Blog": "header-value"
    },
    "origin": "90.80.28.58",
    "url": "http://httpbin.org/get?exQP=someValue"
}
```

_For a full documentation, visit [HTTPie docs](https://httpie.org/doc)._
