---
layout: post
title: "Using Char Arrays For Passwords"
date: 2012-07-25 14:52
comments: true
categories: security
---

Just a few days ago a co-worker asked me about mongodb's choice of `char[]` for the password in their [authenticate](http://api.mongodb.org/java/current/com/mongodb/DB.html#authenticate(java.lang.String, char[]\)) call. He was wondering why they chose`char[]` over `String`.

The reason is simple: some people feel that using a `char[]` is safer since you can "wipe out" the array after you're done with the data (eg. the password). `String` being an immutable type does not allow this precaution; once you're done with the `String` it'll live in memory until garbage collection decides to claim it at an undetermined time in the future.

Them implication, of course, is that an attacker may read the contents of the memory before the `String` is garbage collected and steal the password.

This, however, seems a paranoid to me. If you're really about someone swiping the contents of your memory, switching to a `char[]` doesn't really solve the problem; it only reduces the time window in which this can happen. I'll also say that if someone is able to read the contents of your memory space, you have bigger problems than using a `String` for your password.

Anyhow, back the question at hand: using `char[]` reduces the time window in which an attacker can read the contents of your memory, but it definitely doesn't' solve the problem.