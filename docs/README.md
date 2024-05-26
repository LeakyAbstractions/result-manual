---
title: Page title
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

# 🏠 Home

<div data-full-width="true">

<figure><img src="https://raw.githubusercontent.com/LeakyAbstractions/result/main/docs/result.svg" alt="Result is a Java library to handle success and failure without exceptions." width="563"><figcaption></figcaption></figure>

</div>

The purpose of this library is to type-safely encapsulate the output of operations that may succeed or fail, instead of throwing exceptions.

|      <p><img src=".gitbook/assets/tachometer-alt.svg" alt=""><br><strong>Fast</strong><br>High performance</p>     |      <p><img src=".gitbook/assets/tint.svg" alt=""><br><strong>Simple</strong><br>No frills, easy to use</p>     | <p><img src=".gitbook/assets/bolt.svg" alt=""><br><strong>Error handling</strong><br>Functional style</p> |
| :----------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------: |
| <p><img src=".gitbook/assets/feather-alt.svg" alt=""><br><strong>Lightweight</strong><br>No other dependencies</p> | <p><img src=".gitbook/assets/balance-scale.svg" alt=""><br><strong>Open Source</strong><br>Apache 2 Licensed</p> |   <p><img src=".gitbook/assets/mug-hot.svg" alt=""><br><strong>Java Library</strong><br>JDK 8 and up</p>  |

### Results in a Nutshell

In Java, methods that can _fail_ typically do so by _throwing exceptions_. Then, exception-throwing methods are called from inside a `try` block to handle errors in a separate `catch` block.

<div data-full-width="true">

![Using Exceptions](.gitbook/assets/using-exceptions.png)

</div>

This approach is lengthy, and that's not the only problem — it's also [very slow](https://dev.leakyabstractions.com/result-benchmark/).

{% hint style="info" %}

Conventional wisdom says **exceptional logic shouldn't be used for normal program flow**. Results make us deal with _expected error situations_ explicitly to enforce _good practices_ and make our programs run _faster_.

[![](https://img.shields.io/endpoint?url=https://dev.leakyabstractions.com/result-benchmark/badge.json&style=flat)](https://github.com/LeakyAbstractions/result-benchmark)

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

### Read the Docs

{% content-ref url="docs/start/" %}
[start](docs/start/)
{% endcontent-ref %}

{% content-ref url="docs/basic/" %}
[basic](docs/basic/)
{% endcontent-ref %}

{% content-ref url="docs/advanced/" %}
[advanced](docs/advanced/)
{% endcontent-ref %}
