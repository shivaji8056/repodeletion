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
Follow the prompts to authenticate. Choose HTTPS and use a personal access token (PAT) if prompted. You'll need to grant the PAT the delete_repo scope.

Usage
Download the Script:
Download the delete_repos.sh script to your local machine.

Make the Script Executable:
chmod +x delete_repos.sh
Edit the Script:
vi delete_repos.sh  # or nano delete_repos.sh
USERNAME="YOUR_GITHUB_USERNAME": Replace YOUR_GITHUB_USERNAME with your GitHub username or the name of the GitHub organization that owns the repositories you want to delete. (Note: This variable is not used in the modified script for older gh versions, but it's good practice to set it.)
DRY_RUN=true: Leave this set to true for the initial run. This is a safety feature that prevents the script from actually deleting anything.
Run the Script in Dry Run Mode:
./delete_repos.sh
The script will:

Check if gh is installed.
List the repositories owned by the specified user.
Display the list of repositories in the terminal.
Create a file named repos_to_delete.txt in the same directory as the script, containing the list of repositories.
Print a message saying that the script is running in dry run mode and no repositories will be deleted.
Review repos_to_delete.txt (CRITICAL):
vi repos_to_delete.txt  # or nano repos_to_delete.txt
Carefully examine the list of repositories.
Remove any repositories that you DO NOT want to delete. Delete the corresponding lines in the file.
Save the repos_to_delete.txt file.
(Double-Check Everything!)
Before proceeding, make absolutely sure that the repos_to_delete.txt file contains ONLY the repositories you want to delete, and that you have backups of any important repositories. There is no undo!
If you are absolutely sure, set DRY_RUN=false in the script:
vi delete_repos.sh  # or nano delete_repos.sh
Change the line DRY_RUN=true to DRY_RUN=false.
Save the script.
Run the Script (THIS WILL DELETE REPOSITORIES!):
./delete_repos.sh
The script will:

Check if gh is installed.
List the repositories owned by the specified user.
Read the list of repositories from repos_to_delete.txt.
Delete each repository in the list using the gh repo delete command.
Print a message saying that the deletion is complete.
Verify the Deletion:
Go to your GitHub account in a web browser and verify that the repositories you intended to delete are no longer present.

Script Code (delete_repos.sh)
#!/bin/bash

# --- CONFIGURATION (CHANGE THESE) ---
USERNAME="YOUR_GITHUB_USERNAME"  # Replace with your GitHub username or organization name (NOT USED IN THIS VERSION)
DRY_RUN=true  # Set to 'false' to actually delete repositories (VERY DANGEROUS!)

# --- Check if gh is installed ---
if ! command -v gh &> /dev/null; then
echo "Error: GitHub CLI (gh) is not installed."
echo "Please install it: https://cli.github.com/"
exit 1
fi

# --- List repositories ---
echo "Listing repositories for the authenticated user..."
repos=$(gh repo list --limit 1000 | awk '{print $1}')

# --- Check if any repositories were found ---
if [[ -z "$repos" ]]; then
echo "No repositories found for the authenticated user."
exit 0
fi

# --- Display repositories to be deleted ---
echo "The following repositories will be deleted:"
echo "$repos"
echo ""

# --- Create a list of repositories to delete (for review) ---
echo "$repos" > repos_to_delete.txt
echo "A list of repositories has been saved to repos_to_delete.txt.  Please review it carefully!"

# --- Delete repositories (if DRY_RUN is false) ---
if [ "$DRY_RUN" = "false" ]; then
echo "Deleting repositories..."
cat repos_to_delete.txt | xargs -I {} gh repo delete {} --confirm
echo "Deletion complete."
else
echo "Dry run mode: No repositories were deleted.  Set DRY_RUN=false to actually delete them."
fi

exit 0
Troubleshooting
unknown flag: --owner: This error indicates that you have an older version of the GitHub CLI. Remove the --owner flag from the gh repo list command in the script. The script will then list repositories for the currently authenticated user.
unknown flag: --yes: This error indicates that you have an older version of the GitHub CLI. Replace the --yes flag with --confirm in the gh repo delete command in the script.
Repositories are not being deleted:
Make sure you have the necessary permissions to delete the repositories.
Double-check that DRY_RUN=false in the script.
Verify that the repos_to_delete.txt file contains the correct list of repositories.
If using an older version of gh, you might be prompted to confirm each deletion manually.
Rate Limiting: If you're deleting a large number of repositories, you might encounter rate limiting errors. Consider adding delays between deletions (e.g., using sleep within the xargs command, but this makes the process very slow).
Important Notes
Backups: Before running the script with DRY_RUN=false, make sure you have backups of any repositories that are important to you.
Permissions: You must have the necessary permissions to delete the repositories. You must be the owner or have administrative access.
Double-Check: Double-check everything before running the script with DRY_RUN=false. There is no undo!
Organizations: If you're deleting repositories within an organization, make sure you have the correct permissions and that you're not deleting repositories that are actively used by the organization.
Consider Archiving Instead of Deleting: GitHub allows you to archive repositories. Archived repositories are read-only and don't count towards your storage limits. This might be a better option than deleting them entirely.
Disclaimer
This script is provided as-is, without warranty of any kind. Use at your own risk. The author is not responsible for any data loss or other damages resulting from the use of this script.


**Explanation of the README:**

*   **Clear Warnings:** The README starts with prominent warnings about the destructive nature of the script.
*   **Comprehensive Description:** It explains the purpose of the script and its key features.
*   **Detailed Prerequisites:** It lists all the necessary prerequisites, including the GitHub CLI, a GitHub account, and authentication.
*   **Step-by-Step Installation Instructions:** It provides detailed instructions on how to install the GitHub CLI on Ubuntu and other operating systems.
*   **Detailed Usage Instructions:** It provides a step-by-step guide on how to use the script, including how to run it in dry run mode, review the list of repositories, and then delete the repositories.
*   **Script Code:** It includes the complete script code for easy reference.
*   **Troubleshooting Section:** It provides solutions to common problems, such as the `unknown flag` errors.
*   **Important Notes:** It reiterates the important precautions and considerations.
*   **Disclaimer:** It includes a disclaimer to protect the author from liability.

**How to Use This README:**

1.  **Copy the Markdown code:** Copy the entire code block above.
2.  **Create a `README.md` file:** Create a file named `README.md` in the root directory of your GitHub repository.
3.  **Paste the code:** Paste the code into the `README.md` file.
4.  **Customize the README:**
*   **Replace `YOUR_GITHUB_USERNAME`:**  Replace this placeholder with your actual GitHub username or organization name in the script code within the README.
*   **Add your name/contact info:** Consider adding your name or contact information to the README.
*   **Adjust the description:**  Modify the description to accurately reflect the purpose of your specific repository.
*   **Add screenshots (optional):**  Consider adding screenshots to illustrate the steps.
5.  **Commit and push:** Commit the `README.md` file to your GitHub repository and push the changes.

This README should provide a clear and comprehensive guide for anyone wh
