---
description: Asserting Result Objects
---

# Fluent Assertions

You can use fluent assertions (based on [AssertJ](https://assertj.github.io/)) for Result objects in your unit tests.

## Adding Lazy Results to Your Build

Artifact coordinates:

* Group ID: `com.leakyabstractions`
* Artifact ID: `result-assertj`
* Version: `1.0.0.0`

## How to Use Fluent Assertions

Static method `ResultAssertions::assertThat` allows us to use `Result` assertions in your tests:

```java
import static com.leakyabstractions.result.assertj.ResultAssertions.assertThat;

@Test
public void should_pass() {
    // Given
    final int number = someMethodReturningInt();
    // When
    final Result<String, Integer> result = someMethodReturningResult(number);
    // Then
    assertThat(number).isZero();
    assertThat(result).hasSuccess("OK");
}
```

If, for some reason, we cannot statically import this method, we can use `ResultAssert::assertThatResult` instead:

```java
import static com.leakyabstractions.result.assertj.ResultAssert.assertThatResult;
import static org.assertj.core.api.Assertions.assertThat;

@Test
public void should_pass_too() {
    // Given
    final int number = anotherMethodReturningInt();
    // When
    final Result<String, Integer> result = anotherMethodReturningResult(number);
    // Then
    assertThat(number).isOne();
    assertThatResult(result).hasFailure(1);
}
```
