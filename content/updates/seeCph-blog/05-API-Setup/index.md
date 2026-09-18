---
title: "seeCph: Ticket Master API"
description: "Fetching data from Ticket Master API"
summary: "Fetching data from Ticket Master API and converting received json file into a DTO. After this using a converter to convert to entity and persist to databse."
date: 2026-08-22
project: "seeCph"
header: "API-setup"
subheader: "18 september, 2026"
icon: "list"
externalUrl: "blog/seecph-blog/#api-setup"
weight: 996
categories: ["API"]
tags: ["json", "DTO"]
---

It's was time to add `API` data, so have worked on setting up a call that gets data from Ticket Master and only events happening in Copenhagen, Denmark. Then using the received json file turn convert it to dto and then convert it to an entity, to persist it to the database.

Using `ÒbjectMapper` and `@JsonIgnoreProperties` is pretty new libaries to use. It makes it so much easier to fetch data and convert into dto.

For it to work, the dto classes / records have to be set up the right way, so it mirrors the data from json with the data you want. When setup right, it kinda feels like magic. Just make sure the dto "tree" starts from where you collect data.

It took some time to figure out exactly what data i wanted and how it should be represented in my entity. Cause I had to make it generic enough for it, to also work for other API's and data fetching, while still getting relevant data to use.

Wished I used more time for groundwork to fugure out the variables for my `Event` entity.

Found an easy way to choose category from my `ENUM` directly when the dto is made, that makes it much easier in my service class TicketMasterConverter or any new ones. But also if I need to change anything it my `ENUM` I only have to do it in one place when I don't have any switches.

```java
@JsonCreator
    public static EventCategory fromLabel(String label) {
        if (label == null) return OTHERS;
        return events.getOrDefault(label.toLowerCase(), OTHERS);
    }
```

And then every `ENUM` has String label to catch and give the right one.

{{< alert icon="bell" cardColor="#58585850" iconColor="#efe05baa" textColor="#fcfcfc" >}}
**Next:** User validation, register, login etc.
{{< /alert >}}
