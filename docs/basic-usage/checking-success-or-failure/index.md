---
title: Checking Success or Failure
description: How to find out if a Result is successful or failed
cover: >-
  https://images.unsplash.com/photo-1557318041-1ce374d55ebf?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwyfHxxdWVzdGlvbnxlbnwwfHx8fDE2ODUwMDkyNDh8MA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Checking Success or Failure

As we saw previously, we can easily determine if a `Result` instance is successful or failed.

## Checking Success

We can use [`hasSuccess()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#hasSuccess--) to find out if a result contains a success value.

```java
@Test
void check_success() {
  // Given
  final Result<?, ?> result1 = success(1024);
  final Result<?, ?> result2 = failure(1024);
  // When
  final boolean result1HasSuccess = result1.hasSuccess();
  final boolean result2HasSuccess = result2.hasSuccess();
  // Then
  assertTrue(result1HasSuccess);
  assertFalse(result2HasSuccess);
}
```

## Checking Failure

And [`hasFailure()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#hasFailure--) will let us know if a result contains a failure value.

```java
@Test
void check_failure() {
  // Given
  final Result<?, ?> result1 = success(512);
  final Result<?, ?> result2 = failure(512);
  // When
  final boolean result1HasFailure = result1.hasFailure();
  final boolean result2HasFailure = result2.hasFailure();
  // Then
  assertFalse(result1HasFailure);
  assertTrue(result2HasFailure);
}
```
