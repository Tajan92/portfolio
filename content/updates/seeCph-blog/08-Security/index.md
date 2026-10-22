---
title: "seeCph: Security"
description: "Adding security features with JWT, BCrypt and endpoint user roles"
summary: "Adding security features like creating JWT to users when they login, packed with the token itself, id, name, email and the very important user roles that gives access to different endpoint depending on the specific role. Passwords are hashed and sertified using BCrypt, making it more secure."
date: 2026-10-18
project: "seecph"
header: "Security"
subheader: "18 october, 2026"
icon: "lock"
externalUrl: "blog/seecph-blog/#security"
weight: 993
categories: ["Security"]
tags: ["JWT", "BCrypt", "User Roles"]
---

Finally it was time to add some security to the application. I started by setting up a token creator and deciding what information I wanted to add and ended up with this:

```java
JWT.create()
        .withIssuer(issuer)
        .withSubject(String.valueOf(user.id()))
        .withClaim("email", user.email())
        .withClaim("name", user.name())
        .withClaim("role", user.userRole().name())
        .withIssuedAt(now)
        .withExpiresAt(now.plusMillis(expirationTime))
        .sign(signingAlgorithm(secret));
```

User role being the most important, as I wanted to set access roles on my endpoints to make sure eg. a normal user can't change an event, delete another user etc. Setting this up correctly took a lot of effort, as my code was a very different setup compared to our class examble. Specially user role, where the examble used `String` and I already had set mine up using `ENUM`, but managed to get it right in the end.

As shown in the code above I've used Json Wed Token (JWT) to setup my token. `issuer` `expirationTime` and `secret` are all set as environment variables, locally in a config.properties file thats in gitignore file and in the sky on Docker Hub in an .env file, so it's more secure.
The setup after was pretty smart and was done pretty quickly with a login endpoint -- authentication and authorization like this:

```java
public Javalin startServer(int port) {
        var app = Javalin.create(this::configuration);
        app.beforeMatched(securityHandler::authenticate);
        app.beforeMatched(securityHandler::authorize);
```

An improvement for next time could be that - users can hold more than one user role. - tokens get renewed/updated after an endpoint is hit after a set time - store tokens with an id to make sure tokens can't be used after logout or renewal

Also added hashing with BCrypt, this went pretty smooth as I've used it in projects before. But new this time was the validation of both with both creating, login, update and change password where I used Context bodyValidator.

{{< alert icon="bell" cardColor="#58585850" iconColor="#efe05baa" textColor="#fcfcfc" >}}
Next: Final overview
{{< /alert >}}
