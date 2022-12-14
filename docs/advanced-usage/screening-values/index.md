---
title: Filtering Values
description: Rejecting success values based on a predefined rule
---

# Screening Values

We can run an inline test on our wrapped success value with the [`filter()`][FILTER] method. It takes a predicate and a
mapping function as arguments and returns a `Result` object:

- If it is a failed result, or it is a successful result whose success value passes testing by the predicate then the
  `Result` is returned as-is.
- If the predicate returns `false` then the mapping function will be applied to the success value to produce a failure
  value that will be wrapped in a new failed result.

```java
@Test
void should_pass_test() {
    // Given
    final Result<Integer, String> result = success(-1);
    final Predicate<Integer> isPositive = i -> i >= 0;
    // When
    final Result<Integer, String> filtered = result.filter(isPositive, i -> "Negative number");
    // Then
    assertThat(filtered.hasFailure()).isTrue();
}
```

The `filter` method is normally used to reject wrapped success values based on a predefined rule.


[FILTER]: https://dev.leakyabstractions.com/result/javadoc/{{ site.current_version }}/com/leakyabstractions/result/Result.html#filter-java.util.function.Predicate,java.util.function.Function-
