---
title: Creating Result Objects
description: How to instantiate new Result objects
---


# Creating Result Objects

There are several ways to create `Result` objects.


## Successful Results

To create a successful result, we use [`Results.success()`][NEW_SUCCESS].

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

Note that we can use [`hasSuccess()`][HAS_SUCCESS] or [`hasFailure()`][HAS_FAILURE] to check if a result is successful
or failed.


## Failed Results

On the other hand, if we want to create a failed result, we use [`Results.failure()`][NEW_FAILURE].

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

[`Results.ofNullable()`][OF_NULLABLE] can be used to create results that depend on a possibly-null value.

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

We can also use [`Results.ofOptional()`][OF_OPTIONAL] to create results that depend on an optional value.

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

Finally, if we have a task that may either return a success value or throw an exception, we can encapsulate it as a
result using [`Results.ofCallable()`][OF_CALLABLE], so we don't need to use a _try-catch_ block.

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


[NEW_SUCCESS]: https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Results.html#success-S-
[NEW_FAILURE]: https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Results.html#failure-F-
[OF_NULLABLE]: https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Results.html#ofNullable-S-F-
[OF_OPTIONAL]: https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Results.html#ofOptional-java.util.Optional-F-
[OF_CALLABLE]: https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Results.html#ofCallable-java.util.concurrent.Callable-
[HAS_SUCCESS]: https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#hasSuccess--
[HAS_FAILURE]: https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#hasFailure--
