# pnpm Introduction

## What is a Package Manager?

A package manager is a piece of software that handles the installation, upgrading, and removal of computer software packages.

A package manager stores packages in a central location on the hard disk or network drive. It allows multiple users to share a single copy of the package.

Package managers, like npm install and yarn add, are often CLI-based. Usually, JavaScript applications have many dependencies, and those dependencies are managed by a package manager.

`Node` uses `NPM` by default. However, `NPM` does not have some advanced features that are ideal for more advanced applications, or it is slow when installing packages or solving package dependencies.

`Yarn` and `PNPM`, which are community-made package managers, came into existence to solve the above problem. In the past few years, the yarn has become slower, but today it's probably the most popular option.

## What is pnpm?

`pnpm` is a drop-in replacement for `npm`. It is built on top of `npm` and is much faster and more efficient than its predecessor. It is highly disk efficient and solves inherent issues in npm. In this article, we will discuss pnpm in detail. We will explain how it works and will go through why pnpm is a perfect replacement for npm or yarn.

## Why pnpm?

### Motivation

#### Saving disk space and boosting installation speed

![image](https://user-images.githubusercontent.com/9360415/208339609-78a30fdb-78b6-4609-9c32-63e50130c6b1.png)

When using npm, if you have 100 projects using a dependency, you will have 100 copies of that dependency saved on disk. With pnpm, the dependency will be stored in a content-addressable store, so:

1. If you depend on different versions of the dependency, only the files that differ are added to the store. For instance, if it has 100 files, and a new version has a change in only one of those files, pnpm update will only add 1 new file to the store, instead of cloning the entire dependency just for the singular change.
2. All the files are saved in a single place on the disk. When packages are installed, their files are hard-linked from that single place, consuming no additional disk space. This allows you to share dependencies of the same version across projects.

As a result, you save a lot of space on your disk proportional to the number of projects and dependencies, and you have a lot faster installations!

#### Creating a non-flat node_modules directory

![image](https://user-images.githubusercontent.com/9360415/208339621-9bfa41f4-bb6b-4655-ac31-49ef7031e259.png)

When installing dependencies with npm or Yarn Classic, all packages are hoisted to the root of the modules directory. As a result, source code has access to dependencies that are not added as dependencies to the project.

By default, pnpm uses symlinks to add only the direct dependencies of the project into the root of the modules directory.

## pnpm CLI

Pnpm has a cool CLI. Here are some of the basic commands:

- `pnpm init`: Create a package.json file
- `pnpm install`: Download all the packages listed as dependencies in package.json
- `pnpm add [module_name]@[version]`: Download a particular version of a package and add to the list of dependencies in package.json
- `pnpm remove [module_name]`: Uninstall and remove a package from the list of dependencies in package.json
- `pnpm list`: Print a tree of locally installed modules

## Migrating from NPM/Yarn to PNPM

If your projects use npm or yarn, then migrating to pnpm will not be very difficult. Here is a comparison of commands between npm, yarn, and pnpm.

| npm command            | Yarn command              | pnpm equivalent           |
| ---------------------- | ------------------------- | ------------------------- |
| npm install            | yarn                      | pnpm install              |
| npm install [pkg]      | yarn add [pkg]            | pnpm add [pkg]            |
| npm uninstall [pkg]    | yarn remove [pkg]         | pnpm remove [pkg]         |
| npm update             | yarn upgrade              | pnpm update               |
| npm list               | yarn list                 | pnpm list                 |
| npm run [scriptName]   | yarn [scriptName]         | pnpm [scriptName]         |
| npx [command]          | yarn dlx [command]        | pnpm dlx [command]        |
| npm exec               | yarn exec [commandName]   | pnpm exec [commandName]   |
| npm init [initializer] | yarn create [initializer] | pnpm create [initializer] |

## Feature Comparison

| Feature                     | pnpm                               | Yarn                 | npm                      |
| --------------------------- | ---------------------------------- | -------------------- | ------------------------ |
| Workspace support           | ✔️                                 | ✔️                   | ✔️                       |
| Isolated `node_modules`     | ✔️ - The default                   | ✔️                   | ❌                       |
| Hoisted `node_modules`      | ✔️                                 | ✔️                   | ✔️ - The default         |
| Autoinstalling peers        | ✔️ - Via [auto-install-peers=true] | ❌                   | ✔️                       |
| Plug'n'Play                 | ✔️                                 | ✔️ - The default     | ❌                       |
| Zero-Installs               | ❌                                 | ✔️                   | ❌                       |
| Patching dependencies       | ✔️                                 | ✔️                   | ❌                       |
| Managing Node.js versions   | ✔️                                 | ❌                   | ❌                       |
| Has a lockfile              | ✔️ - `pnpm-lock.yaml`              | ✔️ - `yarn.lock`     | ✔️ - `package-lock.json` |
| Overrides support           | ✔️                                 | ✔️ - Via resolutions | ✔️                       |
| Content-addressable storage | ✔️                                 | ❌                   | ❌                       |
| Dynamic package execution   | ✔️ - Via `pnpm dlx`                | ✔️ - Via `yarn dlx`  | ✔️ - Via `npx`           |
| Side-effects cache          | ✔️                                 | ❌                   | ❌                       |
| Listing licenses            | ✔️ - Via `pnpm licenses list`      | ✔️ - Via a plugin    | ❌                       |
