---
title: Jackson Datatype Module for Result
description: Result-Jackson provides a Jackson datatype module for Result objects
cover: >-
  https://raw.githubusercontent.com/LeakyAbstractions/result/main/docs/result-banner.png
coverY: 417.2062084257206
---

# Jackson Module

This library provides a Jackson datatype module for [results](https://dev.leakyabstractions.com/result/).

## Introduction

When using [Result objects](https://github.com/LeakyAbstractions/result/) with [Jackson](https://github.com/FasterXML/jackson) we might run into some problems. [This library](https://github.com/LeakyAbstractions/result-jackson/) solves them by making Jackson treat results as if they were ordinary objects.

Let's start by creating a class `ApiResponse` containing one ordinary and one Result field:

```java
class ApiResponse {

  @JsonProperty
  String version;

  @JsonProperty
  Result<Integer, String> result;

  // Constructors, getters and setters omitted
}
```

