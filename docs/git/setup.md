# Git Setup and Best Practices

This guide covers the essential setup and configuration for Git on macOS.

## Installation

### Install Git on macOS

Using Homebrew (recommended):
```bash
brew install git
```

Verify installation:
```bash
git --version
```

## Initial Configuration

This guide assumes you're working under multiple companies, names, or aliases. It's recommended to set your name and email locally for each repository after cloning it.

### Basic Configuration

To set your name and email for a specific repository, navigate to the repository directory and run:

```bash
# Set your name and email locally for the current repository
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

If you prefer to set a global name and email (optional), you can use:

```bash
# Set your name and email globally (optional)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Essential Git Configurations

```bash
# Ignore macOS specific files in global `.gitignore` file.
curl -o ~/.gitignore https://raw.githubusercontent.com/github/gitignore/main/Global/macOS.gitignore

# Configure Git to automatically push to the origin
git config --global push.default simple

# Configure pull strategy
git config --global pull.rebase true
```

### Other optional configurations:

```bash
# Set default editor - VS Code Insiders (optional)
git config --global core.editor "code-insiders --wait"

# Enable colorized output (optional)
git config --global color.ui true

# Set up credential helper for macOS (optional)
git config --global credential.helper osxkeychain

# Set default branch name to main
git config --global init.defaultBranch main
```

## SSH Key Setup

It's recommended to use unique SSH keys for each organization or account to maintain security and privacy.

Generate SSH key for every new organization or account you use:
```bash
# Replace "ORG" with the name of your organization
ssh-keygen -t ed25519 -C "your.email@example.com" -f ~/.ssh/id_ed25519_ORG

# Some services like DevOps don't support eliptic curves, so use RSA instead
# Replace "ORG" with the name of your organization
ssh-keygen -t rsa -C "your.email@example.com" -f ~/.ssh/id_rsa_ORG
```

Create or update the SSH configuration file to include the new keys:
```bash
# Open or create the SSH config file
nano ~/.ssh/config

# Add the following configuration for each organization/account
# Replace "ORG" with the name of your organization
# It's recommnded to use first path segment after service name and add it before service domain
# Example: github.com/ORG/...
Host ORG.github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_ORG
```

Copy public keys to clipboard:
```bash
pbcopy < ~/.ssh/id_ed25519_ORG.pub
```

Add the keys to your GitHub account:
   - Go to GitHub Settings → SSH and GPG keys
   - Click "New SSH key"
   - Paste your key and save

Use the appropriate SSH key when cloning repositories:
```bash
# Replace ORG with organization or user name
git clone git@ORG.github.com:ORG/repo.git
```

## Useful Git Aliases

Add these to your global Git config:

```bash
git config --global alias.st "status"
git config --global alias.co "checkout"
git config --global alias.br "branch"
git config --global alias.ci "commit"
git config --global alias.lg "log --oneline --graph --decorate"
```

