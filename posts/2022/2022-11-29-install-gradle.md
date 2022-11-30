---
layout: post
title: Manully Gradle Installation on Linux
date: 2022-11-29
tags: gradle installation
---

## Installing manually

### Step 1. Download the latest Gradle distribution

The current Gradle release is version 7.6, released on 25 Nov 2022. Please refer the latest version from [here](https://gradle.org/install/).

```sh
$ wget https://services.gradle.org/distributions/gradle-7.6-bin.zip
```

### Step 2. Unpack the distribution

Unzip the distribution zip file in the directory of your choosing, e.g.:

```sh
$ sudo mkdir /opt/gradle
$ sudo unzip -d /opt/gradle gradle-7.6-bin.zip
$ ls /opt/gradle/gradle-7.6
```

### Step 3. Create symoblic link

```sh
$ cd /opt/gradle/
$ sudo ln -s gradle-7.6 current
```

### Step 4. Configure your system environment

```sh
$ export PATH=$PATH:/opt/gradle/current/bin
```

In case, if using `fish` as default shell

```sh
$ fish_add_path /opt/gradle/current/bin
```

### Step 5. Verify your installation

```sh
$ gradle -v

Welcome to Gradle 7.6!

Here are the highlights of this release:
 - Added support for Java 19.
 - Introduced `--rerun` flag for individual task rerun.
 - Improved dependency block for test suites to be strongly typed.
 - Added a pluggable system for Java toolchains provisioning.

For more details see https://docs.gradle.org/7.6/release-notes.html


------------------------------------------------------------
Gradle 7.6
------------------------------------------------------------

Build time:   2022-11-25 13:35:10 UTC
Revision:     daece9dbc5b79370cc8e4fd6fe4b2cd400e150a8
...
```

## Upgrade with the Gradle Wrapper

```sh
$ ./gradlew wrapper --gradle-version=7.6 --distribution-type=bin
```

## Upgrade Gradle

Just repeat Step 1 and Step 3. For step 3, re-create symbolic link.
