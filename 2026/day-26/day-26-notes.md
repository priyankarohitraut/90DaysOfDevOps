
Challenge Tasks
Task 1: Install and Authenticate
Install the GitHub CLI on your machine
Authenticate with your GitHub account
Verify you're logged in and check which account is active
Answer in your notes: What authentication methods does gh support?
GitHub CLI Installation & Authentication
1. Install GitHub CLI

Install the GitHub CLI (gh) on Linux:

sudo apt update
sudo apt install gh

Verify installation:

gh --version
2. Authenticate with GitHub

Run the login command:

gh auth login

Follow the prompts:

Choose GitHub.com

Choose HTTPS

Choose Login with a web browser

Copy the code and open the link shown in the terminal

Authorize your account on GitHub

3. Verify Login

Check authentication status:

gh auth status

Example output:

github.com
✓ Logged in to github.com as your-username
✓ Git operations for github.com configured to use https
✓ Token: ****************

This confirms which account is active.

Authentication Methods Supported by gh

The gh CLI supports the following authentication methods:

Method	Description
Web Browser Login (OAuth)	Opens browser to authenticate with GitHub
Personal Access Token (PAT)	Login using a GitHub token
SSH Authentication	Uses SSH keys already configured
GitHub Enterprise Authentication	Login to enterprise GitHub servers
Quick Summary
Command	Purpose
gh --version	Check installation
gh auth login	Authenticate with GitHub
gh auth status	Check logged-in account

Task 2: Working with Repositories

Create a new GitHub repo directly from the terminal — make it public with a README
Clone a repo using gh instead of git clone
View details of one of your repos from the terminal
List all your repositories
Open a repo in your browser directly from the terminal
Delete the test repo you created (be careful!)
Task 2: Working with Repositories (GitHub CLI)
1. Create a New GitHub Repository (Public with README)
gh repo create my-test-repo --public --add-readme

Explanation:

my-test-repo → repository name

--public → makes it public

--add-readme → automatically adds a README file

If you want to clone it immediately:

gh repo create my-test-repo --public --add-readme --clone
2. Clone a Repository Using gh

Instead of git clone, use:

gh repo clone USERNAME/REPOSITORY

Example:

gh repo clone octocat/Hello-World
3. View Details of One of Your Repositories
gh repo view my-test-repo

This shows:

description

visibility

URL

default branch

README preview

For more info:

gh repo view my-test-repo --web
4. List All Your Repositories
gh repo list

Example output includes:

repository name

visibility (public/private)

description

Limit results:

gh repo list --limit 20
5. Open a Repo in Your Browser from the Terminal
gh repo view my-test-repo --web

This automatically opens the repo page in your default browser.

6. Delete the Test Repository (Be Careful)
gh repo delete my-test-repo

You will be asked to confirm.

To skip prompts:

gh repo delete my-test-repo --confirm
Example Workflow (All Together)
gh repo create my-test-repo --public --add-readme
gh repo clone USERNAME/my-test-repo
gh repo view my-test-repo
gh repo list
gh repo view my-test-repo --web
gh repo delete my-test-repo

✅ What you should include in your notes

Commands used for:

Creating repos

Cloning repos

Viewing repo info

Listing repos

Opening repos in browser

Deleting repos

Task 3: Issues

Create an issue on one of your repos from the terminal — give it a title, body, and a label
List all open issues on that repo
View a specific issue by its number
Close an issue from the terminal
Answer in your notes: How could you use gh issue in a script or automation?
Task 3: Working with Issues (GitHub CLI)
1. Create an Issue (Title, Body, Label)

Basic command:

gh issue create --title "Bug in login page" \
--body "Users cannot log in when using Firefox." \
--label "bug"

Example with a specific repo:

gh issue create \
--repo USERNAME/REPO \
--title "Bug in login page" \
--body "Users cannot log in when using Firefox." \
--label "bug"

What each flag does:

--title → issue title

--body → description of the issue

--label → categorize the issue (bug, enhancement, etc.)

2. List All Open Issues
gh issue list

Example output:

#12  Bug in login page        OPEN
#11  Add dark mode feature    OPEN

Limit results:

gh issue list --limit 20

Filter by label:

gh issue list --label bug
3. View a Specific Issue by Number

Example: view issue #12

gh issue view 12

This shows:

title

description

labels

comments

status

Open it in browser:

gh issue view 12 --web
4. Close an Issue from the Terminal
gh issue close 12

Add a closing comment:

gh issue close 12 --comment "Issue fixed in latest commit."
Example Workflow
gh issue create --title "Bug in login page" --body "Users cannot log in when using Firefox." --label bug
gh issue list
gh issue view 1
gh issue close 1
Answer for Your Notes

How could you use gh issue in a script or automation?

You can use gh issue in scripts to automatically manage issues, such as:

Creating issues when automated tests fail

Listing issues to generate reports

Automatically closing resolved issues after deployments

Assigning issues during CI/CD workflows

Integrating with cron jobs or monitoring systems

Example automation script:

#!/bin/bash

gh issue create \
--title "Server Down Alert" \
--body "Monitoring system detected server outage." \
--label "urgent"


Task 4: Pull Requests

Create a branch, make a change, push it, and create a pull request entirely from the terminal
List all open PRs on a repo
View the details of your PR — check its status, reviewers, and checks
Merge your PR from the terminal
Answer in your notes:
What merge methods does gh pr merge support?
How would you review someone else's PR using gh?
Task 4: Pull Requests (GitHub CLI)
1. Create a Branch → Make Change → Push → Create PR
Step 1 — Create and switch to a new branch
git checkout -b feature-update
Step 2 — Make a change

Example:

echo "New feature added" >> README.md
Step 3 — Commit the change
git add README.md
git commit -m "Add new feature note to README"
Step 4 — Push the branch
git push origin feature-update
Step 5 — Create the Pull Request
gh pr create --title "Add feature update" --body "This PR updates the README with a new feature note."

Optional flags:

--base main
--head feature-update

Example:

gh pr create --base main --head feature-update \
--title "Add feature update" \
--body "Updating README with feature info."
2. List All Open Pull Requests
gh pr list

Example output:

#15  Add feature update   OPEN
#14  Fix login bug        OPEN

Limit results:

gh pr list --limit 20
3. View Details of Your Pull Request

Example for PR #15

gh pr view 15

This shows:

PR title and description

author

reviewers

CI checks

status (open/merged)

Open in browser:

gh pr view 15 --web
4. Merge the Pull Request from Terminal
gh pr merge 15

You can specify merge strategy:

gh pr merge 15 --merge

Or:

gh pr merge 15 --squash
Example Workflow
git checkout -b feature-update
echo "New feature added" >> README.md
git add README.md
git commit -m "Update README"
git push origin feature-update

gh pr create --title "Update README" --body "Adds feature description"

gh pr list
gh pr view 1
gh pr merge 1
Answers for Your Notes
1. What merge methods does gh pr merge support?

gh pr merge supports three merge strategies:

Merge commit

gh pr merge --merge

Creates a merge commit and keeps the full commit history.

Squash merge

gh pr merge --squash

Combines all commits into one commit before merging.

Rebase merge

gh pr merge --rebase

Replays commits on top of the base branch without creating a merge commit.

2. How would you review someone else's PR using gh?

Steps:

List pull requests

gh pr list

Checkout the PR locally

gh pr checkout 15

View PR details

gh pr view 15

Review and comment

gh pr review 15 --comment

Approve the PR:

gh pr review 15 --approve

Request changes:

gh pr review 15 --request-changes

Task 5: GitHub Actions & Workflows (Preview)

List the workflow runs on any public repo that uses GitHub Actions
View the status of a specific workflow run
Answer in your notes: How could gh run and gh workflow be useful in a CI/CD pipeline?
(Don't worry if you haven't learned GitHub Actions yet — this is a preview for upcoming days)
Task 5: GitHub Actions & Workflows
1. List Workflow Runs on a Repository

To see workflow runs for the current repository:

gh run list

Example output:

STATUS   TITLE        WORKFLOW   BRANCH   EVENT   ID
✓        CI Build     CI         main     push    12345678
X        Test Suite   CI         main     push    12345679

If you're viewing a specific public repository:

gh run list --repo OWNER/REPOSITORY

Example:

gh run list --repo cli/cli

This will show:

workflow status (success/failure)

workflow name

branch

event trigger

run ID

2. View the Status of a Specific Workflow Run

Use the run ID from the previous command.

Example:

gh run view 12345678

This shows:

workflow status

jobs inside the workflow

logs

duration

commit information

You can also view logs:

gh run view 12345678 --log

Open the run in your browser:

gh run view 12345678 --web
Example Workflow Commands
gh run list
gh run view 12345678
gh run view 12345678 --log
Answers for Your Notes
1. How could gh run and gh workflow be useful in a CI/CD pipeline?

They allow developers to monitor, control, and debug CI/CD pipelines directly from the terminal.

Examples of uses:

Monitor pipeline status
gh run list

Check whether builds or deployments succeeded.

View logs for failed builds
gh run view RUN_ID --log

Helps debug failing tests or build errors.

Trigger workflows manually
gh workflow run workflow.yml

Useful for:

triggering deployments

running test pipelines

scheduled automation

Automate CI/CD monitoring

You can include gh commands in scripts to:

notify developers when builds fail

automatically retry workflows

fetch workflow logs for debugging

monitor deployment status

Example script:

gh run list --limit 1

This could be used to check the latest CI run status in automation.


Task 6: Useful gh Tricks

Explore and try these — add the ones you find useful to your git-commands.md:

gh api — make raw GitHub API calls from the terminal
gh gist — create and manage GitHub Gists
gh release — create and manage releases
gh alias — create shortcuts for commands you use often
gh search repos — search GitHub repos from the terminal
Task 6: Useful gh Tricks
1. gh api — Call the GitHub API from Terminal

This lets you interact with the GitHub REST API without writing curl commands.

Example: Get your user info

gh api user

List your repositories:

gh api user/repos

Example: Get repo details

gh api repos/OWNER/REPO

Why it's useful:

automation scripts

retrieving GitHub data

integrating GitHub with tools

Example automation use:

gh api user/repos --jq '.[].name'

This prints only repo names.

2. gh gist — Manage GitHub Gists

Create a gist (share code snippets quickly).

Create a gist:

gh gist create file.txt

Create a public gist with description:

gh gist create file.txt --public --desc "Example snippet"

List your gists:

gh gist list

View a gist:

gh gist view GIST_ID

Why it's useful:

sharing quick code snippets

storing scripts

debugging examples

3. gh release — Manage Releases

Used to create versioned software releases.

Create a release:

gh release create v1.0.0

Create with notes:

gh release create v1.0.0 --notes "First release"

Upload release files:

gh release create v1.0.0 dist/app.zip

List releases:

gh release list

Why it's useful:

publishing software versions

distributing compiled binaries

release automation

4. gh alias — Create Custom Shortcuts

You can create custom commands to save time.

Example: alias for listing PRs

gh alias set prl "pr list"

Use it like this:

gh prl

Example: open repo in browser

gh alias set openrepo "repo view --web"

Now run:

gh openrepo

Why it's useful:

faster workflows

personalized CLI commands

List aliases:

gh alias list
5. gh search repos — Search GitHub from Terminal

Search repositories directly from CLI.

Example:

gh search repos docker

Search by language:

gh search repos "machine learning" --language python

Limit results:

gh search repos "react dashboard" --limit 5

Why it's useful:

discover projects quickly

research libraries

find open-source examples

Example Section for Your git-commands.md

You could write something like:

Useful GitHub CLI Commands

gh api user
→ Call GitHub API from terminal

gh gist create file.txt
→ Create a GitHub gist

gh release create v1.0.0
→ Create a new repository release

gh alias set prl "pr list"
→ Create shortcut command

gh search repos docker
→ Search GitHub repositories



