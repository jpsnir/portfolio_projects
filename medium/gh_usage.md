# GitHub CLI (`gh`) Usage Guide

The GitHub CLI (`gh`) is a command-line tool that brings GitHub to your terminal. It's a powerful way to interact with GitHub without leaving your command-line environment. This guide covers some of the most important commands to get you started.

## Installation

You can install `gh` using package managers on most operating systems.

**macOS:**
```bash
brew install gh
```

**Ubuntu/Debian:**
```bash
sudo apt install gh
```

**Windows:**
```bash
winget install --id GitHub.cli
```

## Authentication

Before you can use `gh`, you need to authenticate with your GitHub account.

```bash
gh auth login
```

This command will walk you through the authentication process in your browser.

## Core Commands

### `gh repo`

Manage your GitHub repositories.

*   **Clone a repository:**
    ```bash
    gh repo clone <repository>
    ```
    Example: `gh repo clone cli/cli`

*   **Create a new repository:**
    ```bash
    gh repo create <repository-name> --public --source=. --remote=upstream
    ```

*   **List repositories:**
    ```bash
    gh repo list
    ```

### `gh pr`

Manage your pull requests.

*   **Create a pull request:**
    ```bash
    gh pr create
    ```
    This command will interactively guide you through creating a pull request.

*   **List open pull requests:**
    ```bash
    gh pr list
    ```

*   **Checkout a pull request:**
    ```bash
    gh pr checkout <pr-number>
    ```

*   **Merge a pull request:**
    ```bash
    gh pr merge <pr-number> --squash --delete-branch
    ```

### `gh issue`

Manage your issues.

*   **Create an issue:**
    ```bash
    gh issue create
    ```

*   **List issues:**
    ```bash
    gh issue list
    ```

*   **View an issue:**
    ```bash
    gh issue view <issue-number>
    ```

### `gh alias`

Create custom commands.

*   **Set an alias:**
    ```bash
    gh alias set <alias-name> '<command>'
    ```
    Example: `gh alias set prs "pr list"`

*   **List aliases:**
    ```bash
    gh alias list
    ```

## Conclusion

This is just a brief overview of the `gh` command. For a complete list of commands and options, you can always use the `--help` flag or refer to the official documentation.
