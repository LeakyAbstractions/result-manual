---
description: Result-Jackson provides a Jackson datatype module for Result objects
---

# Problem Overview

First, let's take a look at what happens when we try to serialize and deserialize ApiResponse objects with Jackson.

To use Jackson, let's make sure we're using the latest version:

* groupId: `com.fasterxml.jackson.core`
* artifactId: `jackson-core`
* version: `2.13.3`
