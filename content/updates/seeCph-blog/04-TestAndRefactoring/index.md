---
title: "seeCph: Testing and Refactoring DAOs"
description: "Tests for basic CRUD operations and refactoring the whole DAO setup."
summary: "This week was all about getting test-ready for basic CRUD operations across all DAOs. For that, I also needed to add exceptions to my DAO methods. I decided to do a big refactoring and create a generic jig with all basic CRUD operations for easier maintenance in the future."
date: 2026-09-04
project: "seecph"
header: "Test"
subheader: "4 September 2026"
icon: "check"
externalUrl: "blog/seecph-blog/#test"
weight: 997
categories: ["@Test"]
tags: [""]
---

This week was about finishing tests for my entity DAOs' basic `CRUD` operations.

To make it possible to test properly, I needed to add exception handling to all `CRUD` operations. To make this easier and more future-proof, I added an `abstract generic <T> class` with all basic operations.

This will make it easier to maintain the code in the future, both when adding new entities or updating existing ones.

When writing the `@Test`s, I also updated my entities with overridden `equals()` and `hashCode()` methods to compare objects in the tests.

I also added `@PrePersist` and `@PreUpdate` methods to set certain variables based on others, which simplifies maintenance by keeping entity lifecycle logic directly in its own class.

```java
@PrePersist
    private void prePersist() {
        this.status = !LocalDate.now().isBefore(startDate) && !LocalDate.now().isAfter(endDate);
    }
```

```java
@PreUpdate
    private void preUpdate() {
    this.status = LocalDate.now().isBefore(startDate) && !LocalDate.now().isAfter(endDate);
    }
```

Refactoring with a generic abstract class with exception handling was challenging, but satisfying when it was done :white_check_mark:

**IntegrationTests** for `Event`, `Advert` and all `users` took a big effort to keep track of all relations and getting them right. But it helped to find some flaws in my code, to persist, update and delete everything the way I wanted it to behave.
