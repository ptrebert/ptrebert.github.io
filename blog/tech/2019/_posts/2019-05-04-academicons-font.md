---
layout: post
title: Academicons font
tags: jekyll academic-icons how-to
---

# Objective
Use "academic icons" for links, e.g., to your ORCID profile.
Embed this font into the "So Simple" Jekyll theme.

The following applies to Academicons v1.8.6
and "So Simple" v3.1.2

# Procedure
- Download Academicons zip
- Unzip and change to your Jekyll root
- Copy CSS files `academicons.css` and `academicons.min.css` into `/assets/css`
- Copy font files `academicons.eot`, `acedemicons.svg`, `academicons.ttf`,
and `acedemicons.woff` into `/assets/fonts`
- Adapt the `_includes/head.html` and add the path to the CSS
`<link rel="stylesheet" href="{{ '/assets/css/academicons.css' | relative_url }}"/>`
  - Adding the CSS link before the line `<link rel="stylesheet" href="{{ '/assets/css/main.css' | relative_url }}">`
  seems to do the job