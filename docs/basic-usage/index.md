---
title: Basic Usage
description: How to solve simple use-case scenarios
---


# Basic Usage

Very often, when we have a `Result` instance we want to execute a specific action on the underlying success value. On
the other hand, if it’s a failed result we may have some recovery strategy or alternative actions to take.

Other simple use-case scenarios include checking if the `Result` object is successful or failed, or unwrapping the
success/failure values.
