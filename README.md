# repodeletion
Deleting multiple repositories using the GitHub CLI

# GitHub Repository Bulk Deletion Script

**WARNING: This script is designed to delete multiple GitHub repositories at once. This is a PERMANENT and IRREVERSIBLE action. Use with EXTREME CAUTION. Back up your repositories before proceeding.**

## Description

This script provides a way to delete multiple GitHub repositories from the command line using the GitHub CLI (`gh`). It automates the process of listing repositories, creating a list for review, and then deleting the repositories in that list.

**Key Features:**

*   **GitHub CLI Check:** Verifies that the GitHub CLI (`gh`) is installed.
*   **Dry Run Mode:** Allows you to test the script without actually deleting any repositories.
*   **Repository List File:** Creates a file containing the list of repositories to be deleted, allowing for careful review and editing.
*   **Clear Output:** Provides informative output to the console.
*   **Handles Older `gh` Versions:** Includes instructions for adapting the script to work with older versions of the GitHub CLI.

## Prerequisites

*   **GitHub CLI (`gh`):** You need to have the GitHub CLI installed on your system. See installation instructions below.
*   **GitHub Account:** You need a GitHub account with the necessary permissions to delete the repositories.
*   **Authentication:** You need to be authenticated with your GitHub account using the GitHub CLI.

## Installation

1.  **Install the GitHub CLI (`gh`):**

*   **Ubuntu:**

    ```bash
    sudo apt update
    sudo apt install gh
    ```

    If the `gh` package is not found, add the GitHub APT repository:

    ```bash
    curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo gpg --dearmor -o /usr/share/keyrings/githubcli-archive-keyring.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null
    sudo apt update
    sudo apt install gh
    ```

*   **Other Operating Systems:** See the official GitHub CLI installation guide: [https://cli.github.com/](https://cli.github.com/)

2.  **Authenticate with GitHub:**

```bash
gh auth login

