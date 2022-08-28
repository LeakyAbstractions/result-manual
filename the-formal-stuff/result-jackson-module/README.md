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

## Problem Overview

First, let's take a look at what happens when we try to serialize and deserialize ApiResponse objects with Jackson.

To use Jackson, let's make sure we're using the latest version:

* groupId: `com.fasterxml.jackson.core`
* artifactId: `jackson-core`
* version: `2.13.3`

### Serialization Problem

Now, let's instantiate an `ApiResponse`:

```java
ApiResponse response = new ApiResponse();
response.setVersion("v1");
response.setResult(success(1024));
```

And finally, let's try serializing it using an [object mapper](https://www.baeldung.com/jackson-object-mapper-tutorial):

```java
ObjectMapper objectMapper = new ObjectMapper();
String json = objectMapper.writeValueAsString(response);
```

We'll see that the output of the Result field contains both `success` and `failure` but they look like nested JSON objects with a boolean field `empty`:

```json
{
  "version": "v1",
  "result": {
    "failure": {
      "empty": true
    },
    "success": {
      "empty": false
    }
  }
}
```

> You may also see a `present` field along with `empty`, depending on the JDK version you are using.

Although this may look strange, it's actually what we should expect. In this case, `getSuccess()` and `getFailure()` are public getters on the Result class, and both of them return an optional value. Now Optional class has a public getter `isPresent()`, which means that, unless you have registered the [Jackson modules that deal with JDK 8 datatypes](https://github.com/FasterXML/jackson-modules-java8), it will be serialized with a value of `true` or `false`, depending on whether it is empty or not. This is Jackson's default serialization behavior.

```java
@Test
void serialization_problem() throws Exception {
  // Given
  ApiResponse response = new ApiResponse("v1", success(1024));
  // When
  ObjectMapper objectMapper = new ObjectMapper();
  String json = objectMapper.writeValueAsString(response);
  // Then
  assertTrue(json.contains("v1"));
  assertFalse(json.contains("1024"));
}
```

In fact, what we'd like to be serialized is the actual success value of the `result` field:

```json
{
  "version": "v1",
  "result": {
    "failure": null,
    "success": 1024
  }
}
```

### Deserialization Problem

Now, let's reverse our previous example, this time trying to deserialize a JSON object into an ApiResponse:

```java
String json = "{\"version\":\"v2\",\"result\":{\"success\":512}}";
ObjectMapper objectMapper = new ObjectMapper();
objectMapper.readValue(json, ApiResponse.class);
```

We'll see that now we get an `InvalidDefinitionException`. Let's view the stack trace:

```
Cannot construct instance of `com.leakyabstractions.result.Result`
 (no Creators, like default constructor, exist):
 abstract types either need to be mapped to concrete types,
 have custom deserializer, or contain additional type information
 at [Source: (String)"{"version":"v2","result":{"success":512}}"; line: 1, column: 26]
 (through reference chain: ApiResponse["result"])
```

This behavior again makes sense. Essentially, Jackson doesn't have a clue how to create new Result objects, because Result is just an interface, not a concrete type.

```java
@Test
void deserialization_problem() {
  // Given
  String json = "{\"version\":\"v2\",\"result\":{\"success\":512}}";
  // Then
  ObjectMapper objectMapper = new ObjectMapper();
  InvalidDefinitionException error = assertThrows(InvalidDefinitionException.class,
      () -> objectMapper.readValue(json, ApiResponse.class));
  assertTrue(error.getMessage().startsWith(
      "Cannot construct instance of `com.leakyabstractions.result.Result`"));
}
```

## Solution

What we want, is for Jackson to treat Result values as JSON objects that contain either a `success` or a `failure` value. Fortunately, this problem has been solved for us. [This library](https://github.com/LeakyAbstractions/result-jackson/) provides a Jackson module that deals with Result objects.

First, let's add the latest version as a Maven dependency:

* groupId: `com.leakyabstractions`
* artifactId: `result-jackson`
* version: `{{ site.current_version }}`

All we need to do now is register ResultModule with our object mapper:

```java
ObjectMapper objectMapper = new ObjectMapper();
objectMapper.registerModule(new ResultModule());
```

Alternatively, you can also auto-discover the module with:

```java
objectMapper.findAndRegisterModules();
```

Regardless of registration mechanism, after registration all functionality is available for all normal Jackson operations.

### Serialization Solution

Now, let's try and serialize our `ApiResponse` object again:

```java
@Test
void serialization_solution_successful_result() throws Exception {
  // Given
  ApiResponse response = new ApiResponse("v3", success(256));
  // When
  ObjectMapper objectMapper = new ObjectMapper();
  objectMapper.registerModule(new ResultModule());
  String json = objectMapper.writeValueAsString(response);
  // Then
  assertTrue(json.contains("v3"));
  assertTrue(json.contains("256"));
}
```

If we look at the serialized response, we'll see that this time the `result` field contains a null `failure` value and a non-null `success` value:

```json
{
  "version": "v3",
  "result": {
    "failure": null,
    "success": 256
  }
}
```

Next, we can try serializing a failed result:

```java
@Test
void serialization_solution_failed_result() throws Exception {
  // Given
  ApiResponse response = new ApiResponse("v4", failure("Hello"));
  // When
  ObjectMapper objectMapper = new ObjectMapper();
  objectMapper.findAndRegisterModules();
  String json = objectMapper.writeValueAsString(response);
  // Then
  assertTrue(json.contains("v4"));
  assertTrue(json.contains("Hello"));
}
```

And we can verify that the serialized response contains a non-null `failure` value and a null `success` value:

```json
{
  "version": "v4",
  "result": {
    "failure": "Hello",
    "success": null
  }
}
```

### Deserialization Solution

Now, let's repeat our tests for deserialization. If we reread our `ApiResponse` we'll see that we no longer get an `InvalidDefinitionException`:

```java
@Test
void deserialization_solution_successful_result() throws Exception {
  // Given
  String json = "{\"version\":\"v5\",\"result\":{\"success\":128}}";
  // When
  ObjectMapper objectMapper = new ObjectMapper().findAndRegisterModules();
  ApiResponse response = objectMapper.readValue(json, ApiResponse.class);
  // Then
  assertEquals("v5", response.getVersion());
  assertEquals(128, response.getResult().orElse(null));
}
```

Finally, let's repeat the test again, this time with a failed result. We'll see that yet again we don't get an exception, and in fact, have a failed Result:

```java
@Test
void deserialization_solution_failed_result() throws Exception {
  // Given
  String json = "{\"version\":\"v6\",\"result\":{\"failure\":\"Bye\"}}";
  // When
  ObjectMapper objectMapper = new ObjectMapper().findAndRegisterModules();
  ApiResponse response = objectMapper.readValue(json, ApiResponse.class);
  // Then
  assertEquals("v6", response.getVersion());
  assertEquals("Bye", response.getResult().getFailure().orElse(null));
}
```

## Conclusion

We've shown how to use Result with Jackson without any problems by leveraging the [Jackson datatype module for Result](https://github.com/LeakyAbstractions/result-jackson/), demonstrating how it enables Jackson to treat instances of Result objects as an ordinary fields.

The implementation of these examples can be found [here](https://github.com/LeakyAbstractions/result-jackson/blob/main/lib-jackson/src/test/java/com/leakyabstractions/result/jackson/Example\_Test.java).
