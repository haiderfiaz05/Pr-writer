---
name: pr-writer
description: Write a complete pull request description for the current branch by comparing it with a base branch (always ask the user which one first). Output is advanced GitHub-flavored Markdown (callouts, tables, collapsible sections, mermaid, diffs). Covers summary, bugs fixed, new features, key methods/functions, file changes, notes and test plan. Use for "write a PR", "pull request description", "PR summary", /pr-writer.
---

# PR Writer

Write the best pull request description for the current branch. Report only; do not push, open a PR, or change code unless the user asks.

Arguments: optional ticket or context text. Do not assume a base branch.

## 0. Confirm the branch, then ask the base

1. Run `git branch --show-current` first. This is the branch the PR is for. Never reuse a branch name from earlier in the conversation or from an old `PR.md`; the user may have switched branches.
2. Tell the user the current branch in one line.
3. Then ask which base branch to compare against (use AskUserQuestion; offer `main` and `dev`). Wait for the answer. Skip the question only if the user already named a base in the arguments.

## 1. Gather facts

Run these (use `origin/<base>` if the local base branch is missing):

- `git log --reverse --format='%h %s%n%b' <base>..HEAD`
- `git diff --stat <base>...HEAD`
- `git diff <base>...HEAD` (three dots: only this branch's changes)
- `git status --short` (mention uncommitted work separately; it is not in the PR)

For a large diff, read the biggest and most important files in full, not just the hunks. Check callers of any changed function or signature.

## 2. Classify every change

- **Bug fixes:** what was wrong, the cause, how it is fixed. Only list a bug if the diff or a commit message shows it. Never invent one.
- **New features:** what the user can now do. Name the screen or entry point.
- **Refactors / cleanup:** renames, deletions, moved code.
- **Key methods and components:** new or changed functions, hooks, components, endpoints, store slices. One line each: name, file, what it does.
- **Dependencies, config, native or migration changes.**

## 3. Output

Use advanced GitHub-flavored Markdown (GFM). It must render well on GitHub. Give a **Title** (imperative, under 70 characters), then the body in a single fenced ```markdown block so it copies cleanly.

Body template:

````markdown
> [!NOTE]
> One-line TL;DR: what this PR does and why.

| Branch | Base | Commits | Files | Lines |
|---|---|---|---|---|
| `feature` | `dev` | N | N | +A / -D |

## 📝 Summary
2-4 sentences.

## 🐛 Bug fixes
| Bug | Cause | Fix | File |
|---|---|---|---|

## ✨ New features
- **Feature**: what the user sees, and where (screen / entry point).

## 🧩 Key methods & components
| Name | Type | File | Purpose |
|---|---|---|---|
| `fn` | hook / util / component / slice | `path` | ... |

## 🔄 Flow
Add a ```mermaid diagram (flowchart or sequenceDiagram) ONLY when it explains a real flow (navigation, data fetch, state). Skip it otherwise.

## 📂 File changes
<details>
<summary><b>Screens</b> (N files)</summary>

| File | Change | +/- |
|---|---|---|

</details>

(one <details> block per area: screens, navigation, components, utils, store, tests, config)

## 🔍 Notable diffs
Show 1-3 key before/after snippets in ```diff blocks. Only the important ones.

## ⚠️ Breaking changes / migration
> [!WARNING]
> Use a WARNING callout if action is needed. Write "None" if none.

## 🧠 Notes for reviewers
> [!IMPORTANT]
> Risky areas, tradeoffs, known limits, things left out.

## ✅ Test plan
- [ ] Concrete steps a reviewer can follow
- [x] Tests added or run, with result

## 🔗 Related
Closes #issue, links, screenshots placeholder (`<!-- add screenshots -->`).
````

Rules for the format:
- Use GFM callouts (`[!NOTE]`, `[!TIP]`, `[!IMPORTANT]`, `[!WARNING]`, `[!CAUTION]`) sparingly: one per purpose.
- Wrap long file lists in `<details>` so the PR stays scannable.
- Use tables for anything with 3+ attributes. Use task lists for the test plan.
- Skip any empty section entirely. Do not pad.
- Keep the whole body under about 65,000 characters (GitHub's limit).

## 4. Rules

- Every claim must come from the diff or commits. If unsure of intent, say "appears to" or leave it out.
- Plain words, short sentences. Explain a term the first time.
- Flag leftover `console.log`, `TODO`, commented-out code, secrets, or missing tests under Notes.
- Mention the commit count and diff size (files, +/-) in one line.
- Save the title and body to `PR.md` in the repo root (title as the first `# ` line). Then, if `gh` is installed and logged in (`gh auth status`), create the PR: push the branch if it has no upstream, and run `gh pr create --base <base> --title "<title>" --body-file <body without the title line>`. Never add any "Generated with Claude Code" or similar attribution line to the PR body or title. Print the PR URL. If `gh` is unavailable, tell the user to paste `PR.md` into GitHub.
