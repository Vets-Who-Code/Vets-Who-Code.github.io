# Setting up Git

## Introduction

This guide will walk you through the steps to properly set up Git on your machine. You will configure Git, generate SSH keys for secure authentication, set up your user identity, and, if necessary, configure a proxy for Git operations.

### Objectives

By the end of this guide, you will be able to:

- Configure Git using the terminal.
- Generate SSH keys for secure communication.
- Set and manage your Git user identity.
- Configure a proxy for Git operations (if required).

## Configuring Git

### Setting Up User Information

To personalize your Git environment, configure your username and email. This information will be attached to every commit you make.

Use the following commands:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Verifying Configuration

To check your current settings, use:

```bash
git config --list
```

!!! task
    Set your username and email, then verify the configuration using the above command.

## Generating SSH Keys

SSH keys provide secure access to Git services (like GitHub or GitLab), allowing you to interact with remote repositories without entering your password every time.

### Generating an SSH Key

Run the following command in your terminal:

```bash
ssh-keygen -t rsa -b 4096
```

Follow the prompts and press Enter to accept the default file location.

### Adding SSH Key to GitHub/GitLab

Once the key is generated:

1. Copy the public key to your clipboard:

    ```bash
    cat ~/.ssh/id_rsa.pub
    ```

2. Go to your GitHub or GitLab account settings.
3. Navigate to SSH and GPG keys and paste the key.

!!! task
    Generate an SSH key and add it to your GitHub or GitLab account.

## Proxy Configuration (Optional)

If you're working behind a corporate firewall or restricted network, you may need to configure Git to use a proxy.

### Setting Up a Proxy

To set up a proxy, run:

```bash
git config --global http.proxy http://proxy.example.com:8080
```

Replace `proxy.example.com:8080` with your actual proxy details.

### Verifying Proxy Configuration

To confirm the proxy is set up correctly:

```bash
git config --global http.proxy
```

!!! task
    If needed, configure your proxy settings and verify them using the above commands.

## Practical Exercise

Follow these steps to complete your setup:

1. Configure Git with your username and email.
2. Generate an SSH key and add it to your GitHub or GitLab account.
3. If necessary, set up a proxy.

Verify your configuration with:

```bash
git config --list
```

## Additional Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [GitHub SSH Key Setup Guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [Git Configuration Best Practices](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)

With these steps completed, Git is now properly configured on your machine, and you're ready to start working with version control. Happy coding!