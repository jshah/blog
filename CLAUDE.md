# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a personal blog built with Jekyll static site generator, based on the Hyde theme. The blog is hosted at https://jshah.dev and contains technical articles about software engineering, processes, and development practices.

## Key Commands

### Development Server
```bash
bundle install              # Install Ruby dependencies
bundle exec jekyll serve    # Start local development server at http://localhost:4000
bundle exec jekyll serve --drafts  # Include draft posts in development
```

### Build
```bash
bundle exec jekyll build    # Build static site to _site/ directory
JEKYLL_ENV=production bundle exec jekyll build  # Production build (includes analytics)
```

## Architecture & Structure

### Content Organization
- **`_posts/`**: Blog posts in Markdown format with YYYY-MM-DD-title.md naming convention
- **`_layouts/`**: HTML templates (default.html, page.html, post.html)
- **`_includes/`**: Reusable components (head.html, sidebar.html, analytics.html)
- **`public/`**: Static assets (CSS, images, icons)
- **`_site/`**: Generated static site (ignored in git)

### Post Format
Blog posts require YAML front matter with:
```yaml
---
layout: post
title: "Post Title"
date: YYYY-MM-DD HH:MM:SS -0700
categories: space-separated tags
---
```

### Theme Customization
The blog uses Hyde theme with custom styling:
- **Base styles**: `public/css/poole.css` (foundational typography and elements)
- **Hyde theme**: `public/css/hyde.css` (sidebar and layout)
- **Custom overrides**: `public/css/custom.css` (site-specific customizations)
- **Syntax highlighting**: `public/css/syntax.css` (code blocks)

### Key Configuration (_config.yml)
- Site metadata (title, description, URL)
- Pagination set to 1 post per page
- Jekyll plugins: jekyll-seo-tag, jekyll-paginate, jekyll-gist
- Markdown processor: kramdown with rouge syntax highlighter

### Sidebar Navigation
The sidebar dynamically generates navigation from pages with `layout: page` in their front matter. It also includes social media links (RSS, GitHub, Twitter, LinkedIn).

### Analytics
Google Analytics is conditionally loaded only in production builds (when `JEKYLL_ENV=production`).

## Creating New Posts

1. Create a new file in `_posts/` following the naming convention: `YYYY-MM-DD-post-title.md`
2. Add required front matter (layout, title, date, categories)
3. Write content in Markdown
4. Test locally with `bundle exec jekyll serve`
5. Commit and push to deploy (git branch: master for production)
