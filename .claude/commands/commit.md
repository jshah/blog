---
description: Commit changes with a clear, concise message
---

Create a git commit with current changes and push:

1. Run `git status` and `git diff` to see what's changed
2. Draft a clear, concise commit message that describes the changes
3. Add relevant files with `git add`
4. Create the commit with `git commit -m "message"`
5. Push with `git push --set-upstream origin <branch-name>`
6. Run `git status` to confirm

Important:
- DO NOT include any Anthropic/Claude attribution or co-author tags
- Keep commit messages simple and clear
- Use imperative mood (e.g., "add feature" not "added feature")
- Always use --set-upstream when pushing (creates branch if first time)
