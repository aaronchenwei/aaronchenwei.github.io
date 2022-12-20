---
layout: post
title: Introduction to Corepack
date: 2022-12-20
---

Corepack is an experimental tool that helps you to manage your package managers. It ships with Node.js core so you probably have it installed already.

## Why do I need Corepack?

Modern Web Development projects have tons of dependencies. Making sure those dependencies stay consistent over time and across development environments can be a true nightmare. Dependency handling is the job of package managers like `npm`, `yarn`, or `pnpm`. Each has a similar feature set with different trade offs. They all do the following:

- Add/remove/update/dedupe dependencies
- Run package.json scripts
- Manage monorepos (via workspaces)
- Reproducible installs (via lockfile)

While these features seem similar, they have different implementations. Specifically, "reproducible installs" is only guaranteed when you use the same package manager that knows how to read a given lockfile, such as:

- package-lock.json
- yarn.lock
- pnpm-lock.yaml

Not to mention, lockfiles might change implementations between different versions of a package manager. For example, v2 lockfiles introduced in npm 7 are not compatible with earlier versions of npm.

But what manages the package manager? How do you ensure that your collaborators, development environments, continuous integration environments, and even individual projects within a monorepo don't throw surprises at you after using an old version of your package manager? Or even the wrong package manager entirely?

Before Corepack, it was something manual like:

```sh
$ npm install --global pnpm@x.y.z
```

Using npm to install pnpm, is the equivalent of using Internet Explorer to install Chrome.

## What is Corepack?

`Corepack` is an experimental tool that helps you to manage your package managers. Although you may have never heard of it, Corepack ships with Node.js 14.19.0 and newer. So you likely have it installed already!

Corepack transparently installs the correct package manager version based on the closest `package.json` file. For example, you can configure pnpm 6 with one project and pnpm 7 with another project, no need to run additional commands.

## How to use Corepack?

Corepack comes with Node >=14.19 and >=16.9, but it is not enabled by default.

You'll need to **run `corepack enable` to turn it on** in all of your development and continuous integration environments.

All you have to do after that is set the `packageManager` field in all of your project's `package.json` files. You'll need to set it to an *exact version* of `pnpm` or `yarn`. For example:

```json
{
	"name": "your-amazing-project",
	"version": "1.2.3",
	"packageManager": "pnpm@7.18.2"
}
```
And that's it!

You can also use corepack to set the fallback (global) package manager version, to use when you aren't inside of a project that has the `packageManager` field set. To do that, run `corepack prepare pnpm@7.18.2 --activate` (substituting in whatever version of pnpm/yarn you want to use).

