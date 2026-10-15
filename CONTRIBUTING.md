# Contributing

Thanks for helping improve pr-writer.

- The skill's behavior lives in [`SKILL.md`](SKILL.md); the output layout lives in [`template.md`](template.md).
- Keep instructions short and concrete. The skill must never claim something it did not verify (bugs, passing tests).
- To test a change, copy the folder to `~/.claude/skills/pr-writer`, restart Claude Code, run `/pr-writer` on a real branch, and compare with the examples in [`examples/`](examples).
- If you add a flag, document it in the table in `SKILL.md`, in `docs/GUIDE.md`, and in `CHANGELOG.md`.
- Open an issue before large changes.
