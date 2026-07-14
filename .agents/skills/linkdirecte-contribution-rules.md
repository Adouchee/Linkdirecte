---
name: linkdirecte-contribution-rules
description: Essential guidelines and standards for contributing to the Linkdirecte codebase, including AGENTS.md rules, conventional commits, Pull Request templates, and Bun usage. Always use this skill when creating a Pull Request, submitting changes, editing files, or running pre-commit steps.
---

# Linkdirecte Contribution Rules

These guidelines are mandatory for any AI agent or developer contributing to this repository. They reduce errors and ensure the developer experience remains high.

## 1. Pull Request Formatting Template

When submitting a pull request, you **MUST** strictly follow the Markdown template below in your PR description. Replace the placeholder texts but keep the layout and image exactly as shown:

```markdown
<p align="center">
  <picture><source media="(prefers-color-scheme: dark)" srcset="https://shieldcn.dev/header/glow.svg?title=Pull+Request&amp;subtitle=Thanks+for+participating+in+Linkdirecte%E2%80%99s+development%21&amp;logo=lu%3AGitPullRequestArrow&amp;mode=dark&amp;theme=blue&amp;align=left" /><img alt="Thanks for participating in Linkdirecte’s development!" src="https://shieldcn.dev/header/glow.svg?title=Pull+Request&amp;subtitle=Thanks+for+participating+in+Linkdirecte%E2%80%99s+development%21&amp;logo=lu%3AGitPullRequestArrow&amp;mode=light&amp;theme=blue&amp;align=left" /></picture>
</p>

## 🌐 Overview

A quick description of the changes.

## 📋 Changelog

* A clear list of changes
  * **Must** cover every change.
* Another change

## ℹ️ Other info _(optional)_

Potential link to related issue, or any other info you’d like to include.

---

_Pull Request posted by the AI agent [insert name here]_
```

## 2. Coding and Contribution Principles

- **Conventional Commits:** Always follow the conventional commits standard for commit messages (e.g., `feat: ...`, `fix: ...`, `docs: ...`).
- **Short Summary of Changes:** In your responses/replies to the user, always include a short summary of changes and design choices if you edited the codebase.
- **Maintain Multi-Platform Support:** Keep the project available on Bun, Node.js, Deno, and browsers.
- **Update Documentation:** Always update the docs when you make changes.
- **Strict Package Version Rule:** **NEVER** touch the package version number in `package.json`. Only project admins can update it.
- **Bun Over Others:** Always use `Bun` instead of npm, yarn, pnpm, or other package managers.
- **Pre-commit Checks:** Before committing, proposing, or submitting any changes, you must always run `bun precommit` and ensure all tests and type-checks pass.
