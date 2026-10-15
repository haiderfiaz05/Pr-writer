---
name: pr-writer
description: Write a complete pull request description for the current branch by comparing it with a base branch (auto-detected, asked only if unclear). Follows the repo's PR template if one exists. Output is GitHub-flavored Markdown (callouts, tables, collapsible sections, mermaid, diffs). Covers summary, bugs fixed, new features, key functions, breaking changes, file changes, reviewer notes and test plan. Can create the PR with gh after confirmation. Use for "write a PR", "pull request description", "PR summary", /pr-writer.
---

# PR Writer

Write the best pull request description for the current branch. Never push, open a PR, or change code without the user's confirmation (step 5).

## Arguments

Free text after `/pr-writer` is ticket or context. Recognised flags:

| Flag | Effect |
|---|---|
| `base <branch>` | Use this base, skip detection |
| `--draft` | Create the PR as a draft |
| `--no-create` | Only write `PR.md`, never push or create |
| `--short` | Short body (see `template.md`) |
| `--update` | Update the existing PR for this branch with `gh pr edit` instead of creating one |
| `--lang <language>` | Write the PR in that language (code, paths and flags stay as is) |
| `--run-tests` | Run the project's test command and report the real result |

## 1. Confirm the branch and base

1. Run `git branch --show-current`. This is the PR branch. Never reuse a branch from earlier in the conversation or an old `PR.md`. Tell the user the branch in one line.
2. Find the base, first match wins:
   - the `base` argument
   - `gh pr view --json baseRefName -q .baseRefName` (an open PR already exists)
   - `git symbolic-ref --short refs/remotes/origin/HEAD` or `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`
3. Ask the user (AskUserQuestion) only if no base was found, or the branch has no commits ahead of the detected base, or the detected base looks wrong (for example `main` exists but the branch was clearly cut from `dev`: check `git merge-base` against each). Offer `main` and `dev`. Otherwise state the detected base and continue.

Use `origin/<base>` if the local base branch is missing. Run `git fetch origin <base>` first if it is stale and the network allows.

## 2. Gather facts

- `git log --reverse --format='%h %s%n%b' <base>..HEAD`
- `git diff --stat <base>...HEAD`
- `git diff <base>...HEAD -- . ':(exclude)*.lock' ':(exclude)package-lock.json' ':(exclude)pnpm-lock.yaml' ':(exclude)yarn.lock' ':(exclude)*.min.*' ':(exclude)*.snap' ':(exclude)dist/' ':(exclude)build/' ':(exclude)*.map'` (three dots: only this branch's changes). Also exclude any generated code the repo marks (for example `linguist-generated` in `.gitattributes`). Name the excluded files in one line under File changes.
- `git status --short` (uncommitted work is mentioned separately; it is not in the PR)

For a large diff, read the biggest and most important files in full, not just hunks. Check callers of any changed function or signature.

**Repo conventions:**
- PR template: `.github/pull_request_template.md`, `.github/PULL_REQUEST_TEMPLATE/*`, `docs/pull_request_template.md`. If found, use its sections and headings instead of `template.md`.
- Title style: look at `git log -n 20 --format=%s <base>`. If most subjects follow Conventional Commits (`feat:`, `fix(scope):`), use that style for the title. Otherwise use plain imperative.
- Ticket: look for an ID in the branch name or commits (`PROJ-123`, `#123`, `gh-123`). Put it in Related. Use `Closes #N` only for GitHub issue numbers found in commits or the branch, never guessed.
- Reviewers: if `CODEOWNERS` exists (`.github/`, root or `docs/`), match changed paths to owners.

## 3. Classify every change

- **Bug fixes:** what was wrong, the cause, how it is fixed. Only list a bug if the diff or a commit message shows it. Never invent one.
- **New features:** what the user can now do. Name the screen or entry point. If the diff touches UI files (screens, components, CSS), add a screenshots placeholder and a "how to see it" step.
- **Refactors / cleanup:** renames, deletions, moved code.
- **Key methods and components:** new or changed functions, hooks, components, endpoints, store slices. One line each.
- **Dependencies, config, native or migration changes.**

**Breaking-change check.** Look specifically for, and list under Breaking changes with a WARNING callout:
- changed or removed public function signatures, exported types, API routes or CLI flags (grep callers)
- DB migrations or schema changes
- new or renamed environment variables and config keys
- removed files others may import
- dependency major-version bumps or removed dependencies

**Safety scan on the diff.** Flag under Notes: leftover `console.log`/`print` debugging, `TODO`/`FIXME`, commented-out code, and anything that looks like a secret (API keys, tokens, private keys, `.env` values, long random strings next to words like key/secret/token/password). For a possible secret, use a CAUTION callout, show only the file and line (never the value), and tell the user to rotate it if it was pushed.

**Tests.** Note whether test files changed. If changed code has no test changes, say so under Notes. Only with `--run-tests`: find the test command (package.json scripts, Makefile, pyproject, etc.), run it, and report the real pass/fail output. Never tick a "tests pass" box unless you ran them in this session.

## 4. Write

Use the structure in `template.md` (next to this file), or the repo's own template. Output a **Title** (imperative, under 70 characters), then the body in a single fenced ```markdown block so it copies cleanly.

Rules:
- Every claim must come from the diff or commits. If unsure of intent, say "appears to" or leave it out.
- Plain words, short sentences. Explain a term the first time.
- Skip empty sections. Do not pad.
- Mention commit count and diff size (files, +/-) in one line.
- Never add a "Generated with Claude Code" or similar attribution line to the PR title or body.

Save the title (first `# ` line) and body to `PR.md` in the repo root.

## 5. Create the PR (only with confirmation)

Skip this whole step with `--no-create`, or if `gh` is missing or not logged in (`gh auth status`): then tell the user to paste `PR.md` into GitHub.

Otherwise, show what you are about to do and ask the user to confirm (AskUserQuestion): push the branch (if it has no upstream), then create the PR as draft or ready. Default the choice to draft when `--draft` was given.

On yes:
- Push if there is no upstream: `git push -u origin <branch>`
- Create: `gh pr create --base <base> --title "<title>" --body-file <body without the title line>` (add `--draft` if chosen; add `--reviewer`/`--label` only for names the user approved)
- With `--update`: `gh pr edit --title "<title>" --body-file <body file>` on the existing PR instead.
- Print the PR URL.

Report only what actually happened. If a command failed, show the error and leave `PR.md` for manual use.
