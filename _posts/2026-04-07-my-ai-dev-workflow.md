---
layout: post
title: "My AI Dev Workflow"
date: 2026-04-07 10:00:00 -0700
categories: ai developer-tools workflow
---

I've been building my dev workflow around AI tooling over the last year and wanted to share what that looks like today. My setup is layered. Everything is built on top of [Claude Code](https://code.claude.com/docs){:target="_blank"} as the foundation, with custom skills, plugins, integrations, and orchestration tools on top.

This post walks through each layer.

# The Foundation: Claude Code

[Claude Code](https://code.claude.com/docs){:target="_blank"} is the CLI that powers everything. It's my coding environment. It reads files, writes code, runs commands, and interacts with git. I work in it the way I'd work in a terminal, and everything else in this post layers on top of it.

# Custom Skills

Skills are the most useful part of my setup. They're portable `.md` files you drop into a project's `.claude/skills/` directory that act as reusable slash commands. I've built 10 that automate most of my dev lifecycle.

### Git & Branch Management

- **`/branch`** creates branches with standardized naming and integrates with the Atlassian MCP server to auto-fetch Jira ticket titles and generate branch names from them.
- **`/commit`** handles pre-commit hook failures by auto-re-staging after formatters run, so commits don't fail on formatting issues.
- **`/fix-merge-conflicts`** detects whether conflicts are local or on GitHub and resolves them accordingly.

### PR Workflow

- **`/pr`** auto-detects the base branch, extracts ticket IDs from the branch name, finds PR templates, and generates descriptions focused on "why" not "what."
- **`/address-pr-feedback`** reads all unresolved PR comments, groups them by topic, makes changes, creates logical commits, and replies to each thread with the commit SHA. It can also reject feedback with reasoning.
### Code Review

- **`/code-walkthrough`** is an interactive, educational review. It breaks changes into sections (database, backend, frontend, tests) and walks through them conversationally. Designed to teach, not just check boxes.
- **`/pr-review`** is an autonomous review that posts inline GitHub comments with severity tags (Critical, Warning, Suggestion). It runs end-to-end without interaction.
- **`/final-review`** runs 5 parallel review subagents before merge for a thorough last pass.

### Testing

- **`/add-tests`** analyzes the branch diff, detects existing test patterns and frameworks, identifies untested behaviors, and writes tests in a separate subagent context.

### Communication

- **`/ship-it`** generates Slack ship announcements from commits, PRs, or branches. It focuses on user-facing impact, not technical details.

### The Workflow

A typical feature flows through these naturally: `/branch` to start, write code, `/commit`, `/pr`, `/pr-review` or `/code-walkthrough`, `/address-pr-feedback` if there are comments, and `/ship-it` when it lands.

One pattern I want to highlight: `/add-tests` and `/pr-review` deliberately run their analysis in a separate subagent context, independent from the implementation context. The review doesn't have access to the reasoning behind the code, so it evaluates what's actually there rather than what was intended. This avoids confirmation bias.

# Plugins

I use a few plugins from the Claude Code marketplace that add structured workflows on top of the base tool.

[**Superpowers**](https://github.com/obra/superpowers){:target="_blank"} adds structured workflows for TDD, systematic debugging, brainstorming, plan writing and execution, parallel agent dispatch, and git worktrees.

[**Frontend Design**](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design){:target="_blank"} generates UI that avoids the generic look that AI-generated interfaces tend to have.

[**Feature Dev**](https://github.com/anthropics/claude-code/tree/main/plugins/feature-dev){:target="_blank"} provides guided feature development with specialized subagents for codebase exploration, architecture design, and code review.

**Security Guidance** is a hook-based plugin that fires on every edit and write operation. It detects patterns like command injection, XSS, `eval()`, and pickle deserialization in real-time and shows context-specific warnings without blocking.

I also use a few community skills from the Vercel engineering team:

- [Vercel React Best Practices](https://github.com/vercel-labs/agent-skills/tree/main/skills/react-best-practices){:target="_blank"} covers performance rules across priority-ranked categories like eliminating waterfalls, bundle size optimization, and rendering performance.
- [Vercel Composition Patterns](https://github.com/vercel-labs/agent-skills/tree/main/skills/composition-patterns){:target="_blank"} covers React composition patterns that scale: compound components, explicit variants over boolean props, state decoupling.
- [Web Design Guidelines](https://github.com/vercel-labs/agent-skills/tree/main/skills/web-design-guidelines){:target="_blank"} reviews code for accessibility, semantic HTML, responsive design, touch targets, and keyboard navigation.

# Integrations: MCP Servers

MCP (Model Context Protocol) servers give Claude direct access to external tools and services during development. The ones I get the most value from:

- **Playwright** for browser automation. Navigate pages, fill forms, take screenshots, run end-to-end test flows.
- **Chrome DevTools** for performance traces, network request analysis, console messages, and Lighthouse audits.
- **Figma** to access design files, extract component information, and translate designs into code.
- **Sentry** for error monitoring and debugging context during development.
- **Datadog** to query metrics, traces, and logs while debugging.

I also have MCP servers configured for GitHub, Slack, Supabase, Context7 (up-to-date library docs), Terraform, and a few others. Most of these are set-and-forget. They're there when I need them.

# Orchestration: Superset & Conductor

This is the layer where things move from a single agent to multiple agents working in parallel.

[**Superset**](https://superset.sh/){:target="_blank"} is a terminal-based orchestration platform for running multiple AI coding agents in parallel. It supports any CLI-based agent (Claude Code, Codex, Cursor, Gemini, etc.), creates isolated git worktrees for each one, and gives you real-time monitoring and task switching between them. It's the infrastructure that lets me run several things at once without stepping on myself.

[**Conductor**](https://www.conductor.build/){:target="_blank"} by Melty Labs is a macOS app for running a team of coding agents in parallel. It supports Claude Code and Codex agents, each in an isolated git worktree. Conductor provides a unified dashboard showing what each agent is working on, with code review and merging built in.

I've found that single-agent workflows are useful on their own, but being able to parallelize independent work across multiple agents changes how I think about breaking down tasks.

---

This is a snapshot. The setup changes frequently as tools get added, skills get rewritten, and new integrations show up. If you're building something similar or have recommendations, let me know.
