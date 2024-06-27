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

# 🏠 Home

<picture><source srcset=".gitbook/assets/result-logo.dark.svg" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/result-logo.svg" alt=""></picture>

Type-safely encapsulate the output of operations that may succeed or fail instead of throwing exceptions.

<table data-view="cards"><thead><tr><th align="center"></th><th align="center"></th><th align="center"></th></tr></thead><tbody><tr><td align="center"><picture><source srcset=".gitbook/assets/tachometer-alt.dark.svg" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/tachometer-alt.svg" alt=""></picture></td><td align="center"><strong>Boost Performance</strong></td><td align="center">Benefit from faster operations without exception overhead.</td></tr><tr><td align="center"><picture><source srcset=".gitbook/assets/tint.dark.svg" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/tint.svg" alt=""></picture></td><td align="center"><strong>Simple API</strong></td><td align="center">Leverage a familiar interface for a smooth learning curve.</td></tr><tr><td align="center"><picture><source srcset=".gitbook/assets/bolt.dark.svg" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/bolt.svg" alt=""></picture></td><td align="center"><strong>Streamline Error Handling</strong></td><td align="center">Handle errors efficiently and explicitly.</td></tr><tr><td align="center"><picture><source srcset=".gitbook/assets/filter.dark.svg" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/filter.svg" alt=""></picture></td><td align="center"><strong>Functional Style</strong></td><td align="center">Embrace clean, functional programming paradigms.</td></tr><tr><td align="center"><picture><source srcset=".gitbook/assets/glasses.dark.svg" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/glasses.svg" alt=""></picture></td><td align="center"><strong>Enhance Readability</strong></td><td align="center">Increase code clarity and reduce complexity.</td></tr><tr><td align="center"><picture><source srcset=".gitbook/assets/shield-alt.dark.svg" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/shield-alt.svg" alt=""></picture></td><td align="center"><strong>Safe Execution</strong></td><td align="center">Ensure safe and predictable operation outcomes.</td></tr><tr><td align="center"><picture><source srcset=".gitbook/assets/feather-alt.dark.svg" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/feather-alt.svg" alt=""></picture></td><td align="center"><strong>Lightweight</strong></td><td align="center">Keep your project slim using a minimalist library with zero dependencies.</td></tr><tr><td align="center"><picture><source srcset=".gitbook/assets/balance-scale.dark.svg" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/balance-scale.svg" alt=""></picture></td><td align="center"><strong>Open Source</strong></td><td align="center">Enjoy transparent, permissive Apache 2 licensing.</td></tr><tr><td align="center"><picture><source srcset=".gitbook/assets/mug-hot.dark.svg" media="(prefers-color-scheme: dark)"><img src=".gitbook/assets/mug-hot.svg" alt=""></picture></td><td align="center"><strong>Pure Java</strong></td><td align="center">Take advantage of seamless compatibility from JDK8 to the latest versions.</td></tr></tbody></table>

## Results in a Nutshell

In Java, methods that can _fail_ typically do so by _throwing exceptions_. Then, exception-throwing methods are called from inside a `try` block to handle errors in a separate `catch` block.

<div data-full-width="true">

<img src=".gitbook/assets/using-exceptions.png" alt="Using Exceptions">

</div>

This approach is lengthy, and that's not the only problem — it's also very slow.

{% hint style="info" %}
Conventional wisdom says **exceptional logic shouldn't be used for normal program flow**. Results make us deal with _expected error situations_ explicitly to enforce _good practices_ and make our programs [run faster](extra/benchmark.md).
{% endhint %}

Let's now look at how the above code could be refactored if `connect()` returned a `Result` object instead of throwing an exception.

<div data-full-width="true">

<img src=".gitbook/assets/using-results.png" alt="Using Results">

</div>

In the above example, we used only four lines of code to replace the ten that worked for the first one. But we can make it even shorter by chaining methods in functional style. In fact, since we are using `-1` here just to signal that the underlying operation failed, we are better off returning a result instance upstream. This will allow us to compose operations on top of `getServerUptime()` just like we have done with `connect()`.

<div data-full-width="true">

<img src=".gitbook/assets/embracing-results.png" alt="Embracing Results">

</div>

Although this example uses `String` as the failure type, results can use whatever generic type makes the most sense in each situation to represent errors.

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
