---
title: Getting Started
description: Get up and running with results in no time
---

# 🌱 Getting Started

{% hint style="success" %}
&#x20;The best way to think of results is as a super-powered version of Java's optionals.
{% endhint %}

`Result` builds upon the familiar concept of `Optional`, enhancing it with the ability to represent both success and failure states. By leveraging results, you can unleash a powerful tool for error handling that goes beyond the capabilities of traditional optionals, leading to more robust and maintainable Java code.

{% tabs %}
{% tab title="Why Results Over Optionals?" %}
`Optional` class is useful for representing values that might be present or absent, eliminating the need for null checks. However, optionals fall short when it comes to error handling because they do not convey why a value is lacking. `Result` addresses this limitation by encapsulating both successful values and failure reasons, offering a more expressive way to reason about what went wrong.
{% endtab %}

{% tab title="Methods" %}
Results provide the same methods as optionals, plus additional ones to handle failure states effectively.

| Optional            | Result              |
| ------------------- | ------------------- |
| `isPresent()`       | `isSuccess()`       |
| `isEmpty()`         | `isFailure()`       |
| `get()`             | `getSuccess()`      |
|                     | `getFailure()`      |
| `orElse()`          | `orElse()`          |
| `orElseGet()`       | `orElseMap()`       |
| `orElseThrow()`     |                     |
| `stream()`          | `streamSuccess()`   |
|                     | `streamFailure()`   |
| `ifPresent()`       | `ifSuccess()`       |
|                     | `ifFailure()`       |
| `ifPresentOrElse()` | `ifSuccessOrElse()` |
| `filter()`          | `filter()`          |
|                     | `recover()`         |
| `map()`             | `mapSuccess()`      |
|                     | `mapFailure()`      |
|                     | `map()`             |
| `flatMap()`         | `flatMapSuccess()`  |
| `or()`              | `flatMapFailure()`  |
|                     | `flatMap()`         |
{% endtab %}
{% endtabs %}

<div align="center" data-full-width="true">

<figure><img src="../../.gitbook/assets/getting-started.png" alt=""><figcaption><p>No need to return <code>null</code> or throw an exception: just return a failed result.</p></figcaption></figure>

</div>

