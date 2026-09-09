---
permalink: /
title: "1Gould"
author_profile: true
redirect_from:
  - /about/
  - /about.html
---

Welcome to my personal site. This is where I share security career notes, technical writeups, and projects.

You can find my work on [GitHub](https://github.com/1Gould), browse my [projects](/portfolio/), or read my [blog posts](/year-archive/).

## Certifications

Certification details coming soon.

<!-- Replace the text above with your certifications, for example:
- Certification name — Issuing organization, Year
-->

## Recent Training

Recent courses and training details coming soon.

<!-- Replace the text above with your recent training, for example:
- Course or training name — Provider, Month Year
-->

## Projects

{% for project in site.portfolio %}
- [{{ project.title }}]({{ project.url | relative_url }})
{% else %}
Project writeups coming soon.
{% endfor %}

[View all projects]({{ '/portfolio/' | relative_url }})
