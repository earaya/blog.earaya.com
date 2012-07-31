---
layout: post
title: "Using Char Arrays For Passwords"
date: 2012-07-25 14:52
comments: true
categories: [security, java, .net]
---

Just a few days ago a co-worker asked me about mongodb's choice of `char[]` for the password in their [authenticate](http://api.mongodb.org/java/current/com/mongodb/DB.html#authenticate(java.lang.String, char[]\)) call. He was wondering why they chose`char[]` over `String`.

The reason is simple: some people feel that using a `char[]` is safer since you can "wipe out" the array after you're done with the data (eg. the password). `String` being an immutable type does not allow this precaution; once you're done with the `String` it'll live in memory until garbage collection decides to claim it at an undetermined time in the future.

The implication, of course, is that an attacker may read the contents of the memory before the `String` is garbage collected and steal the password.

This, however, seems a moot to me. Switching to a `char[]` does not really solve the problem; it only reduces the time window in which the memory contents are available to an attacker. This is even worse than [security through obscurity](http://en.wikipedia.org/wiki/Security_through_obscurity); really, this is just giving up and pretending to do something to address the problem. I'll also say that if someone is able to read the contents of your memory space, you have bigger problems than using a `String` for your password.

Nonetheless, there is one slight advantage in using character arrays for sensitive data: you're less likely to accidentally print the contents of an `Array` (to a log, or console, etc.) than the contents of a `String`. 

```java
	char[] secret = "Secret".toCharArray();
	System.out.println("Value: " + secret);
```

The above code, for example, does not print "Secret". Rather, it prints the `@` character followed by the unsigned hexadecimal representation of the hash code of the object. This is because `Array` does not override `Object`'s implementation of `toString`, so you just get the default behavior (see [documentation](http://docs.oracle.com/javase/1.4.2/docs/api/java/lang/Object.html#toString(\))). Still, I don't think this is enough "security" to warrant using a character array.

The real solution, would be to implement something like .NET's [`SecureString`](http://msdn.microsoft.com/en-us/library/system.security.securestring.aspx). A `SecureString` object behaves a lot like a regular `String` object except that its memory contents are automatically encrypted. Furthermore, a `SecureString` instance is immediately discarded when no longer in use. This approach offers real security and it gives developers are more appropriate API when handling strings.

If it wasn't for the fact that it sucks to handle strings as character arrays, I'd say that the marginal security benefits of using a `char[]` are worth it. But really, there's no way to rationally justify this. Just keep using a `String`, or if you really care, find a good `SecureString` implementation.