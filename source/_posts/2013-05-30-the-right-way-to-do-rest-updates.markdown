---
layout: post
title: "The Right Way To Do REST Updates"
date: 2013-05-30 22:56
comments: true
categories: [rest, api]
---

There's lots of good advice on the internet about how to design good REST APIs. I strongly agree with a lot of the principles outlined in this artcile by Xyz, so if you'd like to get some general advice, I recommend you start there.

Unfortunately, however, most of the guidance you'll find on the internet regarding REST focuses on GETs (reads) and POSTs (inserts). So today, I just wanted to offer some best practices on how to do updates.

## Gimme The Skinny

Let the consumers of your API update entities by appylying deltas instead of forcing them to replace the entire entity.

I know this isn't strictly related to REST APIs; it's more of a data API recommendation, but partial updates really do work better than full entity updates.

Why? Well, there are several reasons of course, but most of them revolve around:

* Partial updates ease update concurrency problems.

* Partial updates let you more accurately express the changes you want to make and this simplifies your code.

## Gimme More Detail

OK, you've decided to keep reading. So here's the deal: Full entity updates have the following problem:

There's a good chance the user wasn't even trying to update the "conflicting" fields. And if he is, updating the conflicting fields, does the knowledge that the data change matter to him? Why are we showing him an error?

And that's just the beggining of your problems. As soon as your user can make changes you're going to get requests like the following:

1. When the customer address is updated, notify accounting.
2. Make sure only fields x, y and z are updatable on Foo.
3. Keep a history of changes on fields a, b, and c on Bar.

How are you going to do that if all you get from you client is an entirely "new" entity? Are you going to try do diff it? That's for the birds!

Again, just have your API consumers be explicit about the fields they're changing and all your problems will go away, your life will be better, and your co-workers will be super impressed by your simple and intelligent code. :) Just kidding, you'll never impress another programmer. :P

## Gimme Me Some Code

You should check out the samples and the tests in [Voodoo](https://github.com/earaya/voodoo). [Scott Brown](https://github.com/brazilbrown) and I recently added some cool code to support partial enitity with automatic validation in Jersey.

We decided to take a rather informal representation of the data by accepting a dictionary where only the fields to be updated are present.Our Jersey endpoint therefore looks something like:

```java
@Path("/person")
public class PersonResource {
    // Other endpoints omitted.

    public Response updatePerson(@Editable(type = Person.class,
        fields = {"name", "age"}) Map personUpdates) {
        // Note: In Voodoo @Editable makes sure only the editable
        // fields defined here are being updated.
        // Voodoo also validates the values passed according to the
        // 'javax.validation` annotations on the Person class.

        // Build your query to update person here.
        // You can also raise events, based on the properties being updated here.

        // It's a good idea to return the latest representation of the entity.
        Person updatedPerson = personStore.getById(personUpdates.get("id"));
        return Response.ok(updatePerson).build();
    }
}
```


Cons

Lack of support, browsers, tools, etc.
Data update might be a bit harder (can't just save the entire thing).