---
description: Result-Jackson provides a Jackson datatype module for Result objects
---

# Serialization Problem

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

