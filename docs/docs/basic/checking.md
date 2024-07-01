---
title: Checking Success or Failure
description: How to find out if the operation succeded or failed
---

# Checking Success or Failure

As we discovered earlier, we can easily determine if a given `Result` instance is successful or not.

## Checking Success

We can use [`Result::hasSuccess`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#hasSuccess--) to obtain a `boolean` value that represents whether a result is successful.

```java
@Test
void testHasSuccess() {
  // Given
  Result<?, ?> result1 = success(1024);
  Result<?, ?> result2 = failure(1024);
  // When
  boolean result1HasSuccess = result1.hasSuccess();
  boolean result2HasSuccess = result2.hasSuccess();
  // Then
  assertTrue(result1HasSuccess);
  assertFalse(result2HasSuccess);
}
```

## Checking Failure

We can also use [`Result::hasFailure`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#hasFailure--) to find out if a result contains a failure value.

```java
@Test
void testHasFailure() {
  // Given
  Result<?, ?> result1 = success(512);
  Result<?, ?> result2 = failure(512);
  // When
  boolean result1HasFailure = result1.hasFailure();
  boolean result2HasFailure = result2.hasFailure();
  // Then
  assertFalse(result1HasFailure);
  assertTrue(result2HasFailure);
}
```
