
Task 1: Install and Configure Git

Verify Git is installed on your machine
1. Verify Git is Installed

First, check whether Git is installed on your system.

git --version
Example Output
git version 2.43.0

Set up your Git identity — name and email
Set Up Your Git Identity

Configure your name and email address so your commits are properly labeled.

git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
Example
git config --global user.name "DevOps Engineer"
git config --global user.email "devops@example.com"

The --global option applies the configuration to all repositories on your system.

4

Verify your configuration
Verify Git Configuration

Check your configured Git settings with:

git config --list
Example Output
user.name=DevOps Engineer
user.email=devops@example.com

You can also check them individually:

git config user.name
git config user.email

✅ Summary

Step	Command
Check Git installation	git --version
Install Git (Ubuntu)	sudo apt install git -y
Set username	git config --global user.name "Your Name"
Set email	git config --global user.email "email@example.com"
Verify configuration	git config --list

Task 2: Create Your Git Project

Create a new folder called devops-git-practice
Initialize it as a Git repository
Check the status — read and understand what Git is telling you
Explore the hidden .git/ directory — look at what's inside
Task 2: Create Your Git Project
1. Create a New Project Folder

Create a new directory called devops-git-practice.

mkdir devops-git-practice

Move into the directory:

cd devops-git-practice
2. Initialize It as a Git Repository

Initialize the folder using Git.

git init
Example Output
Initialized empty Git repository in /home/user/devops-git-practice/.git/

This command creates a hidden .git directory that allows Git to track changes.

3. Check Git Status

Run the following command:

git status
Example Output
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
What This Means
Message	Meaning
On branch main	Your current branch is main
No commits yet	You haven't saved any versions yet
nothing to commit	No files are being tracked yet
4. View Hidden Files

The .git folder is hidden. To see it:

ls -la

Example output:

.
..
.git
5. Explore the .git Directory

Go inside the Git directory:

cd .git
ls

Example contents:

HEAD
config
description
hooks
info
objects
refs
Important Files and Folders
Item	Purpose
HEAD	Points to the current branch
config	Repository configuration
objects	Stores all Git objects (commits, blobs)
refs	Stores branch and tag references
hooks	Scripts triggered by Git events
6. Return to Project Directory
cd ..

You are now back inside:

devops-git-practice/
Final Project Structure
devops-git-practice/
 └── .git/
     ├── HEAD
     ├── config
     ├── objects/
     ├── refs/
     └── hooks/

✅ Summary

Step	Command
Create project folder	mkdir devops-git-practice
Enter folder	cd devops-git-practice
Initialize Git	git init
Check status	git status
Show hidden files	ls -la
Explore Git directory	cd .git


Task 3: Create Your Git Commands Reference

Create a file called git-commands.md inside the repo
Add the Git commands you've used so far, organized by category:
Setup & Config
Basic Workflow
Viewing Changes
For each command, write:
What it does (1 line)
An example of how to use it
1. Setup & Configuration
git --version

What it does:
Displays the installed Git version.

Example

git --version
git config --global user.name

What it does:
Sets the username that will appear in your commits.

Example

git config --global user.name "DevOps Engineer"
git config --global user.email

What it does:
Sets the email address associated with your commits.

Example

git config --global user.email "devops@example.com"
git config --list

What it does:
Displays all Git configuration settings.

Example

git config --list
2. Basic Workflow
git init

What it does:
Initializes a new Git repository in the current directory.

Example

git init
git status

What it does:
Shows the current state of the repository, including tracked and untracked files.

Example

git status
git add

What it does:
Adds files to the staging area before committing.

Example

git add git-commands.md

Add all files:

git add .
git commit

What it does:
Records changes from the staging area to the repository history.

Example

git commit -m "Add Git commands reference file"
3. Viewing Changes
git log

What it does:
Shows the commit history of the repository.

Example

git log
git diff

What it does:
Displays changes between files, commits, or the working directory.

Example

git diff
git status

What it does:
Shows which files have been modified, staged, or are untracked.

Example

git status
Example Workflow

Typical Git workflow:

git init
git status
git add .
git commit -m "Initial commit"
git log

✅ After creating the file, run:

git add git-commands.md
git commit -m "Add Git commands reference"

Task 4: Stage and Commit

Stage your file
Check what's staged
Commit with a meaningful message
View your commit history
Task 4: Stage and Commit

In this task, you will stage your file, check what is staged, commit the changes, and view the commit history using Git.

1. Stage Your File

Add the file git-commands.md to the staging area.

git add git-commands.md

This tells Git that the file should be included in the next commit.

You can also stage all files:

git add .
2. Check What’s Staged

Run:

git status

Example output:

On branch main

Changes to be committed:
  new file:   git-commands.md
What this means
Message	Meaning
Changes to be committed	Files are staged and ready to commit
new file	Git detected a new tracked file
3. Commit with a Meaningful Message

Create a commit to save the staged changes.

git commit -m "Add Git commands reference file"

Example output:

[main (root-commit) a1b2c3d] Add Git commands reference file
 1 file changed, 40 insertions(+)
 create mode 100644 git-commands.md
Why meaningful messages matter

Good commit messages help explain what changed and why.

Example good messages:

Add Git commands reference
Fix typo in documentation
Update workflow examples
4. View Your Commit History

Use:

git log

Example output:

commit a1b2c3d4e5f6
Author: DevOps Engineer <devops@example.com>
Date:   Tue Jul 2 10:15:00 2025

    Add Git commands reference file
Shorter Log Format (Optional)
git log --oneline

Example:

a1b2c3d Add Git commands reference file
Example Full Workflow
git status
git add git-commands.md
git status
git commit -m "Add Git commands reference file"
git log

✅ Summary Table

Step	Command	Purpose
Stage file	git add git-commands.md	Add file to staging area
Check staged files	git status	See what will be committed
Commit changes	git commit -m "message"	Save changes to Git history
View commits	git log	Show commit history


Task 5: Make More Changes and Build History

Edit git-commands.md — add more commands as you discover them
Check what changed since your last commit
Stage and commit again with a different, descriptive message
Repeat this process at least 3 times so you have multiple commits in your history
View the full history in a compact format
Task 5: Make More Changes and Build History

In this task, you will continue editing your git-commands.md file and create multiple commits using Git to build a commit history.

1. Edit git-commands.md

Open the file and add more Git commands as you learn them.

Example additions:

### git branch
Shows available branches in the repository.

Example:
git branch
### git checkout
Switches between branches.

Example:
git checkout main

Save the file after making changes.

2. Check What Changed Since Your Last Commit

Use the command:

git diff

This command shows the exact changes made to the file since the last commit.

Example output:

+ ### git branch
+ Shows available branches in the repository.
3. Stage the Changes

Add the updated file to the staging area.

git add git-commands.md

Verify the staging area:

git status
4. Commit with a Descriptive Message

Create a commit describing the changes.

git commit -m "Add branch and checkout commands to Git reference"
5. Repeat the Process (At Least 3 Times)

Continue editing the file and committing new changes.

Example commit messages:

Add branch and checkout commands
Add git diff and git log explanations
Update examples for git status and git add

Example workflow:

git diff
git add git-commands.md
git commit -m "Add git diff command explanation"

git diff
git add git-commands.md
git commit -m "Update Git workflow examples"

git diff
git add git-commands.md
git commit -m "Add git branch command description"
6. View the Full Commit History (Compact Format)

Use the compact log view:

git log --oneline

Example output:

a4c2e1d Add git branch command description
b9f41ab Update Git workflow examples
c32d911 Add git diff command explanation
a1b2c3d Add Git commands reference file

Each line represents a commit in your project history.

Example Commit Workflow
edit git-commands.md
git diff
git add git-commands.md
git commit -m "Add git diff command"

edit git-commands.md
git add git-commands.md
git commit -m "Add git branch command"

git log --oneline
Summary
Step	Command	Purpose
Check changes	git diff	Show changes since last commit
Stage changes	git add git-commands.md	Add file to staging
Commit changes	git commit -m "message"	Save changes to history
View history	git log --oneline	Display compact commit history

Task 6: Understand the Git Workflow
Answer these questions in your own words (add them to a day-22-notes.md file):

What is the difference between git add and git commit?
What does the staging area do? Why doesn't Git just commit directly?
What information does git log show you?
What is the .git/ folder and what happens if you delete it?
What is the difference between a working directory, staging area, and repository?
1. What is the difference between git add and git commit?

git add and git commit are two different steps in the Git workflow.

git add: Moves changes from the working directory to the staging area.

git commit: Saves the staged changes permanently to the repository history.

Example workflow:

git add file.txt
git commit -m "Add new file"

In simple terms:

git add → prepares the changes

git commit → records the changes

2. What does the staging area do? Why doesn’t Git commit directly?

The staging area acts as a preparation area where you select which changes will go into the next commit.

This is useful because:

You can choose specific files to commit.

You can organize commits logically.

You avoid committing unfinished changes.

Example:

If you modified three files but only want to commit one:

git add file1.txt
git commit -m "Update file1"

Git will only commit file1.txt.

3. What information does git log show you?

The git log command displays the commit history of a repository.

It usually shows:

Commit ID (unique hash)

Author name

Author email

Date of the commit

Commit message

Example:

commit a1b2c3d4
Author: DevOps Engineer
Date: Tue Jul 2 10:15:00 2025

Add Git commands reference file

This helps developers track project history and understand what changes were made.

4. What is the .git/ folder and what happens if you delete it?

The .git/ folder is the internal database of Git.

It stores:

commit history

branches

configuration

repository metadata

If you delete the .git/ folder:

The project stops being a Git repository

All commit history is permanently lost

Git commands like git status will stop working

Your files will still exist, but Git tracking will disappear.

5. What is the difference between a Working Directory, Staging Area, and Repository?

Git tracks changes through three main areas.

Working Directory

The working directory is the folder where you edit your project files.

Example:

devops-git-practice/
   git-commands.md

This is where you make changes to files.

Staging Area

The staging area is where changes are prepared before committing.

Files enter the staging area using:

git add file.txt
Repository

The repository stores the permanent history of commits.

Files are saved in the repository when you run:

git commit -m "message"
Git Workflow Summary
Working Directory
        ↓
     git add
        ↓
   Staging Area
        ↓
     git commit
        ↓
     Repository





O
