---
layout: page
title: 1Gould
permalink: /
---

![Cybersecurity workspace]({{ '/assets/img/security-workspace.png' | relative_url }}){: width="1200" height="675" }

## About

Documenting research in windows internals, malware development and analysis.

## Projects

Pending...

## Certifications

- Security+
- OSCP
- OSAI

## Recent Training


## Latest Security Posts

{% for post in site.posts limit:5 %}
- [{{ post.title }}]({{ post.url | relative_url }}) - {{ post.date | date: "%B %-d, %Y" }}
{% endfor %}
