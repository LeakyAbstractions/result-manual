---
description: Result-Jackson provides a Jackson datatype module for Result objects
---

# Solution

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

