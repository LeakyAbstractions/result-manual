---
description: Deferring expensive calculation of Results
---

# Lazy Results

Lazy results allow us to encapsulate expensive operations that can be entirely omitted (as an optimization) if there's no actual need to examine the result.

## Adding Lazy Results to Your Build

Artifact coordinates:

* Group ID: `com.leakyabstractions`
* Artifact ID: `result-lazy`
* Version: `1.0.0.0`

## Lazy Results Based on Suppliers

When we have an expression that supplies a result object, we can use [`LazyResults::ofSupplier`][OF_SUPPLIER] to encapsulate it as a lazy result.

```java
@Test
void create_lazy_result_based_supplier() {
  // When
  final Supplier<Result<Integer, String>> supplier = () -> Results.success(200);
  final Result<Integer, ?> result = LazyResults.ofSupplier(supplier);
  // Then
  assertTrue(result::hasSuccess);
  assertFalse(result::hasFailure);
}
```

{% hint style="danger" %}
Note that it is illegal for the `supplier` to return `null`.
{% endhint %}

```java
...
```

## Lazy Results Based on Callables

If we have a task that may either return a success value or throw an exception, we can encapsulate it as a lazy result using [`LazyResults::ofCallable`][OF_CALLABLE].

```java
String task() {
  return "OK";
}

@Test
void create_lazy_result_based_on_callable() {
  // When
  final Result&#x3C;String, Exception> result = LazyResults.ofCallable(this::task);
  // Then
  assertTrue(result::hasSuccess);
}
```

## Handling Lazy Results

Lazy results can be manipulated just like any other result. The only difference is that they will try to defer the invocation of the given supplier or callable as long as possible. For example, when we actually try to determine if the operation succeeded or failed.

```java
Result<String, Void> expensiveCalculation(AtomicLong timesExecuted) {
  timesExecuted.getAndIncrement();
  return Results.success("HELLO");
}

@Test
void should_execute_expensive_action() {
  final AtomicLong timesExecuted = new AtomicLong();
  // Given
  final Result<String, Void> lazy = Results
      .lazy(() -> expensiveCalculation(timesExecuted));
  // When
  final Result<Integer, Void> transformed = lazy.mapSuccess(String::length);
  final boolean success = transformed.hasSuccess();
  // Then
  assertThat(success).isTrue();
  assertThat(timesExecuted).hasValue(1);
}
```

However, if the lazy result is only transformed but its wrapped value is never retrieved, the expensive computation will never be executed.

```java
@Test
void should_not_execute_expensive_action() {
  final AtomicLong timesExecuted = new AtomicLong();
  // Given
  final Result<String, Void> lazy = Results
      .lazy(() -> expensiveCalculation(timesExecuted));
  // When
  final Result<Integer, Void> transformed = lazy.mapSuccess(String::length);
  // Then
  assertThat(transformed).isNotNull();
  assertThat(timesExecuted).hasValue(0);
}
```

Lazy results allow us to optimize complex flows that depend on several operations, where some of them only need to be executed in specific circumstances.

{% hint style="info" %}
Transforming a lazy result holds off the expensive computation.

* `filter`
* `recover`
* `map`
* `mapSuccess`
* `mapFailure`
* `flatMap`
* `flatMapSuccess`
* `flatMapFailure`
{% endhint %}

Once a lazy result materializes, it will behave just like a regular result.

{% hint style="info" %}
Lazy results cache the materialized value, so expensive operations will be executed just once at most.
{% endhint %}

## Lazy Side Effects

The `if...` family of methods is somewhat special because we may want to defer the execution of side effects too.

```
...
```

{% hint style="info" %}
Using any of these methods on a lazy result will execute the expensive operation:

* `getSuccess`
* `getFailure`
* `hasSuccess`
* `hasFailure`
* `orElse`
* `orElseMap`
* `streamSuccess`
* `streamFailure`
{% endhint %}


[OF_SUPPLIER]: https://dev.leakyabstractions.com/result-lazy/javadoc/1.0.0.0/com/leakyabstractions/result/lazy/LazyResults.html#ofSupplier-java.util.function.Supplier-
[OF_CALLABLE]: https://dev.leakyabstractions.com/result-lazy/javadoc/1.0.0.0/com/leakyabstractions/result/lazy/LazyResults.html#ofCallable-java.util.concurrent.Callable-
