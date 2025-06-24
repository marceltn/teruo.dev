---
layout: post
title:  "Jekyll and Github Actions"
date:   2025-05-28 08:00:00 +0900
categories:
  - jekyll
  - github acitons
  - troubleshooting
---

## Context

I wanted to start my blog with the simplest possible setup. As a software developer, GitHub Pages felt like the natural choice — it's free, integrates cleanly with a Git-based workflow, and requires minimal infrastructure.

Reading the <a href="https://docs.github.com/en/pages" target="_blank" rel="noopener noreferrer">official GitHub Pages documentation</a> they suggested to use Jekyll to create a website. Everything worked smoothly… at first.

But then I noticed the Jekyll plugin versions were outdated. As a best practice, bump the dependencies must be a common skill for a programmer, so I started updating the dependencies. That’s when the fun began — and by “fun” I mean “dependency chaos”

In this post, I’ll walk through the setup, the roadblocks I hit, and how I fixed them — with useful links and tips I’ll probably revisit myself.

---

## Starting Simple

I bootstrapped the project using the <a href="https://jekyllrb.com/docs/" target="_blank" rel="noopener noreferrer">Jekyll Quickstart</a>. Got a basic site running locally, then set up a GitHub Actions workflow to build and deploy the site automatically.

To see who’s visiting, I decided to add some basic analytics. After some digging, I went with <a href="https://simpleanalytics.com/" target="_blank" rel="noopener noreferrer">Simple Analytics</a> — privacy-friendly and easy to set up. And also <a href="https://umami.is/" target="_blank" rel="noopener noreferrer">Umami</a> for benchmark comparison. Was just adding a script tag in the HTML header.

Cool. But where’s the header?

---

## Themes and Outdated Dependencies

Digging into the *minima* theme’s structure, you will find the <a href="https://github.com/jekyll/minima/blob/0b7ca6bbdb782a646f8e7b78b1a29fd5032ad4d3/_layouts/base.html#L4" target="_blank" rel="noopener noreferrer">base.html file</a> that points to <a href="https://github.com/jekyll/minima/blob/0b7ca6bbdb782a646f8e7b78b1a29fd5032ad4d3/_includes/head.html#L12" target="_blank" rel="noopener noreferrer">head.html</a> file that refers to `custom-head.html` file. To add the HTML header, should be in this file but in your project.

I added only a configuration to collect analytics data only when it's live. Because I don't want to have metrics when I'm developing locally. So this condition will be fine:

{% highlight liquid mark_lines="1 3" %}
{% raw %}{%- if jekyll.environment == 'production' %}
  <!-- your script analytics here -->
{%- endif -%}{% endraw %}
{% endhighlight %}

And to have it work with Github actions, just add `JEKYLL_ENV` when building your website:

{% highlight yaml mark_lines="5" %}
      - name: Build with Jekyll
        # Outputs to the './_site' directory by default
        run: bundle exec jekyll build --baseurl "${{ steps.pages.outputs.base_path }}"
        env:
          JEKYLL_ENV: production
{% endhighlight %}


When I render the website the layout seems a little different from the demo, so I realized I was using an older version (v2) and not the latest (v3).

Following the <a href="https://en.wikipedia.org/wiki/Broken_windows_theory" target="_blank" rel="noopener noreferrer">broken windows theory</a>, I decided to clean up and upgrade the theme. The <a href="https://github.com/jekyll/minima" target="_blank" rel="noopener noreferrer">minima GitHub repo</a> has a good instructions on how to use the newer version.

I did the same for for Jekyll version. Based on the <a href="https://pages.github.com/versions/" target="_blank" rel="noopener noreferrer">Github page dependency</a>, they support by default the version 3 of jekyll, but by configuring the Github Actions, I could use the verison 4.

---

## Gotchas with GitHub Actions and Local Builds

### ✅ Tips That Helped

- Pin your dependencies versions. For ruby version, jekyll version and theme version. Then we can avoid some mismatches between local and GitHub Actions environments.
- Add `bundle config set --local path 'vendor/bundle'` to isolate gem installs.
- If plugins break, check for exact version compatibility in the <a href="https://jekyllrb.com/docs/continuous-integration/github-actions/" target="_blank" rel="noopener noreferrer">GitHub Actions Jekyll docs</a>.
- To customize your site by adding the analytics, read the documentation always.

---

## Conclusion

What started as a simple blog setup turned into a fun debugging session. But in the process, I learned a lot about how Jekyll, themes, Liquid templates and GitHub Actions really work together.

If you’re starting your own blog or bumping version of old one, I hope these notes save you some time.

---

**Up next:** how I’m adding custom components, organizing my posts and making the blog a little more *me*. Stay tuned!

