---
title: "seeCph: Test and Refactoring DAO's"
description: "Test for basic CRUD operations and refactoring the whole DAO setup."
summary: "This week was all getting test ready for all basic CRUD operations for all DAOS. For that i also needed to add exceptions to my DAO methods. I decided to do a big refactoring and make a generic Jig with all the basic CRUD operations for easier maintainance further on."
date: 2026-09-04
project: "seecph"
header: "Test"
subheader: "4 september, 2026"
icon: "check"
externalUrl: "blog/seecph-blog/#test"
weight: 997
categories: ["@Test"]
tags: [""]
---

This week was about finishing test for my entity DAO's basic `CRUD` operations.

To make it possible to test properly, I needed to add exception handling in all `CRUD` operations. To make this easier and more future proof, I added an `abstact generic<T> class` to have a jig for all basic operations.

This will make it easier to maintain the code in the future, both when adding new entities or updating it.

When making `@Test` I also updated my entities with `equals()` and `hashcode()`, `@Override` methods to compare objects in test.

Also added `@PrePersist` and `@PreUpdate` methods to set some variables from others, which will make it easier to maintain by having service methods directly in its own class.

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

Refactoring with a generic abstract class with exception handling was challenging, but satisfying when done :white_check_mark:

{{< alert icon="bell" cardColor="#58585850" iconColor="#efe05baa" textColor="#fcfcfc" >}}
**Next:** I'll be working on gathering my first Data from an API call
{{< /alert >}}
