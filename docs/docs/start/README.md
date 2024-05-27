---
title: Getting Started
description: Get up and running with results in no time
---

# 🌱 Getting Started

The best way to think of results is as a super-powered version of Java optionals.

The main difference is that an `Optional` instance can only express the _presence_ or _absence_ of a value, whereas a `Result` object may contain either a _success value_ or a _failure value_ that can be used to reason about what went wrong.

{% tabs %}
{% tab title="Optional vs Result" %}
{% hint style="success" %}
If you like `Optional` but feel that it sometimes falls too short, you will feel right at home.
{% endhint %}
{% endtab %}

{% tab title="Methods" %}
As you can see, `Result` objects have methods equivalent to those of `Optional`, plus a few more to handle failure outcomes effectively.

| Optional          | Result            |
| ----------------- | ----------------- |
| `isPresent`       | `isSuccess`       |
| `isEmpty`         | `isFailure`       |
| `get`             | `getSuccess`      |
|                   | `getFailure`      |
| `orElse`          | `orElse`          |
| `orElseGet`       | `orElseMap`       |
| `orElseThrow`     |                   |
| `stream`          | `streamSuccess`   |
|                   | `streamFailure`   |
| `ifPresent`       | `ifSuccess`       |
|                   | `ifFailure`       |
| `ifPresentOrElse` | `ifSuccessOrElse` |
| `filter`          | `filter`          |
|                   | `recover`         |
| `map`             | `mapSuccess`      |
|                   | `mapFailure`      |
|                   | `map`             |
| `flatMap`         | `flatMapSuccess`  |
| `or`              | `flatMapFailure`  |
|                   | `flatMap`         |
{% endtab %}
{% endtabs %}

<div align="center" data-full-width="true">

<figure><img src="../../.gitbook/assets/getting-started.png" alt=""><figcaption><p>No need to return <code>null</code> or throw an exception: just return a <em>failed</em> result.</p></figcaption></figure>

</div>

