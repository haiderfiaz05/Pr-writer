# pr-writer

A [Claude Code](https://claude.com/claude-code) skill that writes a complete pull request description for your current branch, and can open the PR for you.

It asks which base branch to compare against, reads the commits and diff, and produces GitHub-flavored Markdown: summary, bug fixes, new features, key methods, file changes, notable diffs, reviewer notes and a test plan.

## Quick install

```sh
git clone https://github.com/haiderfiaz05/pr-writer.git ~/.claude/skills/pr-writer
```

Restart Claude Code, then run `/pr-writer` inside any git repo.

## Docs

- [Installation guide](docs/INSTALL.md): Git, Claude Code, the skill and GitHub CLI on Ubuntu, macOS and Windows
- [What it does](docs/GUIDE.md): the workflow, output format, usage and limits

## License

MIT
