---
layout: post
title: Few choices of Python linter and formatter with VSCode
date: 2023-08-01
---

## Formatter

[black](https://github.com/psf/black) is the uncompromising Python code formatter. 

In Visual Studio Code, [Black Formatter](https://marketplace.visualstudio.com/items?itemName=ms-python.black-formatter) extension can be installed.

A basic usage in VSCode could be as follow. 

```json
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter",
    "editor.formatOnSave": true
  }
```

## Linter

### MyPy

[Mypy](https://github.com/python/mypy) is a static type checker for Python. Type checkers help ensure that you're using variables and functions in your code correctly. With `mypy`, add type hints (PEP 484) to your Python programs, and mypy will warn you when you use those types incorrectly.

In Visual Studio Code, [Mypy Type Checker](https://marketplace.visualstudio.com/items?itemName=ms-python.mypy-type-checker) extension can be installed.

### Ruff

[Ruff](https://github.com/astral-sh/ruff) is an extremely fast Python linter, written in Rust. Ruff can be used to replace `Flake8`` (plus dozens of plugins), `isort`, `pydocstyle`, `yesqa`, `eradicate`, `pyupgrade`, and `autoflake`, all while executing tens or hundreds of times faster than any individual tool.

In Visual Studio Code, [Ruff](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff)  extension can be installed.
