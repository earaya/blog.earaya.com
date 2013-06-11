---
layout: post
title: "The Right Way To Do REST Updates"
date: 2013-05-30 22:56
comments: true
categories: [rest, api]
---

There's lots of good advice on the internet about how to design good REST APIs. The folks at [apigee](http://apigee.com/about/api-best-practices) have tons of great articles and really know their stuff, so if you are looking for some general advice on REST, you should start there.

Most of the guidance you'll find on the internet regarding REST, however, focuses on GETs (reads) and POSTs (inserts). Today, therefore, I want to offer advice on how to do updates.

## Gimme The Skinny

Let the consumers of your API update resources by appylying deltas instead of forcing them to replace the entire resource.

I know this isn't strictly related to REST APIs; it's more of a data API recommendation, but partial updates really do work better than full resource updates.

Why? Well, there are several reasons of course, but most of them revolve around:

* Partial updates ease update concurrency problems.

* Partial updates let you more accurately express the changes you want to make and this simplifies your code.

One final recommendation regarding REST: use the HTTP PATCH method instead of PUT to do partial updates. PATCH was specifically introduced in March 2010 to allow [partial resource modification](http://tools.ietf.org/html/rfc5789).

## Gimme More Detail

OK, you've decided to keep reading (thanks!). So here's the deal: Full resource updates (replacements) have the following problem:

{% img /images/posts/dotnet-concurrency-control.jpg 'A common problem with full updates' %}

Pretty ugly, huh?

There's a good chance the user wasn't even trying to update the "conflicting" fields. And if he was updating the conflicting fields, does the knowledge that the data changed matter to him? Why are we showing him an error? What is he supposed to do about it? Really, there's not a lot of good solutions here.

And then there's even more problems. As soon as your user can make changes you're going to get requirements like the following:

1. When the customer address is updated, notify accounting.
2. Make sure only fields x, y and z are updatable on Foo.
3. Keep a history of changes on fields a, b, and c on Bar.

How are you going to do that if all you get from you client is an entirely "new" resource? Are you going to diff it and derive what changed? That's hard and usually ends up being really messy.

Again, just have your API consumers be explicit about the fields they're changing and all your problems will go away, your life will be better, and your co-workers will be super impressed by your simple and intelligent code. :) Just kidding, you'll never impress another programmer, so don't even bother. :P

## Gimme Me Some Code

You should check out the samples and the tests in [Voodoo](https://github.com/earaya/voodoo). [Scott Brown](https://github.com/brazilbrown) and I recently added some cool code to support partial enitity with automatic validation in Jersey.

We decided to take a rather informal representation of the data by accepting a dictionary where only the fields to be updated are present. Our Jersey endpoint therefore looks something like:

```java
@Path("/person")
public class PersonResource {
    // Other endpoints omitted.

    @PATCH
    @Path("/{id}")
    public Response updatePerson(@PathParam("id") String id, @Editable(type = Person.class,
        fields = {"name", "age"}) Map personUpdates) {
        // Note: In Voodoo @Editable makes sure only the editable
        // fields defined here are being updated.
        // Voodoo also validates the values passed according to the
        // 'javax.validation` annotations on the Person class.

        // Build your query to update person here.
        // You can also raise events, based on the properties being updated here.

        // It's a good idea to return the latest representation of the entity.
        Person updatedPerson = personStore.getById(personUpdates.get(id));
        return Response.ok(updatePerson).build();
    }
}
```

As you can see, the approach is rather straightforward. I think the `@Editable` annotation actually turned out pretty well: it abstracts away all the validation code from your endpoint.

## A few parting thoughts

Of course not everything is perfect with this approach; there are a few minor drawbacks. Chief among these is the fact that there's a good chunk of tooling that doesn't quite support PATCH yet.

Backbone for example, has recently added support for PATCH (in IE) but hasn't put it into any of their releases yet. We've had similar problems with [Swagger](https://github.com/wordnik/swagger-core); the method isn't really supported there either.

One other minor consideration is that if you don't really need any intelligence around what's changing, the code to do a full resource update is usually simpler.

So there you go: if you want to be awesome, start using PATCH and supporting partial resource updates. Otherwise, you can keep doing the same old thing you've been doing. :)