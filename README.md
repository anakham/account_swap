# GitHub Account Switcher

Scripts for switching between multiple GitHub accounts (`anakham` and `anakham-ai`) for both `gh` CLI and `git`.

## Quick Start

### 1. Initial Setup (run once)

```bash
# Authenticate both accounts with gh
./setup-gh-accounts

# OR manually:
gh auth login  # Login as anakham
gh auth login  # Login as anakham-ai
gh auth setup-git
```

### 2. Configure Per-Directory Identity (optional, automatic switching)

```bash
./setup-git-includes
# Enter your project directories when prompted
```

### 3. Switch Accounts

```bash
./switch-gh-account anakham
./switch-gh-account anakham-ai
```

## Files

| File | Purpose |
|------|---------|
| `switch-gh-account` | Main script to switch between accounts |
| `setup-gh-accounts` | Initial gh authentication setup |
| `setup-git-includes` | Configure per-directory git identity |

## How It Works

### gh CLI
- Uses `gh auth switch --user <account>` to change the active account
- Tokens stored in system keychain (secure)
- `gh auth setup-git` makes gh handle git credentials automatically

### git (HTTPS)
- When `gh auth setup-git` is run, gh acts as credential helper
- `gh auth switch` automatically updates git credentials

### git (SSH)
- Script sets `core.sshCommand` to use different SSH keys per account
- Requires SSH keys in `~/.ssh/id_ed25519_anakham` and `~/.ssh/id_ed25519_anakham-ai`

### git user.name/email
- Global config updated on switch
- Optional: Per-directory config via `includeIf` in `~/.gitconfig`

## SSH Key Setup (if using SSH remotes)

```bash
# Generate keys for each account
ssh-keygen -t ed25519 -C "anakham@github.com" -f ~/.ssh/id_ed25519_anakham
ssh-keygen -t ed25519 -C "anakham-ai@github.com" -f ~/.ssh/id_ed25519_anakham-ai

# Add public keys to GitHub:
# https://github.com/settings/keys
```

## Usage

```bash
# Check current status
gh auth status
git config user.name

# Switch to anakham
./switch-gh-account anakham

# Switch to anakham-ai
./switch-gh-account anakham-ai
```

## Troubleshooting

```bash
# Check which account gh is using
gh auth status

# Check git identity
git config user.name
git config user.email

# Re-run setup-git if git credentials aren't updating
gh auth setup-git

# For SSH issues, verify key is being used
git config core.sshCommand
```