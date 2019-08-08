---
layout: post
title: Jekyll and bundler
tags: jekyll bundler how-to build
---

# Note to self...
The documentation for the general how to build the Jekyll
blog on the local machine and serve it (localhost:4000) can
be found here:

[jekyllrb.com/tutorials/using-jekyll-with-bundler](https://jekyllrb.com/tutorials/using-jekyll-with-bundler)

On the shell, the following commands are needed:

```
# continuous building
bundle exec jekyll build --watch

# make site accessible under localhost:4000
bundle exec jekyll serve
```