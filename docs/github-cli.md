# GitHub CLI Overview

## Introduction to GitHub CLI

### Introduction

This guide covers the essential aspects of GitHub CLI (Command Line Interface), a powerful tool that allows you to interact with GitHub repositories directly from your terminal. Whether you're creating pull requests, managing issues, or navigating repositories, GitHub CLI helps streamline your workflow.

### Learning Outcomes

By the end of this guide, you will:

- Install GitHub CLI
- Execute basic GitHub CLI commands
- Create and manage aliases for frequently used operations
- Authenticate your GitHub account via the CLI

## Installing GitHub CLI (Terminal)

### Installation Steps

For macOS users:

```bash
brew install gh
```

For Ubuntu users:

```bash
sudo apt install gh
```

### Verification

To ensure the installation was successful, check the installed version:

```bash
gh --version
```

## Basic GitHub CLI Commands (Terminal)

### Key Commands

- View repository details:

```bash
gh repo view
```

- Check pull request status:

```bash
gh pr status
```

- List repository issues:

```bash
gh issue list
```

## Creating Aliases in GitHub CLI (Terminal)

Aliases let you create shortcuts for frequently used commands to save time. For example, to create an alias for viewing pull requests:

```bash
gh alias set pr-list 'pr list'
```

Now, instead of typing `gh pr list`, you can simply type `gh pr-list`.

## Authentication with GitHub CLI (Terminal)

To authenticate GitHub CLI with your GitHub account, use the following command:

```bash
gh auth login
```

Follow the on-screen prompts to complete the authentication process.

## Managing Repositories with GitHub CLI

### Learning Outcomes

- Create and clone repositories using GitHub CLI
- Fork repositories and create pull requests
- Manage issues within a repository
- Clean up repositories efficiently

### Creating and Cloning Repositories (Terminal)

To create a new repository:

```bash
gh repo create <repository-name>
```

To clone an existing repository:

```bash
gh repo clone <repository-name>
```

### Forking and Making Pull Requests (Terminal)

Fork a repository to your account:

```bash
gh repo fork <original-repo>
```

Create a pull request from your branch:

```bash
gh pr create
```

Follow the interactive prompts to provide a title, description, and reviewers for your pull request.

### Issue Management (Terminal)

To create a new issue:

```bash
gh issue create
```

List all open issues:

```bash
gh issue list
```

Close an issue by its number:

```bash
gh issue close <issue-number>
```

### Cleaning Up Repositories (Terminal)

To delete a repository from GitHub:

```bash
gh repo delete <repository-name>
```

## Navigating GitHub with GitHub CLI

### Learning Outcomes

- Manage issues and pull requests efficiently
- Work with GitHub Gists and Actions
- Navigate between repositories
- Use search filters to find specific repositories or issues

### Managing Issues and Pull Requests (Terminal)

To view the status of all open issues:

```bash
gh issue status
```

To check the status of pull requests:

```bash
gh pr status
```

### Working with Gists and Actions (Terminal)

To create a GitHub Gist from a file:

```bash
gh gist create <file-name>
```

To view the list of GitHub Actions workflows:

```bash
gh run list
```

### Navigating Repositories (Terminal)

To open the main page of a repository in your browser:

```bash
gh repo view --web
```

This command opens the repository's webpage, making it easier to review and interact with the project.

### Searching and Filtering (Terminal)

To search for repositories owned by a specific user or organization:

```bash
gh repo list <owner> --limit <n>
```

To filter issues based on specific labels:

```bash
gh issue list --label "bug"
```

By mastering GitHub CLI, you can significantly boost your productivity by performing GitHub operations directly from the terminal. Keep practicing these commands and use GitHub CLI as part of your development workflow for faster and more efficient project management!
