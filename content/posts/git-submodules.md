+++
title = "Git Submodules"
description = "Git submodule cookbook"
categories = []
tags = ['Git']
date = 2020-09-06T17:28:34+01:00
draft = false
+++

Git submodules are not the most intuitive.

This is a quick cookbook for common use cases.

## Add Submodule

Add a submodule to a project:

```bash
git submodule add https://github.com/thomasmarkrea/test-submodule.git
```

## Switch Version (Tag/Commit)

Switch to a certain version of the submodule code:

```bash
cd test-submodule/
git checkout v1

cd ..
git add .
git commit -m 'Switch submodule to v1'
```

If the version required was released after the submodule was added, a `fetch` is needed:

```bash
cd test-submodule/
git fetch
git checkout v2

cd ..
git add .
git commit -m 'Switch submodule to v2'
```

If the version isn't tagged, the commit sha-1 can be used:

```bash
cd test-submodule/
git fetch
git checkout 916712fc7771ba5cb22915150e0facfd265d2d37

cd ..
git add .
git commit -m 'Switch submodule to v3 [WIP]'
```

## Switch to Latest Commit

Switch to the latest commit on the submodule master branch:

```bash
cd test-submodules/
git pull origin master

cd ..
git add .
git commit -m 'Switch submodule to latest commit'
```

# Cloning a Project with Submodules

After cloning a project that includes submodules they must be initiated before they can be used:

```bash
git clone https://github.com/thomasmarkrea/test-submodule-project.git
git submodule update --init
```

## Links

Git's own submodule documentation:

- [git-scm.com/book/en/v2/Git-Tools-Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
- [git-scm.com/docs/git-submodule](https://git-scm.com/docs/git-submodule)
