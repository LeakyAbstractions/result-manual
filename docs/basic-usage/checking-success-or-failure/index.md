---
title: Checking Success or Failure
description: How to find out if a Result is successful or failed
---


# Checking Success or Failure

As we saw previously, we can determine if a `Result` instance is successful or failed via [`hasSuccess()`][HAS_SUCCESS]
and [`hasFailure()`][HAS_FAILURE].


```java
@Test
void check_success_or_failure() {
  // Given
  final Result<?, ?> result1 = success(1024);
  final Result<?, ?> result2 = failure(1024);
  // When
  final boolean result1HasSuccess = result1.hasSuccess();
  final boolean result1HasFailure = result1.hasFailure();
  final boolean result2HasSuccess = result2.hasSuccess();
  final boolean result2HasFailure = result2.hasFailure();
  // Then
  assertTrue(result1HasSuccess);
  assertFalse(result1HasFailure);
  assertFalse(result2HasSuccess);
  assertTrue(result2HasFailure);
}
```


[HAS_SUCCESS]: https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#hasSuccess--
[HAS_FAILURE]: https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#hasFailure--
