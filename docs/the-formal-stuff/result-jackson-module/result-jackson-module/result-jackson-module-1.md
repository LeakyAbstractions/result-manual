---
description: Result-Jackson provides a Jackson datatype module for Result objects
---

# Deserialization Problem



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

