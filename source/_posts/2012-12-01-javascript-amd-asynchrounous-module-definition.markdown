---
layout: post
title: "Javascript AMD: Asynchrounous Module Definition"
date: 2012-12-01 14:54
comments: true
categories: [javascript, amd, requirejs]
---

Before you read any further, I should warn you: I have limited experience with JS. However, please hear me out; I've found that the concepts I'm going to talk about, although imporant, are still not widely adopted by Javascript developers.

In the last few years we've seen a shift to the "cloud". With that, there has been a resurgence of the web. We're now doing things with HTML and HTTP that no one would have thought possible just 5 years ago. We now render data on the client, we push data from the server to the browser to have real time interactions like a native application would. Heck, we can even do 3D rendering on the browser... and if you have a modern browser, it works well!

In a few words, we're doing what Java Applets promised to do but never accomplished.

This re-birth of the web, however, has been bumpy. Developing for the browser is plagued with problems. You have to target multiple (old) browsers, on multiple OSes, with multiples displays; you have to deal with disparate hardware, internet connections, etc. I think you get the point: you really don't know where and how your app is going to run. And to make all of this worse, the tooling for writing browser applications is still maturing; there's not a lot of help out there.

Imagine, for example, if you had to write a Java server application and you didn't have a good compiler; or if you had to add a bunch of conditionals to detect what OS you're running on - it'd be crazy, right?

But that's not even the worse of it; imagine Java the language provided no mechanism for modularizing your code: not JARs, no classes, no nothing. And of top of that everything was globally scoped. Scary, huh?

Well, to some extent that's the situation we find ourselves in when we write Javascript applications. Javascript has no supports for modules. And of top of that, everything in the browser is globally scoped. Now, I'm aware that we developers have been playing games for years to diminish the global scoping problem, but we really haven't had a good solution. Unitl now, at least. Now we have AMD... and it changes everything.

AMD stands for asynchronous module definition. The goal of the AMD format is to provide a solution for modular Javascript that we can use right now (while we wait for [harmony](http://wiki.ecmascript.org/doku.php?id=harmony:modules)).

The genius in AMD is that it proposes a format where both the module and its dependencies are asynchronously loaded. If you're working on the browser, the async nature of AMD provides advantages over other module systems (such as CommonJS) that really make it enjoyable to code in Javascript.

But enough talk, let's get to some code. I don't want to write a full tutorial on AMD, so the code samples will be brief. I'm just hoping that when you're done reading this, you won't write a single line of code withouth using [RequireJS](http://requirejs.org).

Here's how you define a module:

```javascript

define(['jquery', 'backbone'], function ($, Backbone) {

  // At this point your dependencies are loaded (jquery and backbone).

  // Let's do some setup. Anything here is private to the module.
  var privateVariable = 5;

  // This what consumers of your module will get when they require your module.
  return Backbone.View.extend({
    id: privateVariable,
    initialize: function () {
      // Some view init code.
    },

    // The rest of your public methods.
  });


});

```

I don't know about you, but the first time I saw an AMD module I immediately fell in love. Finally I saw an easy way to have private members. Finally there was a cleaner way to scope modules and their dependencies. I could go on and on, but I think the benefits I've listed, should be enough to convince you.

Or at least, I hope this has at least made you aware of AMD and piqued your interest. Writing Javascript apps on the browser doesn't have to be a pain anymore. In fact, I rather enjoy.

I know, however, I haven't done this topic justice. Please go look at the RequireJS site and look at the samples and the documentation - espeically the the optmizer section. Go look at [this post](http://tagneto.blogspot.com/2011/04/on-inventing-js-module-formats-and.html) by James Burke where he talks about the many reasons developers are now using AMD. And then when you're done reading that, come join the fun.