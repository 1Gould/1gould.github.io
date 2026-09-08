# Daniel Gould Security Blog

A simple personal cybersecurity blog built with Jekyll and the Chirpy theme for GitHub Pages.

## Local Preview

Install Ruby and Bundler, then run:

```bash
bundle install
bundle exec jekyll serve
```

Open `http://localhost:4000/blog/`.

## Writing Posts

Create Markdown posts in `_posts` using this filename format:

```text
YYYY-MM-DD-post-title.md
```

Each post needs front matter:

```yaml
---
title: "Post Title"
date: 2026-09-01 09:00:00 -0400
categories: [Security, Topic]
tags: [tag-one, tag-two]
description: "Short summary for search and previews."
---
```

## GitHub Pages

Push this folder to a repository named `blog`, then enable GitHub Pages with GitHub Actions as the source.
