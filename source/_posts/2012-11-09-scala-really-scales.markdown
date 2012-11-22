---
layout: post
title: "Scala Really Scales"
date: 2012-11-09 15:13
comments: true
categories: [scala, jvm]
---

About 3 months ago [@iammerrick](http://twitter.com/iammerrick) started talking to me about Scala. After talking a little bit about it, he said: "It's called Scala because the language scales with you".

My initial reaction was to dismiss Merrick's statement as "just marketing fluff". However, after playing with the language a bit, I'm convinced Scala is really the way forward on the JVM.

As a side note - if you can take the [Functional Programming Principles In Scala](https://www.coursera.org/course/progfun) course by [Martin Odersky](http://en.wikipedia.org/wiki/Martin_Odersky) on [Coursera](http://coursera.org), I'd highly recommend it. I just finished the class and really enjoyed it. The course is a little on the tough side (specially the last 3 assignments), but it's a great introduction to Scala and to functional programming.

## Why Scala Matters

The JVM is a superb platform. It runs everywhere and it's just solid.

Furthermore, the Java ecosystem is fantastic. Everything from the IDEs, to the build tools, to the Servlet containers, to the OSS projects that run on Java are first class, well documented and well supported by a fantastic community of excellent developers.

Java the language, however, has failed to keep with the times. The lack of anonymous functions and closures, the lack of type inference, the lack of object and array literals, and the lack of many other constructs make the language feel archaic. Unfortunately it's not just that the language is arcane, it's verbose and it gets in the way; it's not that all of this is annoying, it gets in the way of productivity. If often wish C# would run on the JVM.

In summary, the JVM is a great platform, but it just needs a better statically typed language.

## How Scala Truly Scales

The thing about Scala is that it has a low barrier to entry. If you're not used to functional languages, you can write imperative code and get started anyhow. Classes are also first class citizens, so if you're coming from Java, you'll feel right at home.

If you've written C# and used some of the newer features such as LINQ, anonymous functions, etc., the transition will be even easier.

And then, when you're starting to get comfortable, you can write truly idiomatic Scala. It's simple, uncluttered, and succinct. Here's a short sample:

``` scala

  /**
   * This function decodes the bit sequence `bits` using the code tree `tree` and returns
   * the resulting list of characters.
   */

  def decode(tree: CodeTree, bits: List[Bit]): List[Char] = {
    def decode0(tree0: CodeTree, bits: List[Bit], acc: List[Char]): List[Char] = {
      tree0 match {
        case Leaf(c, w) => decode0(tree, bits, acc :+ c)
        case Fork(l, r, cs, w) => {
          if (bits.isEmpty) acc
          else if (bits.head == 0) decode0(l, bits.tail, acc)
          else decode0(r, bits.tail, acc)
        }
      }
    }
    decode0(tree, bits, List())
  }

```

(Note, I'm not claiming to write idiomatic Scala. In fact, as mentioned before, I've just barely started using the language. This snippet, however, demonstrates some good features).

If you're like me, it may take you a bit to get used to they syntax. In fact, I thought I'd never get used to the implicit returns and the optional semicolons - but now I wish I'd never have to type those extra characters ever again.

[Pattern matching and case classes](http://www.scala-lang.org/node/107) are a fantastic features. I wish more languages (I'm looking at you C#) would offer such capabilities. In the snippet above you can also see functions are first class citizens, and how to type variables.

Things you can't see in the sample, but that are just as important:

1. The `List` class you see above, is immutable. This means most operations on it actually return a new list instead of mutating it.

2. Types are optional; if the compiler can figure out the type, you can omit it.

It's hard to explain, but I felt like Scala was really forgiving and let me grow into the language. For example, as I learned, I realized more and more that I really didn't need mutable types. This of course, has a nice side effect of making parallelization of your code much easier.

In summary Scala scales with you. As you learn the language, you can express more interesting thoughts in simpler ways. Furthermore, the language pushes to clean, elegant solutions.

## Final Thoughts

If Scala required it's own runtime, I'd have to say it'd be just another functional language. But because Scala runs on the JVM, and because it can use all the Java code you currently depend on, Scala really does have a bright future.

I hope in 5 or so years we're all writing Scala instead of Java.