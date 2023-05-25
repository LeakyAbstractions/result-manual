---
title: Creating Result Objects
description: How to instantiate new Result objects
cover: >-
  https://images.unsplash.com/photo-1493106641515-6b5631de4bb9?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxwb3R0ZXJ5fGVufDB8fHx8MTY4NTAyMDg2Mnww&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Creating Result Objects

There are several ways to create `Result` objects.

## Successful Results

To create a successful result, we use [`Results.success()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Results.html#success-S-).

```java
@Test
void create_successful_result() {
  // When
  final Result<Integer, ?> result = Results.success(200);
  // Then
  assertTrue(result::hasSuccess);
  assertFalse(result::hasFailure);
}
```

Note that we can use [`hasSuccess()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#hasSuccess--) or [`hasFailure()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#hasFailure--) to check if a result is successful or failed.

## Failed Results

On the other hand, if we want to create a failed result, we use [`Results.failure()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Results.html#failure-F-).

```java
@Test
void create_failed_result() {
  // When
  final Result<?, String> result = Results.failure("The operation failed");
  // Then
  assertTrue(result::hasFailure);
  assertFalse(result::hasSuccess);
}
```

## Results Based on Nullable Value

[`Results.ofNullable()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Results.html#ofNullable-S-F-) can be used to create results that depend on a possibly-null value.

```java
@Test
void create_result_based_on_nullable() {
  // Given
  final String string1 = "The operation succeeded";
  final String string2 = null;
  // When
  final Result<String, Integer> result1 = Results.ofNullable(string1, 404);
  final Result<String, Integer> result2 = Results.ofNullable(string2, 404);
  // Then
  assertTrue(result1::hasSuccess);
  assertTrue(result2::hasFailure);
}
```

## Results Based on Optional Value

We can also use [`Results.ofOptional()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Results.html#ofOptional-java.util.Optional-F-) to create results that depend on an optional value.

```java
@Test
void create_result_based_on_optional() {
  // Given
  final Optional<BigDecimal> optional1 = Optional.of(BigDecimal.ONE);
  final Optional<BigDecimal> optional2 = Optional.empty();
  // When
  final Result<BigDecimal, Integer> result1 = Results.ofOptional(optional1, -1);
  final Result<BigDecimal, Integer> result2 = Results.ofOptional(optional2, -1);
  // Then
  assertTrue(result1::hasSuccess);
  assertTrue(result2::hasFailure);
}
```

## Results Based on Callable Value

Finally, if we have a task that may either return a success value or throw an exception, we can encapsulate it as a result using [`Results.ofCallable()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Results.html#ofCallable-java.util.concurrent.Callable-), so we don't need to use a _try-catch_ block.

```java
String task1() {
  return "OK";
}

String task2() throws Exception {
  throw new Exception("Whoops!");
}

@Test
void create_result_based_on_callable() {
  // When
  final Result<String, Exception> result1 = Results.ofCallable(this::task1);
  final Result<String, Exception> result2 = Results.ofCallable(this::task2);
  // Then
  assertTrue(result1::hasSuccess);
  assertTrue(result2::hasFailure);
}
```
