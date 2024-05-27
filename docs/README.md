---
title: Home
description: A Java library to handle success and failure without exceptions
layout:
  title:
    visible: false
  description:
    visible: false
  tableOfContents:
    visible: false
  outline:
    visible: false
  pagination:
    visible: false
---

# Home

<div data-full-width="true">

![Result is a Java library to handle success and failure without exceptions](https://raw.githubusercontent.com/LeakyAbstractions/result/main/docs/result.svg)

</div>

The purpose of this library is to type-safely encapsulate the output of operations that may succeed or fail, instead of throwing exceptions.

| ![](.gitbook/assets/tachometer-alt.svg) <br> **Fast**        <br> High performance | ![](.gitbook/assets/tint.svg)          <br> **Simple**      <br> Very easy to use  | ![](.gitbook/assets/bolt.svg)    <br> **Error handling** <br> Functional style |
| :--------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------: | :----------------------------------------------------------------------------: |
| ![](.gitbook/assets/feather-alt.svg)    <br> **Lightweight** <br> No dependencies  | ![](.gitbook/assets/balance-scale.svg) <br> **Open Source** <br> Apache 2 licensed | ![](.gitbook/assets/mug-hot.svg) <br> **Java Library**   <br> JDK 8 and up     |

## Results in a Nutshell

In Java, methods that can _fail_ typically do so by _throwing exceptions_. Then, exception-throwing methods are called from inside a `try` block to handle errors in a separate `catch` block.

<div data-full-width="true">

![Using Exceptions](.gitbook/assets/using-exceptions.png)

</div>

This approach is lengthy, and that's not the only problem — it's also very slow.

{% hint style="info" %}
Conventional wisdom says **exceptional logic shouldn't be used for normal program flow**. Results make us deal with _expected error situations_ explicitly to enforce _good practices_ and make our programs [run faster](extra/benchmark.md).
{% endhint %}

Let's now look at how the above code could be refactored if `connect()` returned a `Result` object instead of throwing an exception.

<div data-full-width="true">

![Using Results](.gitbook/assets/using-results.png)

</div>

In the above example, we used only four lines of code to replace the ten that worked for the first one. But we can make it even shorter by chaining methods in functional style. In fact, since we are using `-1` here just to signal that the underlying operation failed, we'd be better off returning a result instance upstream. This allows us to easily compose operations on top of `getServerUptime()` just like we did with `connect()`.

<div data-full-width="true">

![Embracing Results](.gitbook/assets/embracing-results.png)

</div>

Although this example uses `String` as the failure type, results can use whatever generic type makes the most sense in each situation to represent errors.

{% hint style="success" %}
If you like `Optional` but feel that it sometimes falls too short, you will feel right at home.
{% endhint %}

## Read the Docs

{% content-ref url="docs/start/" %}
[start](docs/start/)
{% endcontent-ref %}

{% content-ref url="docs/basic/" %}
[basic](docs/basic/)
{% endcontent-ref %}

{% content-ref url="docs/advanced/" %}
[advanced](docs/advanced/)
{% endcontent-ref %}
