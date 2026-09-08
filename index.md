---
layout: page
title: Daniel Gould
permalink: /
---

![Cybersecurity workspace]({{ '/assets/img/security-workspace.png' | relative_url }}){: width="1200" height="675" }

## Career

I am building a career around cybersecurity, hands-on technical projects, and clear documentation. My interests include defensive security, vulnerability analysis, incident response, malware analysis, cloud security, and secure systems administration.

This site collects my career path, selected projects, certifications, and practical security notes.

## Projects

### Home Security Lab

Documented lab for practicing Linux hardening, logging, endpoint monitoring, and network detection.

### Malware Analysis Notes

Static and dynamic analysis writeups focused on behavior, indicators, and safe reverse engineering habits.

### Cloud Security Checks

Small scripts and checklists for reviewing identity, storage, network exposure, and audit configuration.

## Certifications

- Security+ or equivalent fundamentals
- Network security and systems administration coursework
- Cloud, incident response, or malware analysis certifications in progress

## Latest Security Posts

{% for post in site.posts limit:5 %}
- [{{ post.title }}]({{ post.url | relative_url }}) - {{ post.date | date: "%B %-d, %Y" }}
{% endfor %}
