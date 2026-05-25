# GitHub Account Switcher

Scripts for switching between multiple GitHub accounts for both `gh` CLI and `git`.

## Quick Start

### 1. Setup each account (run once per account)

```bash
./setup-gh-accounts <login_name>
```

The setup script will:
- Authenticate with gh via browser
- Optionally configure SSH key (existing or generate new)
- Store SSH key path in `~/.gh-accounts` config file

### 2. Switch Accounts

```bash
./switch-gh-account <login_name>
```

## Files

| File | Purpose |
|------|---------|
| `switch-gh-account` | Main script to switch between accounts |
| `setup-gh-accounts` | Setup a new account with gh and optional SSH key |

## Config File

SSH key paths are stored in `~/.gh-accounts`:

```
login-1_ssh_key=/Users/user/.ssh/id_ed25519_login-1
login-2_ssh_key=/Users/user/.ssh/id_ed25519_login-2
```

## How It Works

### gh CLI
- Uses `gh auth switch --user <account>` to change the active account
- Tokens stored in system keychain (secure)
- `gh auth setup-git` makes gh handle git credentials automatically

### git (HTTPS)
- When `gh auth setup-git` is run, gh acts as credential helper
- `gh auth switch` automatically updates git credentials

### git (SSH)
- Script reads SSH key path from `~/.gh-accounts` config
- Falls back to `~/.ssh/id_ed25519_<username>` if not in config
- Sets `core.sshCommand` for current repository

### git user.name/email
- Set manually per-repository as needed

## Usage

```bash
# Check current status
gh auth status
git config user.name

# Switch to account
./switch-gh-account <login_name>
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

# View stored SSH key config
cat ~/.gh-accounts
```
