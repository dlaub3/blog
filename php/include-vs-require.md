---
title: "Include VS Require"
date: 2018-07-22
Tags: ["PHP"]
description: "What are the differences between include and require in PHP"
---


The require() function performs the same as include() except for error handling.

Include() will generate a warning and continue to execute while require() will generate a fatal error and stop.

The require() and require_once() functions are the same. The require_once() function will check to see if it has already been included, require() won't check.