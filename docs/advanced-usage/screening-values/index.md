---
title: Filtering Values
description: Conditionally rejecting success values and accepting failure values
---

# Screening Results

Often, something that a lower logic layer considers a _success_ can be considered a _failure_ by an upper layer. Or _vice versa_.

Results provide convenient methods for rejecting success values as well as accepting failure values, based on a predefined rule that is represented by a [`Predicate`](https://docs.oracle.com/en/java/javase/18/docs/api/java.base/java/util/function/Predicate.html).

## Rejecting Success

We can run an inline test on our wrapped success value with [`Result::filter`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#filter-java.util.function.Predicate-java.util.function.Function-). This method takes a predicate and a mapping function as arguments and returns a possibly-new result object:

* If the result has a success value that doesn't satisfy the predicate, `filter` will return a new result object containing the failure value produced by the mapping function.
* Otherwise, the result will be returned as-is.

```java
@Test
void should_pass_test() {
  // Given
  final Result<Integer, String> result = success(-1);
  final Predicate<Integer> isPositive = i -> i >= 0;
  // When
  final Result<Integer, String> filtered = result
      .filter(isPositive, i -> "Negative number");
  // Then
  assertThat(filtered.hasFailure()).isTrue();
}
```

{% hint style="info" %}
Note that it is illegal for a mapping function to return `null`.
{% endhint %}

```java
@Test
void filter_successful_result_to_null() {
  // Given
  final Result<Pet, PetError> result = createPet("Rantanplan", AVAILABLE);
  // When
  final Throwable thrown = catchThrowable(() -> result.filter(s -> false, s -> null));
  // Then
  assertThat(thrown)
      jav.isInstanceOf(NullPointerException.class)
      .hasMessage("failure value returned by mapper");
}
```

## Recovering From Failure

We can run an inline test on our wrapped failure value with [`Result::recover`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#recover-java.util.function.Predicate-java.util.function.Function-). This method takes a predicate and a mapping function as arguments and returns a possibly-new `Result` object:

* If the result has a failure value that satisfies the predicate, `recover` will return a new result object containing the success value produced by the mapping function.
* Otherwise, the result will be returned as-is.

```java
@Test
void recover_failed_result() {
  // Given
  final Result<Pet, PetError> result1 = failure(new PetError("Not found"));
  final Result<Pet, PetError> result2 = createPet("Foo", SOLD);
  final Predicate<PetError> predicate = x -> x.message.equals("Not found");
  final Function<PetError, Pet> mapper = x -> new Pet("Fallback pet", AVAILABLE);
  // When
  final Result<Pet, PetError> mapped1 = result1.recover(predicate, mapper);
  final Result<Pet, PetError> mapped2 = result2.recover(predicate, mapper);
  // Then
  assertEquals("Fallback pet", mapped1.orElse(null).name);
  assertEquals("Foo", mapped2.orElse(null).name);
}
```

{% hint style="info" %}
Note that it is illegal for a mapping function to return `null`.
{% endhint %}

<pre class="language-java"><code class="lang-java"><strong>@Test
</strong>void recover_failed_result_to_null() {
  // Given
  final Result&#x3C;Pet, PetError> result = failure(new PetError("Not found"));
  // When
  final Throwable thrown = catchThrowable(() -> result
      .recover(s -> true, s -> null));
  // Then
  assertThat(thrown)
      .isInstanceOf(NullPointerException.class)
      .hasMessage("success value returned by mapper");
}
</code></pre>
