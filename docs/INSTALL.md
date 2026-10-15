# Installation Guide

This guide takes you from nothing to a working `/pr-writer` command. It covers Ubuntu/Linux, macOS and Windows.

You need three things:

1. **Git** (to read your branch and diff)
2. **Claude Code** (runs the skill)
3. **GitHub CLI (`gh`)** (optional, only needed if you want the skill to open the PR for you)

Without `gh`, the skill still writes the description and saves it to `PR.md` for you to paste into GitHub.

---

## Step 1: Install Git

| OS | Command |
|---|---|
| Ubuntu / Debian | `sudo apt update && sudo apt install git -y` |
| macOS | `brew install git` (or run `git --version` and accept the Xcode tools prompt) |
| Windows | `winget install --id Git.Git -e` |

Check: `git --version`

## Step 2: Install Claude Code

Follow the official instructions at <https://docs.claude.com/en/docs/claude-code/setup>. Common options:

| OS | Command |
|---|---|
| macOS / Linux / WSL | `curl -fsSL https://claude.ai/install.sh \| bash` |
| Windows (PowerShell) | `irm https://claude.ai/install.ps1 \| iex` |
| Any (needs Node.js 18+) | `npm install -g @anthropic-ai/claude-code` |

Then run `claude` once in a terminal and sign in.

Check: `claude --version`

## Step 3: Install the skill

A skill is a folder containing a `SKILL.md` file. Claude Code loads skills from a `skills` folder.

Pick one location:

| Where | Path | Available in |
|---|---|---|
| Personal | `~/.claude/skills/pr-writer/` | every project |
| Project | `<your-repo>/.claude/skills/pr-writer/` | that repo only (commit it to share with your team) |

### Ubuntu / Linux / macOS

```sh
git clone https://github.com/haiderfiaz05/pr-writer.git ~/.claude/skills/pr-writer
```

### Windows (PowerShell)

```powershell
git clone https://github.com/haiderfiaz05/pr-writer.git "$env:USERPROFILE\.claude\skills\pr-writer"
```

### No git? Manual copy

1. Download `SKILL.md` from this repo.
2. Create the folder `~/.claude/skills/pr-writer/` (Windows: `%USERPROFILE%\.claude\skills\pr-writer\`).
3. Put `SKILL.md` inside it.

### Update later

```sh
git -C ~/.claude/skills/pr-writer pull
```

Restart Claude Code after installing or updating so it picks up the skill.

## Step 4: Install GitHub CLI (optional)

### Ubuntu / Debian

```sh
(type -p wget >/dev/null || (sudo apt update && sudo apt-get install wget -y)) \
&& sudo mkdir -p -m 755 /etc/apt/keyrings \
&& out=$(mktemp) && wget -nv -O$out https://cli.github.com/packages/githubcli-archive-keyring.gpg \
&& cat $out | sudo tee /etc/apt/keyrings/githubcli-archive-keyring.gpg > /dev/null \
&& sudo chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
&& sudo mkdir -p -m 755 /etc/apt/sources.list.d \
&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
&& sudo apt update \
&& sudo apt install gh -y
```

Other Linux distros: see <https://github.com/cli/cli/blob/trunk/docs/install_linux.md>.

### macOS

```sh
brew install gh
```

No Homebrew? Use `sudo port install gh` (MacPorts) or download the installer from <https://github.com/cli/cli/releases/latest>.

### Windows

```powershell
winget install --id GitHub.cli
```

Other options: `choco install gh` (Chocolatey), `scoop install gh` (Scoop), or the `.msi` from <https://github.com/cli/cli/releases/latest>. Open a **new** terminal afterwards so `gh` is on your PATH.

### Log in to GitHub

```sh
gh auth login
```

Choose **GitHub.com**, your preferred protocol (HTTPS or SSH), then follow the browser prompts.

Check: `gh auth status` should say you are logged in.

## Step 5: Verify

1. Open a terminal in any git repo that has a feature branch.
2. Run `claude`.
3. Type `/pr-writer`. The skill should tell you the current branch and ask for a base branch.

If `/pr-writer` is not listed, see Troubleshooting.

## Troubleshooting

| Problem | Fix |
|---|---|
| `/pr-writer` not found | Check the file is at `.../skills/pr-writer/SKILL.md` (not nested one level deeper). Restart Claude Code. |
| `gh: command not found` | Open a new terminal. On Windows, restart PowerShell. Re-run the install step. |
| `gh auth status` says not logged in | Run `gh auth login`. |
| PR was not created | The skill falls back to `PR.md`. Paste its contents into GitHub, or fix `gh` and ask again. |
| `fatal: ambiguous argument 'main..HEAD'` | The base branch does not exist locally. Run `git fetch` and pick a base that exists (the skill also tries `origin/<base>`). |
| Permission denied (publickey) when cloning over SSH | Use the HTTPS clone URL, or add an SSH key to your GitHub account. |

## Uninstall

```sh
rm -rf ~/.claude/skills/pr-writer
```

Windows: `Remove-Item -Recurse -Force "$env:USERPROFILE\.claude\skills\pr-writer"`
