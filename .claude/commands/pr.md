---
description: Create a pull request with a brief description
---

Create a pull request for the current branch:

1. Check `git status` to see current branch and if it's pushed
2. Review commits with `git log master..HEAD` to understand what's being added
3. Push the branch if needed: `git push -u origin <branch-name>`
4. Create PR with `gh pr create --title "title" --body "Brief description"`

Important:
- Keep the PR description brief and clear
- Focus on what was changed and why
- DO NOT include any Claude/Anthropic attribution
- Use simple, direct language
