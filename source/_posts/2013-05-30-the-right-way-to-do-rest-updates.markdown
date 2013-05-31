---
layout: post
title: "The Right Way To Do REST Updates"
date: 2013-05-30 22:56
comments: true
categories: [rest, api]
---

There's lots of good advice on the internet about how to design a good REST API. I strongly agree with a lot of the principles outlined in this artcile by Xyz.

Unfortunately, most of the advice you'll find out there focuses on GETs (reads) and POSTs (inserts). So, I just wanted to offer some advice on updates.

First and foremost, you should support partial updates through the PATCH method (verb). There are several reasons why you should prefer PATCHes over PUTs, but the key ones are:

1. Partial updates ease update concurrency problems.
2. Partial updates simplify

Cons

Lack of support, browsers, tools, etc.
Data update might be a bit harder (can't just save the entire thing).