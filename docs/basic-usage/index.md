---
title: Basic Usage
description: How to solve simple use-case scenarios
cover: >-
  https://images.unsplash.com/photo-1608751819407-6ec94205fd7b?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw2fHxlYXN5fGVufDB8fHx8MTY4NTAxOTQzOXww&ixlib=rb-4.0.3&q=85
coverY: 0
---

# Basic Usage

Very often, when we have a `Result` instance we want to execute a specific action on the underlying success value. On the other hand, if it’s a failed result we may have some recovery strategy or alternative actions to take.

Other simple scenarios include checking if the `Result` object is successful or failed, or unwrapping the success/failure values.

<figure><img src="../.gitbook/assets/basic-usage.png" alt=""><figcaption><p>You don't need <code>if</code> or early <code>return</code> statements when you can handle success and failure without any hassle.</p></figcaption></figure>

![](../../assets/images/basic-usage.png)
