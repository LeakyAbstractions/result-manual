---
title: Advanced Usage
description: Advanced Usage
---

# Advanced Usage

While retrieving success/failure values out of `Result` objects can be convenient sometimes, what's in fact more
idiomatic is to manipulate the value inside a result without actually unwrapping it. Most of the times, we will apply
transformations to a `Result` instance, obtaining a possibly different result object in return. This allows us to
compose behavior in a [monadic way][MONAD].

[MONAD]: https://en.wikipedia.org/wiki/Monad_(functional_programming)
