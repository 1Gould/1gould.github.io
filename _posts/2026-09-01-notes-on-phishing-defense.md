---
title: "Notes on Phishing Defense"
date: 2026-09-01 10:00:00 -0400
categories: [Security, Detection]
tags: [phishing, mfa, email-security]
description: "Practical controls and habits that reduce phishing risk."
---

Phishing defense works best as layered risk reduction. Training helps, but it should not be the only control. Strong identity protections, email filtering, reporting workflows, and fast investigation habits all matter.

## Controls that matter

- Require multi-factor authentication for important accounts
- Prefer phishing-resistant methods where possible
- Block lookalike domains and suspicious attachments
- Add a clear way for users to report suspicious messages
- Review authentication logs after confirmed phishing attempts

## Investigation checklist

When a suspicious message is reported, preserve the original message headers, URLs, attachment names, sender details, and timestamps. Check whether other mailboxes received the same message, then search for clicks, sign-ins, mailbox rules, and unusual forwarding settings.

## Practical takeaway

The goal is not to make every user perfect. The goal is to make compromise harder, detection faster, and recovery cleaner.
