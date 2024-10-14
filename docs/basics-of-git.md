Setting up Git

Introduction

This guide will walk you through the steps to properly set up Git on your machine. You will configure Git, generate SSH keys for secure authentication, set up your user identity, and, if necessary, configure a proxy for Git operations.

Objectives

By the end of this guide, you will:

	•	Configure Git using the terminal.
	•	Generate SSH keys for secure communication.
	•	Set and manage your Git user identity.
	•	Configure a proxy for Git operations (if required).

Section 1: Configuring Git

Setting Up User Information

To personalize your Git environment, configure your username and email. This information will be attached to every commit you make.

Use the following commands:

git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

Verifying Configuration

To check your current settings, use:

git config --list

Task: Set your username and email, then verify the configuration using the above command.

Section 2: Generating SSH Keys

SSH keys provide secure access to Git services (like GitHub or GitLab), allowing you to interact with remote repositories without entering your password every time.

Generating an SSH Key

Run the following command in your terminal:

ssh-keygen -t rsa -b 4096

Follow the prompts and press Enter to accept the default file location.

Adding SSH Key to GitHub/GitLab

Once the key is generated:

	1.	Copy the public key to your clipboard:

cat ~/.ssh/id_rsa.pub


	2.	Go to your GitHub or GitLab account settings.
	3.	Navigate to SSH and GPG keys and paste the key.

Task: Generate an SSH key and add it to your GitHub or GitLab account.

Section 3: Proxy Configuration (Optional)

If you’re working behind a corporate firewall or restricted network, you may need to configure Git to use a proxy.

Setting Up a Proxy

To set up a proxy, run:

git config --global http.proxy http://proxy.example.com:8080

Replace proxy.example.com:8080 with your actual proxy details.

Verifying Proxy Configuration

To confirm the proxy is set up correctly:

git config --global http.proxy

Task: If needed, configure your proxy settings and verify them using the above commands.

Practical Exercise

Follow these steps to complete your setup:

	1.	Configure Git with your username and email.
	2.	Generate an SSH key and add it to your GitHub or GitLab account.
	3.	If necessary, set up a proxy.

Verify your configuration with:

git config --list

Additional Resources

	•	Official Git Documentation
	•	GitHub SSH Key Setup Guide
	•	Git Configuration Best Practices

With these steps completed, Git is now properly configured on your machine, and you’re ready to start working with version control.