---
title: Adding Result to Your Build
description: How to add Result as a dependency to your build
---


# Adding Result to Your Build

The library requires JDK 1.8 or higher. Other than that, it has no external dependencies and it is very lightweight.
Adding it to your build should be very easy.


## Artifact coordinates

- Group ID: `com.leakyabstractions`
- Artifact ID: `result`
- Version: `1.0.0.0`


## Maven

To use `Result`, we can add a [**Maven**][MAVEN] dependency to our project.

```xml
<dependencies>
    <dependency>
        <groupId>com.leakyabstractions</groupId>
        <artifactId>result</artifactId>
        <version>1.0.0.0</version>
        <type>pom</type>
    </dependency>
</dependencies>
```


## Gradle

We can also add `Result` as a [**Gradle**][GRADLE] dependency.

```gradle
dependencies {
    implementation 'com.leakyabstractions:result:1.0.0.0'
}
```

This is the most common configuration when we are building an application that uses `Result` internally. If we were
building a library that exposed `Result` in its public API, we should use instead:

```gradle
dependencies {
    api 'com.leakyabstractions:result:1.0.0.0'
}
```

> For more information on when to use `api` and `implementation`, read the
> [Gradle documentation on API and implementation separation][GRADLE_LIBRARY_SEPARATION].


[MAVEN]: https://maven.apache.org/
[GRADLE]: https://gradle.org/
[GRADLE_LIBRARY_SEPARATION]: https://docs.gradle.org/current/userguide/java_library_plugin.html#sec:java_library_separation
