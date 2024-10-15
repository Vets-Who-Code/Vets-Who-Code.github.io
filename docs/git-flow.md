# Git Flow Overview

## Understanding Git Flow

### Introduction

This guide will help you understand Git Flow, an advanced workflow that simplifies the management of complex projects. Git Flow introduces a branching model designed to keep your development process organized, from feature development to handling hotfixes and releases.

### Learning Outcomes

By the end of this guide, you will:

- Understand what Git Flow is and how it improves project workflow.
- Implement a feature using Git Flow.
- Manage hotfixes and releases effectively.
- Use Git tags for version control and learn versioning best practices.

## What is Git Flow? (UI & Terminal)

### Definition

Git Flow is a branching model that helps teams manage project development in a structured way. It ensures that work on new features, bug fixes, and releases is done in isolation and merged back into the main codebase when ready.

### Key Components

- **Main Branch**: The production-ready branch, always reflecting the latest release.
- **Develop Branch**: The integration branch where features are merged and tested before release.
- **Feature Branches**: Temporary branches for developing new features or functionalities.
- **Release Branches**: Used to prepare new releases, allowing final fixes and tests.
- **Hotfix Branches**: Created for critical patches in the production branch.

## Implementing a Feature using Git Flow (Terminal)

### Workflow for Features

Git Flow makes it easy to develop new features in isolation before integrating them into the main codebase.

1. Checkout the develop branch:

    ```bash
    git checkout develop
    ```

2. Create a new feature branch:

    ```bash
    git checkout -b feature/new-feature
    ```

3. Develop the feature:
   Add your code and commit regularly:

    ```bash
    git add .
    git commit -m "Start implementing new feature"
    ```

4. Merge the feature back into develop:
   Once the feature is complete:

    ```bash
    git checkout develop
    git merge feature/new-feature
    ```

5. Delete the feature branch (optional):
   Clean up by deleting the feature branch:

    ```bash
    git branch -d feature/new-feature
    ```

## Hotfixes and Releases (Terminal)

### Hotfix Workflow

Hotfixes are quick patches applied directly to the production code. Git Flow makes it easy to apply fixes and keep both the main and develop branches updated.

1. Create a hotfix branch from main:

    ```bash
    git checkout -b hotfix/urgent-fix main
    ```

2. Apply the fix and commit it:

    ```bash
    git add .
    git commit -m "Apply urgent fix for production"
    ```

3. Merge the fix back into main and develop:

    ```bash
    git checkout main
    git merge hotfix/urgent-fix
    git checkout develop
    git merge hotfix/urgent-fix
    ```

### Release Workflow

Releases prepare the develop branch for production. Final bug fixes, versioning, and testing occur on the release branch before merging into main.

1. Create a release branch from develop:

    ```bash
    git checkout -b release/v1.0 develop
    ```

2. Perform final changes and commit:

    ```bash
    git add .
    git commit -m "Prepare for release v1.0"
    ```

3. Merge the release into main and tag the release:

    ```bash
    git checkout main
    git merge release/v1.0
    git tag -a v1.0 -m "Release version 1.0"
    ```

4. Merge the release back into develop (to ensure any last-minute fixes are included):

    ```bash
    git checkout develop
    git merge main
    ```

## Git Tags and Versioning (Terminal)

### Using Git Tags

Tags allow you to mark important points in your project's history, such as a new release. Tags can be lightweight (simple references) or annotated (contain metadata).

1. Create an annotated tag for a release:

    ```bash
    git tag -a v1.0 -m "Release version 1.0"
    ```

2. Push the tags to your remote repository:

    ```bash
    git push origin --tags
    ```

### Versioning Practices

When tagging releases, many developers follow semantic versioning, which uses the format MAJOR.MINOR.PATCH. For example:

- MAJOR: Increment for incompatible changes (e.g., v2.0.0).
- MINOR: Increment for new backward-compatible functionality (e.g., v1.1.0).
- PATCH: Increment for backward-compatible bug fixes (e.g., v1.0.1).

## Summary

With Git Flow, you've learned how to manage features, releases, and hotfixes in a structured, organized way. You also gained an understanding of tagging releases and versioning best practices to keep your projects clean and maintainable. Keep practicing these workflows, and you'll streamline your project management processes significantly!