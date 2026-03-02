# GitHub Maintainer

Automated GitHub repository maintenance with AI-powered decision making.

## Features

- Pulls latest changes from default branch
- Merges dependabot PRs after AI verification
- Updates direct dependencies with configurable age constraints
- Runs tests before committing
- Commits and pushes changes if tests pass
- Dry-run mode for safe testing

## Installation

```bash
git clone https://github.com/andornaut/github-maintainer.git
cd github-maintainer
chmod +x github-maintainer

# Install to ~/.local/bin (no sudo required)
mkdir -p ~/.local/bin
ln -s "$(pwd)/github-maintainer" ~/.local/bin/github-maintainer

# Ensure ~/.local/bin is in your PATH (add to ~/.bashrc or ~/.zshrc if needed)
export PATH="$HOME/.local/bin:$PATH"
```

**Dependencies**:

- Python 3.7+
- Git
- GitHub CLI (`gh`) - authentication only required for private repos
- AI agent: [Claude Code CLI](https://docs.anthropic.com/claude/docs) (default) or [Ollama](https://ollama.ai/)

## Usage

```bash
github-maintainer                                    # Current directory
github-maintainer --base-dir ~/src/github.com        # Specify directory
github-maintainer --dry-run --verbose                # Preview changes
github-maintainer --limit 5                          # Only update 5 repos with changes
github-maintainer --agent-command "ollama run llama3" # Use local LLM
github-maintainer --dependency-age-threshold 60      # Conservative (60 days)
```

**Feature toggles**:

- `--no-merge-dependabot` - Skip merging PRs
- `--no-update-dependencies` - Skip dependency updates
- `--no-run-tests` - Skip tests
- `--no-push` - Don't push to remote

## How It Works

For each repository in the specified directory:

1. Validates it's on the default branch with a clean working directory
2. Pulls latest changes
3. Asks AI to verify open dependabot PRs, then merges approved ones locally
4. Asks AI which direct dependencies to update (respects age threshold)
5. Runs tests only if changes were made
6. Commits and pushes if tests pass

## Automation

**Cron** (daily at 3 AM):

```bash
0 3 * * * $HOME/.local/bin/github-maintainer --base-dir ~/src/github.com 2>&1
```

## Security

Runs with `--dangerously-skip-permissions` for autonomous operation. Start with `--dry-run` to verify behavior.

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
```

## License

MIT
