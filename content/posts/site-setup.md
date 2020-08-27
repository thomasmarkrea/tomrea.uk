+++
title = "Site Setup"
description = "Additional steps to set up a Hugo site"
categories = []
tags = ['Hugo']
date = 2020-08-27T13:39:13+01:00
draft = true
+++

Additional steps to set up this site.

Follows [Hugo Quick Start](/posts/hugo-quick-start/).

# .gitignore

Add a gitignore and push site to github

```bash
touch .gitignore
```

```bash
# .gitignore

# Hugo files
public
resources

# R project files
.Rproj.user
.Rhistory
.RData
.Ruserdata
```

# Archetypes

Update default archetype

```toml
# archetypes/default.md

+++
title = "{{ replace .Name "-" " " | title }}"
description = ""
categories = []
tags = []
date = {{ .Date }}
draft = true
+++
```

# r2hugo

Install [r2hugo](https://github.com/thomasmarkrea/r2hugo) so posts can be created using R Markdown.

# R Studio Project

Create R Studio project to make working with R easier

```bash
touch r.Rproj
echo Version: 1.0 >> r.Rproj
```

# Config

## Basic

Add basic site config

```toml
# config.toml

baseURL = "https://tomrea.uk/"
languageCode = "en-gb"
title = "tomrea.uk"
theme = ["r2hugo", "mole"]
ignoreFiles = [ "\\.rmd$" ]

[params]
  description = "Data analysis, R and other things"
```

## Menu

Configure site menu

```toml
# config.toml

...

[menu]
  [[menu.main]]
    weight = 1
    name = "home"
    url = "/"
    post = " |"
  [[menu.main]]
    weight = 2
    name = "about"
    url = "/about/"
    post = " |"
  [[menu.main]]
    weight = 3
    name = "posts"
    url = "/posts/"
```

## Markup

Turn code highlighting off

```toml
# config.toml

...

[markup]
  [markup.highlight]
    codeFences = false
```

# About

Create an about me page

```bash
hugo new about.md
```

# Host

Host site with [Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/)

Add `netlify.toml` to configure the build

```bash
touch netlify.toml
```

```toml
# netlify.toml

[build]
  publish = "public"
  command = "hugo" 

[build.environment]
  HUGO_VERSION = "0.74.3"

[context.draft]
  command = "hugo -D --baseURL https://draft.tomrea.uk/"

[context.dev]
  command = "hugo --baseURL https://dev.tomrea.uk/"
```

Use UI to set up:

- [Custom Domain](https://docs.netlify.com/domains-https/custom-domains/#definitions)
- [Deploy Contexts](https://docs.netlify.com/site-deploys/overview/#branch-deploy-controls)
- [Branch Subdomains](https://docs.netlify.com/domains-https/custom-domains/multiple-domains/#branch-subdomains)