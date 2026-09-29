---
title: "seeCph: Endpoints"
description: "Figuring out how to set up handlers, controllers and their endpoints"
summary: "Figuring out how to set up handlers, controllers and their endpoints. Handlers with javalins Validator and controllers with javalins EndPoint Group interface."
date: 2026-09-25
project: "seecph"
header: "Endpoints"
subheader: "25 september, 2026"
icon: "tag"
externalUrl: "blog/seecph-blog/#endpoints"
weight: 995
categories: ["Endpoints"]
tags: ["Interface", "Dependency injection"]
---

Figuring out how to structure my code to handle my endpoints took a lot of time. The goal was to make the code readable and easy to maintain. Structuring it this way helps keep my classes aligned with the Single Responsibility Principle. For now, I ended up structuring it like this:

{{< alert icon="none" cardColor="#58585850" textColor="#fcfcfc" >}}

{{< icon "globe" >}} **HTTP Request:** User input<br>
&darr; **Controller:** Adds endpoints<br>
&darr; **Handler:** Manages user input validation & HTTP responses<br>
&darr; **Service:** Executes business logic<br>
&darr; **DAO:** Interacts with the database
{{< /alert >}}

- {{< icon "a11y" >}} **Handlers:** Takes care of what happens when calling the different routes, manage user input validation using Javalin's `Validator`, and set/return HTTP status messages. I will also implement a `Logger` here later on.

- {{< icon "globe" >}} **Controllers:** Takes care of linking routes up with the methods in handlers. Here, I use Javalin's `EndpointGroup` interface to add all routes in each controller.

- {{< icon "fork" >}} **Dependency Injection:** To handle dependency injection, I created an `ApplicationConfig` class that makes sure DAOs, services, handlers, and controllers get what they need, and adds all controller endpoints in one method.

```java
public class ApplicationConfig implements EndpointGroup {

    private final EventController eventController;
    private final UserController userController;
    private final AdvertController advertController;

    public ApplicationConfig(EntityManagerFactory emf) {
        // DAOs
        AdvertDAO advertDAO = new AdvertDAO(emf);
        EventDAO eventDAO = new EventDAO(emf);
        UserDAO userDAO = new UserDAO(emf);

        // Services
        EventService eventService = new EventService(eventDAO);
        UserService userService = new UserService(userDAO);
        AdvertService advertService = new AdvertService(advertDAO);

        // Handlers
        EventHandler eventHandler = new EventHandler(eventService);
        UserHandler userHandler = new UserHandler(userService);
        AdvertHandler advertHandler = new AdvertHandler(advertService);

        // Controllers
        this.eventController = new EventController(eventHandler);
        this.userController = new UserController(userHandler);
        this.advertController = new AdvertController(advertHandler);
    }

    @Override
    public void addEndpoints() {
        eventController.addEndpoints();
        userController.addEndpoints();
        advertController.addEndpoints();
    }
}
```

If `ApplicationConfig` gets too big, the plan is to split it up into categories so it remains readable and easy to maintain.

To keep the code easier to maintain and build on, I use interfaces to provide a blueprint for new additions and keep method names consistent.

{{< alert icon="bell" cardColor="#58585850" iconColor="#efe05baa" textColor="#fcfcfc" >}}
**Next:** I think will be Rest-assured test of my end points.
{{< /alert >}}
