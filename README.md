# pr-writer: AI Pull Request Description Generator for Claude Code

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Claude Code Skill](https://img.shields.io/badge/Claude%20Code-skill-blueviolet)](https://claude.com/claude-code)
[![GitHub CLI](https://img.shields.io/badge/works%20with-GitHub%20CLI-black)](https://cli.github.com)

**pr-writer** is a free, open-source [Claude Code](https://claude.com/claude-code) skill (agent skill) that **automatically writes pull request descriptions** from your git branch. Run `/pr-writer`, pick a base branch, and get a complete PR title and body in GitHub-flavored Markdown. If the GitHub CLI (`gh`) is installed, it can also open the pull request for you.

Use it to generate PR descriptions with AI, write better PR summaries, speed up code review, and stop writing one-line PRs.

## Features

- **Generates a full PR description** from commits and `git diff`: summary, bug fixes, new features, key functions, file changes, notable diffs, reviewer notes and test plan
- **Advanced GitHub Markdown**: callouts (`[!NOTE]`, `[!WARNING]`), tables, collapsible `<details>` sections, task lists, `diff` blocks and mermaid diagrams
- **Detects the base branch** automatically and asks only when unclear
- **Uses your repo's PR template** if one exists, and matches your title style and ticket IDs
- **Evidence-based**: every claim comes from the diff or commit messages, with no invented bugs
- **Flags review risks**: breaking changes, leftover `console.log`, `TODO`, commented-out code, possible secrets, missing tests
- **Skips noise**: lockfiles, generated and minified files stay out of the diff
- **Flags**: `--draft`, `--short`, `--update`, `--no-create`, `--run-tests`, `--lang`
- **Creates the PR** with `gh pr create` after you confirm, or saves `PR.md` to paste into GitHub
- Works on **Ubuntu/Linux, macOS and Windows**

## Quick install

```sh
git clone https://github.com/haiderfiaz05/Pr-writer.git ~/.claude/skills/pr-writer
```

Restart Claude Code, open any git repo on a feature branch, and run:

```
/pr-writer
```

Full steps for Windows, macOS and Ubuntu, including installing Git, Claude Code and the GitHub CLI, are in the [Installation Guide](docs/INSTALL.md).

## How to write a pull request description with Claude Code

1. Commit your work on a feature branch.
2. Run `/pr-writer` (or ask: "write a PR description for this branch").
3. Confirm the detected base branch (or choose one if it asks).
4. Review the generated title and body, saved as `PR.md`.
5. Confirm, and the skill opens the PR with `gh`; or paste `PR.md` into GitHub.

## Example output

See a full example in [`examples/payment-retry.md`](examples/payment-retry.md). Short sample:

```markdown
> [!NOTE]
> Adds retry with backoff to the payment API client so transient failures no longer fail checkout.

| Branch | Base | Commits | Files | Lines |
|---|---|---|---|---|
| `fix/payment-retry` | `main` | 4 | 6 | +142 / -31 |

## 🐛 Bug fixes
| Bug | Cause | Fix | File |
|---|---|---|---|
| Checkout failed on timeout | No retry on 503 | Retry up to 3 times with backoff | `src/api/payments.ts` |
```

(Shortened and illustrative. The linked example is from a real run.)

## Documentation

| Guide | What is in it |
|---|---|
| [Installation Guide](docs/INSTALL.md) | Install Git, Claude Code, this skill and GitHub CLI on Ubuntu, macOS and Windows; troubleshooting |
| [What pr-writer does](docs/GUIDE.md) | Workflow diagram, flags, output format, usage, what it reads and writes, limits |
| [Example](examples/payment-retry.md) | A full generated PR |
| [Changelog](CHANGELOG.md) · [Contributing](CONTRIBUTING.md) | Version history and how to help |

## FAQ

**What is a Claude Code skill?**
A folder with a `SKILL.md` file that teaches Claude Code a repeatable task. Claude Code loads it from `~/.claude/skills/` and exposes it as a slash command.

**How do I generate a pull request description with AI?**
Install this skill, then run `/pr-writer` on your branch.

**Does it work without the GitHub CLI?**
Yes. It writes the description to `PR.md`. The GitHub CLI is only needed for automatic PR creation.

**Does it work with GitLab or Bitbucket?**
The description is plain Markdown and can be pasted anywhere, but automatic PR creation uses `gh` and supports GitHub only. Some GitHub-only syntax, such as callouts, may not render elsewhere.

**Does it send my code anywhere?**
It runs inside Claude Code, so your diff is processed under your Claude Code account's normal data terms. The skill adds no other service.

**Will it push or open a PR without asking?**
No. It asks first, and `--no-create` never pushes.

**Does it add a "Generated with Claude Code" line?**
No. It never adds attribution to the PR title or body.

## Keywords

AI pull request generator, PR description generator, automatic PR summary, Claude Code skill, Claude Code slash command, agent skill, GitHub pull request template, code review automation, git diff summary, GitHub CLI `gh pr create`.

## Contributing

Issues and pull requests are welcome. To change the skill's behavior, edit [`SKILL.md`](SKILL.md).

## License

[MIT](LICENSE)
