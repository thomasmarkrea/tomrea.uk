+++
title = "r2hugo"
description = ""
categories = []
tags = ['Hugo', 'R']
date = 2020-08-31T20:24:07+01:00
draft = true
+++

Create Hugo content from R Markdown.

r2hugo is a Hugo [theme component](https://gohugo.io/hugo-modules/theme-components/). It works alongside your normal theme and allows you to easily create content from R Markdown files.

## Install

Add r2hugo to your project as a git submodule

```bash
git submodule add https://github.com/thomasmarkrea/r2hugo.git themes/r2hugo
```

And update Hugo's config file, add

- `r2hugo` to `theme`
- `.rmd` to `ignoreFiles`

```toml
# config.toml

...

theme = ["r2hugo", ...]
ignoreFiles = [ "\\.rmd$" ]

...
```

## Usage

Use `hugo new` to create a new post.

Add the `-k` flag to tell Hugo to use the `r2hugo` archetype

```
hugo new -k r2hugo posts/test-r2hugo-post
```

This will create a new directory in `content/posts` containing `index.md` and `content.rmd`

```
content
└── posts
    └── test-r2hugo-post
        ├── index.md
        └── content.rmd
    └── ...
```

`index.md` is a standard Hugo content file, except where the content usually goes there is a [shortcode](https://gohugo.io/content-management/shortcodes/)

```
# content/posts/test-r2hugo-post/index.md

+++
title = "test-r2hugo-post"
description = "Post to test r2hugo"
categories = ['test']
tags = ['hugo', 'r']
date = 2020-01-01T13:00:00+00:00
draft = true
+++

{{% include "posts/test-r2hugo-post/content.md" %}}
```

`content.rmd` is where the actual content goes

````
# content/posts/test-r2hugo-post/content.rmd

---
output:
  md_document:
    variant: commonmark
---

Post to test r2hugo.

# Example Code

```{r}
print("example code")
```

# Example Plot

```{r}
plot(1:10,1:10)
```
````

To test the page, *knit* `content.rmd` and run 

```bash
hugo server -D
```

> **Note:** If you create content in a directory other than `posts/` you will need to manually update the path in the `include` shortcode.
>
> This will be fixed in a future version once this issue is resolved [https://github.com/gohugoio/hugo/issues/7589](https://github.com/gohugoio/hugo/issues/7589)

## How it Works

*running the code + adding the file content in this section will give an exact copy of r2hugo v0.1.1*

r2hugo consists of a shortcode and an archetype.

```bash
mkdir r2hugo
cd r2hugo
```

### Shortcode

```bash
mkdir -p layouts/shortcodes
touch layouts/shortcodes/include.html
```

The shortcode, named `include`, is used to take the output of an R Markdown file and include it within a page. 

It takes a file as input (`[.Get 0](https://gohugo.io/functions/get/)`) and outputs its contents as a string (`[readFile](https://gohugo.io/functions/readfile/)`). It treats the input as 'safe' and doesn't do any html escaping (`[safeHTML](https://gohugo.io/functions/safehtml/)`).

```bash
# layouts/shortcodes/include.html

{{ .Get 0 | readFile | safeHTML }}
```

### Archetype

The archetype contains two files that make up a Hugo [leaf bundle](https://gohugo.io/content-management/page-bundles/#leaf-bundles) 

```bash
mkdir -p archetypes/r2hugo

touch archetypes/r2hugo/index.md
touch archetypes/r2hugo/content.rmd
```

[`index.md`](http://index.md) is the main file Hugo will use to create the page.

It holds the content's front matter and the `include` shortcode.

```
# archetypes/r2hugo/index.md

+++
title = "{{ replace .Name "-" " " | title }}"
description = ""
categories = []
tags = []
date = {{ .Date }}
draft = true
+++

{{% include "posts/{{ .Name }}/content.md" %}}
```

`content.rmd` is where the R Markdown content goes.

It is a standard R Markdown file that will knit to a [commonmark](https://commonmark.org/) file - `content.md`.

```
# archetypes/r2hugo/content.rmd

---
output:
  md_document:
    variant: commonmark
---
```

When the site is built by Hugo, it will combine `index.md` with the knited `content.md` and build a single page.

## Thanks

I may not be using them, but I wouldn't be using Hugo without them:

- [https://github.com/rstudio/blogdown](https://github.com/rstudio/blogdown)
- [https://github.com/r-lib/hugodown/](https://github.com/r-lib/hugodown/)

Also, this post from rOpenSci was very useful, we seem to have been working through the same issues:

- [https://ropensci.org/technotes/2020/04/23/rmd-learnings/](https://ropensci.org/technotes/2020/04/23/rmd-learnings/)