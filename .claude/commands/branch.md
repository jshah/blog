---
description: Create and switch to a new branch
---

Create a new branch based on the user's description:

1. Take the user's input text describing what they're trying to accomplish
2. Summarize it into a concise 3-5 word kebab-case branch name
3. Create and switch to the branch: `git checkout -b jshah/<branch-name>`
4. Confirm with `git branch --show-current`

Important:
- Branch format: `jshah/<short-description>`
- Use kebab-case (lowercase with hyphens)
- Keep it concise: 3-5 words maximum
- Examples:
  - "I want to add a new blog post about testing" → `jshah/add-testing-post`
  - "Fix the broken navigation links" → `jshah/fix-navigation-links`
  - "Update the about page with new information" → `jshah/update-about-page`
