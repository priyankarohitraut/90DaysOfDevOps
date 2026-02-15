

What is a branch in Git?
A Git branch is an independent development path that lets you work without affecting the main codebase.

main  → production-ready code
feature-login → new feature you're building
bugfix-header → fixing a bug
Create branch:

git branch feature-login
Create + switch:

git checkout -b feature-login
Switch branch:

git checkout main
git branch

Merge:
git merge feature-login



Why do we use branches instead of committing everything to main?
Risk of Breaking Production
Harder Collaboration
Cleaner History
Safe Experimentation
Code Review / Pull Requests

What is HEAD in Git?
Code Review / Pull Requests
Detached HEAD
Moves With Your Commits

What happens to your files when you switch branches
Git updates your working directory
HEAD moves to the new branch
Uncommitted changes
Staging area (index) stays separate

List all branches in your repo
List Local Branches
git branch

List Remote Branches
git branch -Extra: Show Current Branch Only
git branch --show-current

List All Branches (Local + Remote)
git branch -a

Create a new branch called feature-1
git checkout -b feature-1
git checkout -b feature-1


Switch to feature-1
git checkout feature-1

Create a new branch and switch to it in a single command — call it feature-2
git checkout -b feacture-2

Try using git switch to move between branches — how is it different from git checkout?
git switch feacture-1

Make a commit on feature-1 that does not exist on main

git checkout feature-1
touch hellow.txt
git switch main
git merge feature-1


Switch back to main — verify that the commit from feature-1 is not there
 checked, file is there.

Delete a branch you no longer need
git branch -d feature-1


Branch Creation
Command	                       Purpose
git branch branch-name       	Create a new branch (does not switch to it)
git checkout -b branch-name  	Create a new branch and switch to it
git switch -c branch-name   	Modern equivalent of checkout -b

2️⃣ Switch Between Branches
Command	                      Purpose
git checkout branch-name    	Switch to an existing branch
git switch branch-name        Modern equivalent to switch branch
                             
List Branches
Command	                      Purpose
git branch	                   List local branches
git branch -r	                  List remote branches
git branch -a	                   List all branches (local + remote)
git branch --show-current       	Show current branch only

Rename Branch
Command                               	Purpose
git branch -m old-name new-name      	Rename current or specified branch

5️⃣ Delete Branch
Command	                                 Purpose
git branch -d branch-name               	Delete local branch (safe, merged only)
git branch -D branch-name               	Delete local branch (force, even if unmerged)
git push origin --delete branch-name      	Delete branch on remote (e.g., GitHub)

6️⃣ Merge Branches
Command                                           	Purpose
git merge branch-name	                              Merge branch-name into current branch
git merge --no-ff branch-name                        	Merge with no fast-forward (keeps branch history)

7️⃣ Cherry-Pick Specific Commits
Command	                                                  Purpose
git cherry-pick commit-hash                           	Apply a specific commit from another branch

8️⃣ Stash Before Switching

If you have uncommitted changes:

Command                                                           	Purpose
git stash                                                           	Temporarily save changes
git switch branch-name	                                                  Switch branches safely
git stash pop	                                                         Reapply stashed changes


9️⃣ Check Branch Tracking (Remote)
Command                                                              	Purpose
git branch -vv                                                          	Show branches with remote tracking inf

Create a new repository on GitHub (do NOT initialize it with a README
done

Connect your local devops-git-practice repo to the GitHub remote
connect

Push your main branch to GitHub 
yes

Push feature-1 branch to GitHub
yes

Verify both branches are visible on GitHub
yes

Answer in your notes: What is the difference between origin and upstream? 
origin

origin is the default name Git gives to the remote repository you cloned from (or added manually).
It usually points to your fork or main repository that you own or work on directly.

git push origin branch-name

upstream
upstream is a conventionally used name for the original repository you forked from.
Usually applies in fork workflows:
You fork a repo to your own GitHub account (your origin)
The original repo is called upstream

You fetch changes from upstream to keep your fork updated:

git fetch upstream
git merge upstream/main


Task 4: Pull from GitHub

Make a change to a file directly on GitHub (use the GitHub editor)
yes

Pull that change to your local repo 
git pull 

Answer in your notes: What is the difference between git fetch and git pull?
git fetch

What it does:
Downloads commits, files, and references from a remote repository to your local repository, but does not merge them into your working branch.

Key points:

Updates your remote-tracking branches (like origin/main)
Your current branch stays unchanged
Safe: won’t overwrite local work

Usage example:

git fetch origin


git pull

Fetches from remote AND immediately merges into your current branch.

Key points:

Shortcut for git fetch + git merge
Can create conflicts if your local branch has changes
Convenient for quickly updating your branch

Usage example:

git pull origin main


Task 5: Clone vs Fork

Clone any public repository from GitHub to your local machine
compilted

Fork the same repository on GitHub, then clone your fork
complited

Answer in your notes:
What is the difference between clone and fork?
git clone
What it is:
A Git command that copies a repository from a remote server (like GitHub) to your local machine.

What it does:
Downloads the entire repository
Creates a local working copy
Automatically sets the remote as origin

Example:
git clone https://github.com/priyankarohitraut/devops-git-practice.git

2️ Fork
What it is:
A GitHub feature, not a Git command.
Fork creates a copy of someone else's repository into your GitHub account.

What it does:
Creates a new repo under your GitHub profile
Keeps a connection to the original repository
Lets you modify it without affecting the original project

Example:
You fork:
original-user/project


When would you clone vs fork?
 Clone → When you have write access and just need a local copy to work on.

 Fork → When you don’t have write access and need your own copy on GitHub to contribute.

 Clone = copy to your computer
 Fork = copy to your GitHub account

After forking, how do you keep your fork in sync with the original repo?

Add original repo as upstream
git remote add upstream <original-repo-url>

2️⃣ Fetch updates
git fetch upstream

3️⃣ Merge into your main
git checkout main
git merge upstream/main

4️⃣ Push to your fork
git push origin main



