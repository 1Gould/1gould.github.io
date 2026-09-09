# Daniel Gould — Academic Pages

Personal site at https://1gould.github.io, using the [Academic Pages](https://github.com/academicpages/academicpages.github.io) Jekyll template (MIT license; see LICENSE).

## Customize

- `_config.yml`: name, biography, profile image, social links, theme colors.
- `_pages/about.md`: home page.
- `_data/navigation.yml`: header links.
- `_posts/YYYY-MM-DD-title.md`: blog posts, with YAML front matter containing `title`, `date`, and `layout: single`.
- `_portfolio/`: project Markdown files, with front matter containing `title` and `collection: portfolio`.
- `images/` and `files/`: images and downloadable attachments.

The sample publications, talks, teaching, CV, and blog entries have been omitted. The theme layouts and collection configuration remain available for adding these sections later.

## Preview

Install Ruby and Bundler, then run:

```sh
bundle install
bundle exec jekyll serve
```

Visit http://localhost:4000. Restart the server after changing `_config.yml`.

## Deployment

The existing `.github/workflows/pages-deploy.yml` builds and deploys on pushes to `main`, or manually through GitHub Actions. In repository Settings → Pages, select GitHub Actions as the build source.
