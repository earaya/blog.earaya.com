---
layout: post
title: "Javascript AMD: Asynchrounous Module Definition"
date: 2012-12-01 14:54
comments: true
categories: javascript, amd, requirejs
---

Before you go any further, I should warn you: I have no idea what I'm talking about. I've only been doing "strong" client development in the browser for a few months. Note, however, that I say client development, not website development. Let me explain what I mean.

In the last few years we've seen a shift to the "cloud". With that, there has been a resurgence of the web. We're now doing things with HTML and HTTP that no one would have thought possible just 5 years ago. We now render data on the client, we push data from the server to the browser to have real time notifications. Heck, we can even do 3D rendering on the browser... and if you have a modern browser, it works well!

In a few words, we're doing what Java Applets promised to do... t

This web re-birth, however, hasn't been easy. Developing for the browser is plagued with problems. You have to target multiple browsers, on multiple OSes, with multiples displays; you have to deal with d... And to make all of this worse, the tooling is still maturing; there's not a lot of help out there.

In this article, however, I really want to talk about just one problem: Javascript has no way to define modules. And of top of that, everything in the browser is scoped globally. Now, I'm aware that we developers have been playing games for years to diminish the global namespacing problem, but we really haven't had a good solution. Unitl now, at least. Now we have AMD.

AMD stands for asynchronous module definition.