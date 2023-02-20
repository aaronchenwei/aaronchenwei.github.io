---
layout: post
title: How to add multiple directories in dependabot.yml config file? 
date: 2023-02-20
---

If you want github Dependabot to check multiple directories, you could configure multiple `updates` in the file `dependabot.yml`. Here is an example.

```yaml
# To get started with Dependabot version updates, you'll need to specify which
# package ecosystems to update and where the package manifests are located.
# Please see the documentation for all configuration options:
# https://docs.github.com/github/administering-a-repository/configuration-options-for-dependency-updates

version: 2
updates:
  - package-ecosystem: "maven" # See documentation for possible values
    directory: "/module1" # Location of package manifests
    schedule:
      interval: "weekly"
  - package-ecosystem: "maven" # See documentation for possible values
    directory: "/module2" # Location of package manifests
    schedule:
      interval: "weekly"
```
