# PR body template

Fill this in. Skip any empty section entirely. Keep the body under about 65,000 characters (GitHub's limit). If the repo has its own PR template, use that instead and map the facts into it.

````markdown
> [!NOTE]
> One-line TL;DR: what this PR does and why.

| Branch | Base | Commits | Files | Lines |
|---|---|---|---|---|
| `feature` | `main` | N | N | +A / -D |

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
| `fn` | hook / util / component / endpoint | `path` | ... |

## 🔄 Flow
Add a mermaid diagram ONLY when it explains a real flow (navigation, data fetch, state). Skip otherwise.

## 📂 File changes
<details>
<summary><b>Area</b> (N files)</summary>

| File | Change | +/- |
|---|---|---|

</details>

(one <details> block per area; list excluded noise files in one line)

## 🔍 Notable diffs
1-3 key before/after snippets in ```diff blocks.

## ⚠️ Breaking changes / migration
> [!WARNING]
> Use a WARNING callout if action is needed. Write "None" if none.

## 🧠 Notes for reviewers
> [!IMPORTANT]
> Risky areas, tradeoffs, known limits, things left out.

## ✅ Test plan
- [ ] Concrete steps a reviewer can follow
- [x] Tests that were actually run, with result

## 👀 Suggested reviewers / labels
From CODEOWNERS and change types. Omit if unknown.

## 🔗 Related
Closes #issue, ticket link, screenshots (`<!-- add screenshots -->`).
````

Format rules:
- GFM callouts (`[!NOTE]`, `[!TIP]`, `[!IMPORTANT]`, `[!WARNING]`, `[!CAUTION]`) sparingly: one per purpose.
- `<details>` for long file lists. Tables for 3+ attributes. Task lists for the test plan.

## Short mode (`--short`)

Title plus at most: one summary sentence, 3-5 bullets of what changed, a breaking-change line if any, and the Closes line.
