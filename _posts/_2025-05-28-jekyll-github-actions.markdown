---
layout: post
title:  "Jekyll and Github Actions"
date:   2025-05-28 08:00:00 +0900
categories:
  - jekyll
  - github acitons
  - troubleshooting
---

# Context
I wanted to start my blog with the simplest possible setup. As a software developer, GitHub Pages felt like the natural choice as it keeps everything in a Git workflow, and it's free.

I followed the official GitHub Pages documentation and went with Jekyll using the default minima theme. It all worked smoothly… at first.

But then I noticed the Jekyll plugin versions were outdated. I like to keep my stack up to date, so I started bumping the dependencies. This is the time when things got messy.

In this post, I’ll share the issues I faced into and how I fixed them.

# Follow the documentation
Bootstraped with the quickstart guide from Jekyll official web site: https://jekyllrb.com/docs/
Configured the github actions workflow.
Run everything locally and it works fine.

next step was to add some metrics just to know how many users are accessing the website.
so, choose the simpleanalytics for that and when I'll add the header tag, I realized that my version of the current theme was outdated.
following the broken window rule, I'll keep this code up to date as far as I can, so started updating.
issue to run locally.
https://jekyllrb.com/docs/continuous-integration/github-actions/


Stay tuned for the gotchas and fixes.

https://jekyllrb.com/docs/continuous-integration/github-actions/

