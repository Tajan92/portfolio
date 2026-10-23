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

Finally, it was time to add security to the application. I started by configuring a token creator to determine what information to include, resulting in this setup:

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

The user role was particularly important, as I needed to enforce access control on my endpoints—ensuring, for example, that a standard user cannot modify events or delete other users. Getting this right took some effort because my codebase structure differed significantly from our class example. Most notably, the example used a String for roles, whereas I had already implemented mine using an Enum, but I managed to get it working smoothly in the end.

The values used to create the token, such as the secret key, issuer and token expiration time, are never hardcoded or committed. Locally they are read from a `config.properties` file that is listed in `.gitignore`. When deployed, the application reads the same values from environment variables instead. On the DigitalOcean server, Docker Compose loads them from a `.env` file and passes them to the container when it starts, so the Docker image itself contains no secrets. During the build, GitHub Actions gets the values it needs to run the tests from GitHub Secrets.

The setup for the login endpoint, along with the authentication and authorization handlers, came together quickly and cleanly:

```java
public Javalin startServer(int port) {
        var app = Javalin.create(this::configuration);
        app.beforeMatched(securityHandler::authenticate);
        app.beforeMatched(securityHandler::authorize);
        // ...
}
```

Potential improvements for next time:

- Allowing users to hold multiple roles.

- Implementing token renewal or refresh mechanisms after a set period of activity.

- Storing token identifiers to ensure tokens can be invalidated immediately upon logout or renewal.

I also integrated BCrypt for password hashing, which went smoothly since I have used it in previous projects. However, a new addition this time was implementing robust input validation using `Context.bodyValidator()` across user creation, login, updates, and password changes.

{{< alert icon="bell" cardColor="#58585850" iconColor="#efe05baa" textColor="#fcfcfc" >}}
Next: Final overview
{{< /alert >}}
