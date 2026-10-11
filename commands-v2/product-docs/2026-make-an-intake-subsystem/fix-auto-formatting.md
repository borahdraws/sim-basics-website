---
description: >-
  VS Code is messing with my formatting and when I try to make some indentation
  or something it flattens everything out
icon: cloud-hail-mixed
---

# Fix auto formatting

I was having this issue where my code would go from this:

{% code overflow="wrap" %}
```java
  controller
    .leftBumper()
    .whileTrue(
      intake.moveForwards()
    );
```
{% endcode %}

to this after building and running it :(&#x20;

{% code overflow="wrap" %}
```java
  controller.leftBumper().whileTrue(intake.moveForwards());
```
{% endcode %}

It's the same thing but very annoying for me who prefers code formatted in a certain way

## Fix auto formatting

Find the build.gradle file.

There will be a block of code that says spotless. Delete everything inside spotless to prevent auto formatting
