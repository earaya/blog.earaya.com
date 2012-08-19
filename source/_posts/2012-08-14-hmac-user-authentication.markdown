---
layout: post
title: "HMAC User Authentication"
date: 2012-08-14 22:07
comments: true
categories: 
---

[@derrickisaacson](http://twitter.com/derrickisaacson) recently showed me a really slick way to do user authentication with HMACs. Here's a real quick overview on how to accomplish it:

###User Sign In

One of the cool things about using HMACs for authentication is that you can keep your current sign in process. Validate user credentials however you see fit, and then, upon credential validation, hand your user something like the following token:

`<userId, timestamp, hmac(userId + timestamp)>`

The idea behind this is that once a user has proved his identity, we can hand them the above token as an authentication code that will be able to prove their identity going forward. The client, of course, will have to submit the token on every subsequent request to the server so, if your client is a browser, you may want to consider sending the token down in a cookie.

_A note of caution:_ if an attacker is able to intercept and or sniff the token when sent to the client, all bets are off and this, and just about any other authentication scheme, provides no protection. Please make sure you encrypt your client connection when using the idea describe here.

While you have to worry about safely delivering the token, you don't have to worry about legitimate users trying to modify the token. This is because even though clients will have access to the token, they can't recalculate the HMAC of the data handed to them; they don't have the key that your server used to calculate the hash.

###Request authentication

Now that your client has the necessary token to make authenticated requests, all you have to do is verify the token. This is accomplished by recalculating the HMAC of the token they've sent you and comparing it to the one you just produced on your server. Here's some pseudocode to make the point clearer:

```java

HmacToken clientToken = HmacToken.parse(stringWithTokenFromClient);
HmacToken serverToken = HmacTokenGenerator.generateToken(clientToken.getUserId(), clientToken.getTimestamp());

return clientToken.getSignature() == serverToken.getSignature();

```

You can adjust the above code to also validate the timestamp and reject the token if it's too old (and require the user to sign in again), but you ge the point.

One of the best things of using this idea for authentication is that you can now validate requests without keeping any sort of state; every request contains all the information you need to validate it. No need to keep any sort of session tokens around, and no need to do any kind of lookups.




