---
title: Page title
description: A Java library to handle success and failure without exceptions
---

# 🏠 Introduction

<figure><img src="https://raw.githubusercontent.com/LeakyAbstractions/result/main/docs/result-magic-ball.png" alt="Result is a Java library to handle success and failure without exceptions."><figcaption><p><em><code>Result</code> objects have all the answers you need</em></p></figcaption></figure>



<details>

<summary>Result Library in a Nutshell</summary>

The main difference is that an `Optional` instance can only express the _presence_ or _absence_ of a value, whereas a `Result` object may contain either a _success value_ or a _failure value_ that can be used to reason about what went wrong.

![](.gitbook/assets/cover\_small.jpg)

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td></td><td></td><td><mark style="color:purple;">one</mark></td><td><a href=".gitbook/assets/cover.jpg">cover.jpg</a></td></tr><tr><td></td><td></td><td>two</td><td><a href=".gitbook/assets/basic-usage.png">basic-usage.png</a></td></tr><tr><td></td><td></td><td>three</td><td><a href=".gitbook/assets/getting-started.png">getting-started.png</a></td></tr></tbody></table>

As you can see, `Result` objects have methods equivalent to those of `Optional`, plus a few more for handling failure cases.

</details>



|   <img src=".gitbook/assets/image.png" alt="" data-size="original">   |                  ![](<.gitbook/assets/image (1).png>)                 |            ![](<.gitbook/assets/image (2).png>)            |
| :-------------------------------------------------------------------: | :-------------------------------------------------------------------: | :--------------------------------------------------------: |
|         <p><strong>Fast</strong><br>Faster than exceptions</p>        |        <p><strong>Simple</strong><br>No frills, easy to use</p>       | <p><strong>Error Handling</strong><br>Functional style</p> |
|                                                                       |                                                                       |                                                            |
| <img src=".gitbook/assets/image (3).png" alt="" data-size="original"> | <img src=".gitbook/assets/image (4).png" alt="" data-size="original"> |            ![](<.gitbook/assets/image (5).png>)            |
|      <p><strong>Lightweight</strong></p><p>Zero dependencies</p>      |   <p><strong>Open Source</strong></p><p>Licensed under Apache 2</p>   |   <p><strong>Java Library</strong></p><p>JDK 8 and up</p>  |

The purpose of this library is to type-safely encapsulate the output of operations that may _succeed_ or _fail_, instead of throwing exceptions.

If you like `Optional` but feel that it sometimes falls too short, you will feel right at home.

### Result Library in a Nutshell

Before `Result`, we would wrap the invocation of an exception-throwing method `foo` inside a `try` block so that errors can be handled inside a `catch` block.

```java
public int getFooLength() {
  int length;
  try {
    String result = foo();
    this.ok(result);
    length = result.length();
  } catch(SomeException problem) {
    this.error(problem);
    length = -1;
  }
  return length;
}
```

This approach is lengthy, and that's not the only problem — it's also [very slow](https://dev.leakyabstractions.com/result-benchmark/). Conventional wisdom says that exceptional logic shouldn't be used for normal program flow. `Result` makes us deal with expected, non-exceptional error situations explicitly as a way to enforce good programming practices and make our programs run faster.

Let's now look at how the above code could be refactored if `foo` returned a result object instead of throwing an exception:

```java
public int getFooLength() {
  Result<String, SomeFailure> result = foo();
  result.ifSuccessOrElse(this::ok, this::error);
  Result<Integer, SomeFailure> resultLength = result.mapSuccess(String::length);
  return resultLength.orElse(-1);
}
```

In the above example, we use only four lines of code to replace the ten that worked in the first example. But we can make it even shorter by chaining methods in typical functional programming style:

```java
public int getFooLength() {
  return foo().ifSuccessOrElse(this::ok, this::error).mapSuccess(String::length)
    .orElse(-1);
}
```

In fact, since we are using `-1` here just to signal that the underlying operation failed, we'd be better off returning a `Result` object upstream:

```java
public Result<Integer, SomeFailure> getFooLength() {
  return foo().ifSuccessOrElse(this::ok, this::error).mapSuccess(String::length);
}
```

This allows others to easily compose operations on top of ours, just like we did with `foo`.
