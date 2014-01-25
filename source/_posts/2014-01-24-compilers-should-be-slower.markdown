---
layout: post
title: "Compilers Should Be Slower"
date: 2014-01-24 16:30
comments: true
categories:
---

Sometimes I think compilers (and interpreters, I suppose) should be slower. This would force us to slow down and reason about the code we write.

In [Thinking, Fast and Slow](http://www.amazon.com/gp/product/0374533555/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=0374533555&linkCode=as2&tag=earaya-20), Daniel Kahneman breaks down thinking into 2 modes. The slow mode is a deliberate and conscious mode; it's the kind of thinking you do when you're in control and are accutely aware of the choices you're making. The fast mode, is a "reactive" mode driven by our nature and our insticts; it's the kind of thinking you do when you need to quickly asses a situation and react to it immediately.

Slow thinking, then, is the kind of thinking you do when deciding what vehicle to purchase. Fast thinking, is the kind of thinking you do when you wake up at night and need to go to the bathroom.

And here, again, is the reason why I half-wish compilers were slower. Writing code is not the kind of task you can do in fast thinking mode. Even with years of prior experience writing software, you can't code thinking fast.

Writing software is a tough beast: it takes a lot of research, discovery, and prototyping. Clearly, writing code is slow thinking.

It's easy, however, to feel like systems you've developed before are similar to the software you're currently writing. It's tempting to think you can just go with prior knowledge, make some assumptions, do things just like you've done them before. It's easy to think you can use some heuristics and think fast.

But you can't. There are too many details and too many dependencies that you need track. There are too many unknowns and too many side effects that you need to consider. In other words, you'll never achieve quality software by thinking fast.

So, just take a minute and slow down. Think. Design. Prototype. Test. Then throw away all the code you've written and start all over again. It's surprising how much even just a little design helps.

Even if all you do is grab some paper and draw a couple of diagrams; even if all you do is just talk to a few people about some of your ideas, you'll be better off.

We need to reason about the code we write; we need to think slowly and carefully about the changes we make.

And if compilers were slower, we might just be forced to think slow. :)