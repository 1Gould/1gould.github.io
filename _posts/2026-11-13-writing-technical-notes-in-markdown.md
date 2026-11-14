---
title: "Writing technical notes in Markdown"
description: "A small, reproducible structure for technical posts — with code, context, and results."
date: 2026-11-13 00:00:00 +0000
---
A useful technical note should make it possible for someone else to understand the problem and repeat the work. Markdown keeps the source readable while leaving room for commands, examples, and references.

## Start with the problem

State what you wanted to achieve, what happened instead, and the constraints. Include relevant versions and environment details. A command that works on one operating system may behave differently on another.

## Show a reproducible example

Use fenced code blocks with a language name for syntax highlighting. Keep the example small and explain what it demonstrates.

```python
from pathlib import Path

# List Markdown files in a stable order.
for path in sorted(Path("_posts").glob("*.md")):
    print(path.name)
```

This lists Markdown files directly inside `_posts`. It does not recurse into subdirectories. Sorting makes the output deterministic; date-prefixed filenames also sort chronologically.

> Never include credentials, private keys, or sensitive production data in a public example.

## Separate observations from assumptions

Record the result, not just the command. If you have not verified an explanation, label it as a hypothesis.

| Include | Why it helps |
| --- | --- |
| Environment and versions | Makes differences easier to diagnose |
| Small input example | Removes unrelated complexity |
| Expected and actual results | Makes the failure explicit |
| Limitations | Shows where the solution may not apply |

## Publish a post on this site

Create a file in `_posts` named `YYYY-MM-DD-short-title.md`. Start it with YAML front matter:

```yaml
---
title: "Understanding a tricky bug"
description: "The symptom, the cause, and the fix."
date: 2026-11-13 09:00:00 +0000
---
```

Write the article in Markdown below the closing `---`. The homepage picks up published posts automatically and lists them newest first. Posts dated in the future stay hidden until a build occurs after their publication time; publishing is triggered by a push, not by a scheduled job. To keep a post unpublished regardless of its date, set `published: false` in its front matter.

Preview the site locally:

```sh
bundle install
bundle exec jekyll serve
```

Open `http://localhost:4000`. When ready, push to `main`; the GitHub Actions workflow builds and deploys the site. In the repository's **Settings → Pages**, the publishing source must be **GitHub Actions**.

The profile sections live in `index.md`; edit them with ordinary Markdown. Change the site name and description in `_config.yml`. This introductory post is a working example and can be removed when you publish your own notes.

## Leave useful references

Link to the documentation you used and explain any trade-offs. A short note with a reproducible example is more useful than a long one without evidence.

- [Markdown basics](https://www.markdownguide.org/basic-syntax/)
- [Jekyll posts and front matter](https://jekyllrb.com/docs/posts/)
