---
title: Conditional Actions
description: Handling success and failure
cover: >-
  https://images.unsplash.com/photo-1533073526757-2c8ca1df9f1c?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwxfHxkaXJlY3Rpb25zfGVufDB8fHx8MTY4NDkzNDA3MHww&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Conditional Actions

The `if...` family of methods enables us to run some code on the wrapped success/failure value.

## Handling Exceptions

Before `Result`, we would wrap exception-throwing `foobar` method invocation inside a `try` block so that errors can be handled inside a `catch` block:

```java
try {
    final String result = foobar();
    this.commit(result);
} catch(SomeException problem) {
    this.rollback(problem);
}
```

## Handling Results

Let's now look at how the above code could be refactored if method `foobar` returned a `Result` object instead of throwing an exception:

```java
final Result<String, SomeFailure> result = foobar();
result.ifSuccessOrElse(this::commit, this::rollback);
```

The first action passed to [`ifSuccessOrElse()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#ifSuccessOrElse-java.util.function.Consumer,java.util.function.Consumer-) will be performed if `foobar` succeeded; otherwise, the second one will.

The above example is not only shorter but also faster. We can make it even shorter by chaining methods in typical functional programming style:

```java
foobar().ifSuccessOrElse(this::commit, this::rollback);
```

There are other methods [`ifSuccess()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#ifSuccess-java.util.function.Consumer-) and [`ifFailure()`](https://dev.leakyabstractions.com/result/javadoc/1.0.0.0/com/leakyabstractions/result/Result.html#ifFailure-java.util.function.Consumer-) to handle either one of the success/ failure cases only:

```java
foobar()
    .ifSuccess(this::commit)    // commits only if the result is success
    .ifFailure(this::rollback); // rolls back only if the result is failure
```
