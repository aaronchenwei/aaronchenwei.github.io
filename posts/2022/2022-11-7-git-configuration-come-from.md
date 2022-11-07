---
layout: post
title: Where do the settings in my Git configuration come from?
date: 2022-11-07
---

Git checks following 4 places for a configuration files:

- Your machine's system `.gitconfig` file.
- Your user .gitconfig file located at `~/.gitconfig`. If you're using MSysGit on Windows, you'll probably find your user `~/.gitconfig` file where ever `%homepath%` points to if you use `echo %homepath%` from a Windows command prompt.
- A second user-specific configuration file located at `$XDG_CONFIG_HOME/git/config` or `$HOME/.config/git/config`.
- The local repository's configuration file `.git/config`.

The settings cascade in the following order, with each file adding or overriding settings defined in the file above it.

1. System configuration.
1. User configuration.
1. Repository-specific configuration.

Teach `git config` the `--show-origin` option to print the source configuration file for every printed value.

For example:

```sh
$ git config --list --show-origin
file:$HOME/.gitconfig    user.name=aaronchenwei
file:$HOME/.gitconfig    user.email=aaronchenwei@hotmail.com
file:$HOME/.gitconfig    core.autocrlf=input
file:$HOME/.gitconfig    core.safecrlf=false
file:$HOME/.gitconfig    pull.rebase=true
file:$HOME/.gitconfig    push.default=current
file:$HOME/.gitconfig    init.defaultbranch=main
file:.git/config        core.repositoryformatversion=0
file:.git/config        core.filemode=true
file:.git/config        core.bare=false
file:.git/config        core.logallrefupdates=true
file:.git/config        remote.origin.url=https://github.com/aaronchenwei/aaronchenwei.github.io.git
file:.git/config        remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
file:.git/config        branch.main.remote=origin
file:.git/config        branch.main.merge=refs/heads/main
```

Since **Git 2.26**, you can add the `--show-scope` option:

```sh
$ git config -l --show-origin --show-scope
```
