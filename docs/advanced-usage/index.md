---
title: Advanced Usage
description: Advanced Usage
cover: >-
  https://images.unsplash.com/photo-1555597673-b21d5c935865?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHwyfHxrYXJhdGV8ZW58MHx8fHwxNjg0OTMzODM4fDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Advanced Usage

While retrieving success/failure values out of `Result` objects can be convenient sometimes, what's in fact more idiomatic is to manipulate the value inside a result without actually unwrapping it. Most of the times, we will apply transformations to a `Result` instance, obtaining a possibly different result object in return. This allows us to compose behavior in a [monadic way](https://en.wikipedia.org/wiki/Monad\_\(functional\_programming\)).

<figure><img src="../.gitbook/assets/advanced-usage (1).png" alt=""><figcaption><p>Results can be filtered and transformed just like Java streams.</p></figcaption></figure>
