+++
title = "Mole, a Hugo Theme"
description = ""
categories = []
tags = ['Hugo']
date = 2020-09-06T16:52:11+01:00
draft = true
+++

A Hugo theme in four files

- `layouts/_default/baseof.html`
- `layouts/_default/single.html`
- `layouts/_default/list.html`
- `layouts/index.html`

## Install

Install mole as a git submodule 

```bash
git submodule add https://github.com/thomasmarkrea/mole.git themes/mole
```

Add mole to the `theme` section of Hugo config file

```bash
# config.toml

...

theme = "mole"

...
```

## How it Works

*running the code + adding the file content in this section will give an exact copy of Mole v0.1.1*

```bash
mkdir mole
cd mole

mkdir -p layouts/_default
```

### baseof.html

Base template that everything else sits inside.

Main block, `{{ block "main" . }}{{ end }}`, will be populated with individual page content.

Footer nav controlled by main menu block in `config.toml.`

```bash
touch layouts/_default/baseof.html
```

```html
# layouts/_default/baseof.html

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name=viewport content="width=device-width, initial-scale=1">
    
    <title>{{ .Title }}</title>
    <meta name="description" content="{{ .Description | default .Site.Params.description }}">
    
    <style>
      @import url('https://fonts.googleapis.com/css2?family=Lato:ital,wght@0,100;0,300;0,400;0,700;0,900;1,100;1,300;1,400;1,700;1,900&display=swap');

      body {
        font-family: 'Lato', sans-serif;
        max-width: 70ch;
        padding: 2ch;
        margin: auto;
        background-color: #FFFADD;
      }
      h1, h2, h3 {
        font-style: italic;
        font-weight: 900;
      }
      a {
        color: #1438FF;
        text-decoration: none;
        outline: 0;
      }
      a:hover {
        text-decoration: underline;
      }
      img {
        max-width: 100%;
      }
      ul {
        list-style-type: none;
        padding: 1rem 0;
      }
      li {
        margin-bottom: 0.5rem;
      }
      ul time {
        margin-right: 2ch;
        font-family: monospace;
      }
      pre {
        padding: 2ch;
        background-color: #EDEDED;
        overflow: scroll;
      }
      ::selection {
        background-color: #FFED8A;
      }
    </style>
  </head>
  <body>
    <header>
      <h1><a href="/">{{ .Site.Title }}</a></h1>
    </header>
    {{ block "main" . }}{{ end }}
    <footer>
      <hr>
      &copy; {{ now.Year }} {{ .Site.Title }}
      <nav style="display:block;float:right">
        {{ range .Site.Menus.main }}
          <a href="{{ .URL }}">{{ .Name }}</a>{{ with .Post }}{{ . }}{{ end }}
        {{ end }}
      </nav>
    </footer>
  </body>
</html>
```

### single.html

Template for content pages (e.g. posts & about me)

```bash
touch layouts/_default/single.html
```

```html
# layouts/_default/single.html

{{ define "main" }}
<main>
  <article>
    <header>
      <h1>{{ .Title }}</h1>
      {{ with .Date}}
        <time datetime="{{ . }}">{{ .Format ("January 2, 2006") }}</time>
      {{ end }}    
    </header>
    {{ .Content }}
  </article>
</main>
{{ end }}
```

### list.html

Template for list pages (e.g. post archive)

```bash
touch layouts/_default/list.html
```

```html
# layouts/_default/list.html

{{ define "main" }}
<main>
  <ul>
    {{ range .Pages }}
      <li>
        <article>
          <time datetime="{{ .Date }}">{{ .Date.Format "Jan 2, 2006" }}</time>
          <a href="{{ .Permalink }}">{{ .Title }}</a>
        </article>
      </li>
    {{ end }}
  </ul>
</main>
{{ end }}
```

### index.html

Special list template for the home page.

Same as standard list template but with [pagination](https://gohugo.io/templates/pagination/).

```bash
touch layouts/index.html
```

```html
# layouts/index.html

{{ define "main" }}
<main>
  <ul>
    {{ $paginator := .Paginate (where .Site.RegularPages "Type" "in" .Site.Params.mainSections) }}
		{{ range $paginator.Pages }}
      <li>
        <article>
          <time datetime="{{ .Date }}">{{ .Date.Format "Jan 2, 2006" }}</time>
          <a href="{{ .Permalink }}">{{ .Title }}</a>
        </article>
      </li>
    {{ end }}
  </ul>
  <nav style="font-family:monospace">
    <a href="{{ if .Paginator.HasPrev }}{{ .Paginator.Prev.URL }}{{ end }}"><</a>
    {{ .Paginator.PageNumber }} of {{ .Paginator.TotalPages }}
    <a href="{{ if .Paginator.HasNext }}{{ .Paginator.Next.URL }}{{ end }}">></a>
	</nav>
</main>
{{ end }}
```

## Thanks

For a lot of the design inspiration:

- [https://jrl.ninja/](https://jrl.ninja/)

For understanding how the basics of Hugo themes work:

- [https://morph.sh/](https://morph.sh/) ([https://github.com/colorchestra/smol](https://github.com/colorchestra/smol))