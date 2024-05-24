---
title: Getting Started
description: How to get started with results
---

# 🌱 Getting Started

The best way to think of results is as a super-powered version of Java optionals.

The main difference is that an `Optional` instance can only express the _presence_ or _absence_ of a value, whereas a `Result` object may contain either a _success value_ or a _failure value_ that can be used to reason about what went wrong.

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

As you can see, `Result` objects have methods equivalent to those of `Optional`, plus a few more for handling failure cases.

<figure><img src="../../.gitbook/assets/getting-started.png" alt=""><figcaption></figcaption></figure>
