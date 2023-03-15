---
layout: post
title: What's the difference between --add-exports and --add-opens in Java 9?
date: 2023-03-15
---

## Difference

- With `--add-exports` the package is exported, meaning all public types and members therein are accessible at compile and run time.
- With `--add-opens` the package is opened, meaning all types and members (not only public ones!) therein are accessible at run time.

So the main difference at run time is that `--add-opens` allows "deep reflection", meaning access of non-public members. You can typically identify this kind of access by the reflecting code making calls to `setAccessible(true)`.

It is worth adding that at runtime `-add-opens` implicates `-add-exports`.

For more, please review details through [JEP 261](https://openjdk.org/jeps/261).
