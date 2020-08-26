+++
title = "Hugo Quick Start"
description = "Steps to get started with Hugo"
categories = []
tags = ['Hugo']
date = 2020-08-26T18:05:49+01:00
draft = false
+++

Steps to get started with Hugo.

Based on [gohugo.io/getting-started/quick-start/](https://gohugo.io/getting-started/quick-start/)

```
hugo new site tomrea.uk
cd tomrea.uk
```

```
git init
git checkout -b main
```

```
git submodule add https://github.com/thomasmarkrea/mole.git themes/mole
echo 'theme = "mole"' >> config.toml
```

```
hugo new posts/test-page.md
```

```
# -D includes drafts
hugo server -D
```