---
title: Getting Started
description: How to start using Result in your own projects
cover: >-
  https://images.unsplash.com/photo-1562516155-e0c1ee44059b?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw0fHxzdGFydHxlbnwwfHx8fDE2ODUwMTQxNDd8MA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Getting Started

The purpose of this library is to provide a type-safe encapsulation of operation results that may have succeeded or failed, instead of throwing exceptions.

If you like `Optional` but feel that it sometimes falls too short, you'll love `Result`.

The best way to think of `Result` is as a super-powered version of `Optional`. The only difference is that whereas `Optional` may contain a successful value or express the absence of a value, `Result` contains either a successful value or a failure value that explains what went wrong.

<figure><img src="../.gitbook/assets/getting-started.png" alt=""><figcaption><p>Don't return <code>null</code> or throw an exception: just return a <em>failed</em> result.</p></figcaption></figure>
