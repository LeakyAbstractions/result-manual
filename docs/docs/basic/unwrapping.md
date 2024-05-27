---
title: Unwrapping Values
description: How to get values out of Result objects
---

# Unwrapping Values

In a nutshell, a `Result` object is just a container that wraps either a success or a failure value for us. Sometimes we simply need to get that value out of the container.

{% hint style="info" %}
As useful as this may seem, we will soon realize that we won't be doing it that often.
{% endhint %}

## Unwrapping Success Value

The most basic way to retrieve the success value wrapped inside a `Result` instance is by invoking [`Result::getSuccess`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#getSuccess--). This method will return an optional success value, depending on whether the result was actually successful or not.

```java
@Test
void testGetSuccess() {
  // Given
  Result<?, ?> result1 = success("SUCCESS");
  Result<?, ?> result2 = failure("FAILURE");
  // Then
  Optional<?> success1 = result1.getSuccess();
  Optional<?> success2 = result2.getSuccess();
  // Then
  assertEquals("SUCCESS", success1.get());
  assertTrue(success2::isEmpty);
}
```

## Unwrapping Failure Value

Similarly, we can use [`Result::getFailure`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#getFailure--) to obtain the failure value held by a `Result` object.

```java
@Test
void testGetFailure() {
  // Given
  Result<?, ?> result1 = success("SUCCESS");
  Result<?, ?> result2 = failure("FAILURE");
  // Then
  Optional<?> failure1 = result1.getFailure();
  Optional<?> failure2 = result2.getFailure();
  // Then
  assertTrue(failure1::isEmpty);
  assertEquals("FAILURE", failure2.get());
}
```

{% hint style="success" %}
Unlike [`Optional::get`](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html#get--), these methods are null-safe. However, in practice, we will not be using them frequently. Especially, since there are more convenient ways to get the success value out of a result.
{% endhint %}

## Using Alternative Success Value

We can for example use [`Result::orElse`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#orElse-S-) to provide an alternative success value that must be returned when the result is unsuccessful.

```java
@Test
void testGetOrElse() {
  // Given
  Result<String, String> result1 = success("IDEAL");
  Result<String, String> result2 = failure("ERROR");
  String alternative = "OTHER";
  // When
  String value1 = result1.orElse(alternative);
  String value2 = result2.orElse(alternative);
  // Then
  assertEquals("IDEAL", value1);
  assertEquals("OTHER", value2);
}
```

{% hint style="info" %}
Note that alternative success values can be `null`.
{% endhint %}

## Mapping Failure Value

The [`Result::orElseMap`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#orElseMap-java.util.function.Function-) method is similar to `orElse()` but it takes a mapping function instead of an alternative success value. The function will receive the failure value to produce the alternative success value:

```java
@Test
void testGetOrElseMap() {
  // Given
  Result<String, Integer> result1 = success("OK");
  Result<String, Integer> result2 = failure(1024);
  Result<String, Integer> result3 = failure(-256);
  Function<Integer, String> mapper = x -> x > 0 ? "HI" : "LO";
  // When
  String value1 = result1.orElseMap(mapper);
  String value2 = result2.orElseMap(mapper);
  String value3 = result3.orElseMap(mapper);
  // Then
  assertEquals("OK", value1);
  assertEquals("HI", value2);
  assertEquals("LO", value3);
}
```

{% hint style="info" %}
Note that the mapping function may return `null` (it wouldn't be the best practice though).
{% endhint %}

## Streaming Success/Failure Values

Finally, we can use [`Result::streamSuccess`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#streamSuccess--) and [`Result::streamFailure`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#streamFailure--) to wrap the value held by an instance of `Result` into a possibly-empty `Stream` object.

```java
@Test
void testStreamSuccess() {
  // Given
  Result<?, ?> result1 = success("Yes");
  Result<?, ?> result2 = failure("No");
  // When
  Stream<?> stream1 = result1.streamSuccess();
  Stream<?> stream2 = result2.streamSuccess();
  // Then
  assertEquals("Yes", stream1.findFirst().orElse(null));
  assertNull(stream2.findFirst().orElse(null));
}

@Test
void testStreamFailure() {
  // Given
  Result<?, ?> result1 = success("Yes");
  Result<?, ?> result2 = failure("No");
  // When
  Stream<?> stream1 = result1.streamFailure();
  Stream<?> stream2 = result2.streamFailure();
  // Then
  assertNull(stream1.findFirst().orElse(null));
  assertEquals("No", stream2.findFirst().orElse(null));
}
```
