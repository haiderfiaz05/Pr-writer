# Changelog

## 1.1.0

- Ask before pushing or creating a PR; add `--draft`, `--no-create`, `--update`
- Auto-detect the base branch; ask only when unclear
- Follow the repo's PR template if one exists
- Exclude lockfiles, generated and minified files from the diff
- Pick up ticket IDs and `Closes #N` from branch and commits; match title style to repo history
- Breaking-change check, secret scan, test detection and optional `--run-tests`
- Suggest reviewers from `CODEOWNERS`
- Add `--short` and `--lang`; move the body template to `template.md`

## 1.0.0

- Initial release
