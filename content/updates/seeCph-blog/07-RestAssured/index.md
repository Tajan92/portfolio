---
title: "seeCph: RestAssured"
description: "Testing endpoints with RestAssured test with positive and negative test"
summary: "Testing endpoints with RestAssured test with positive and negative test. Creating a webhook using GitHub Actions to hit endpoint, to activate daily syncs of my API events"
date: 2026-10-02
project: "seecph"
header: "RestAssured"
subheader: "2 october, 2026"
icon: "check"
externalUrl: "blog/seecph-blog/#restassured"
weight: 994
categories: ["RestAssured"]
tags: ["Test", "Web-Hook"]
---

### RestAssured

After creating the **endpoints**, I needed to test if they actually worked as intended. I started by manually testing the connection with an HTTP test on **localhost**, just a quick positive test to see if it behaved as designed.

Next came the `RestAssured` tests to verify that my handlers returned the correct status codes across different scenarios, covering both positive and negative results. All of this helped ensure the application was working the way I planned.

Writing failure tests can be a bit tricky to get right, but as I worked through them, it got easier and easier. Since I had already tested my service layer, I had previously caught and corrected a few mistakes and behaviors in my code. The `RestAssured` tests mainly focused on validating my handlers' contexts, which went pretty smoothly, though I did notice I needed better error and status handling in a few places.

### Web-Hook

I also implemented a `webhook` to sync my API events once a day during off-hours at night, when server traffic is minimal. It is set up with **GitHub Actions** to hit this endpoint:

```java
/api/v1/events/ticketmaster
```

When sending a request to the endpoint, it also passes a key/password to check against a system environment variable before running the sync. This ensures it cannot be hit by unauthorized users. Once validated, it makes a new thread, running in the background, to run the syncs. This allows the main thread to immediately return a **200 OK** status back to **GitHub**, preventing the HTTP request from timing out if the sync takes too long.

{{< alert icon="bell" cardColor="#58585850" iconColor="#efe05baa" textColor="#fcfcfc" >}}
Next: User validation using **web-tokens** and setting up **BCrypt** hashing
{{< /alert >}}
