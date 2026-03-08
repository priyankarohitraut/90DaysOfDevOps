
Challenge Tasks
Task 1: Git Reset — Hands-On

Make 3 commits in your practice repo (commit A, B, C)
Use git reset --soft to go back one commit — what happens to the changes?
Re-commit, then use git reset --mixed to go back one commit — what happens now?
Re-commit, then use git reset --hard to go back one commit — what happens this time?
Answer in your notes:
What is the difference between --soft, --mixed, and --hard?
Which one is destructive and why?
When would you use each one?
Should you ever use git reset on commits that are already pushed?
Git Reset — Hands-On Notes
1. Difference between --soft, --mixed, and --hard
Command	What happens to commits	What happens to staged changes	What happens to working directory
git reset --soft	Moves HEAD to previous commit	Changes stay staged	Files stay the same
git reset --mixed (default)	Moves HEAD to previous commit	Changes become unstaged	Files stay the same
git reset --hard	Moves HEAD to previous commit	Staged changes removed	Working directory is reset and changes are deleted
Example with commits A → B → C

Current history:

A → B → C (HEAD)
git reset --soft HEAD~1
A → B (HEAD)

Changes from commit C remain staged

You can recommit immediately

git reset --mixed HEAD~1
A → B (HEAD)

Changes from commit C remain in files

But they are unstaged

git reset --hard HEAD~1
A → B (HEAD)

Commit C disappears

All file changes from C are deleted

2. Which one is destructive and why?

git reset --hard is destructive.

Reason:

It permanently deletes changes from the working directory and staging area.

If the commit is not saved elsewhere (e.g., another branch or remote), the changes are lost.

3. When would you use each one?
--soft

Use when:

You want to undo a commit but keep everything staged

Example: fixing the commit message or combining commits.

Example:

git reset --soft HEAD~1
git commit -m "Better commit message"
--mixed

Use when:

You want to undo a commit but keep the code changes

You want to edit files before recommitting

Example:

git reset HEAD~1
--hard

Use when:

You want to completely discard commits and changes

Reset the project to a clean previous state

Example:

git reset --hard HEAD~1
4. Should you ever use git reset on commits that are already pushed?

Generally, NO.

Reason:

git reset rewrites history

If commits were pushed, other collaborators may already have them

This causes conflicts and broken histories

If you must undo a pushed commit, use:

git revert <commit-hash>

git revert creates a new commit that reverses the changes, which is safe for shared repositories.

✅ Summary

--soft → undo commit, keep staged changes

--mixed → undo commit, keep unstaged changes

--hard → undo commit and delete changes (dangerous)

Avoid using reset on pushed commits

Task 2: Git Revert — Hands-On

Make 3 commits (commit X, Y, Z)
Revert commit Y (the middle one) — what happens?
Check git log — is commit Y still in the history?
Answer in your notes:
How is git revert different from git reset?
Why is revert considered safer than reset for shared branches?
When would you use revert vs reset?
Git Revert — Hands-On Notes
1. What happens when you revert the middle commit (Y)?

Assume the commit history:

X → Y → Z (HEAD)

If you run:

git revert <hash-of-Y>

Git creates a new commit that reverses the changes introduced by commit Y.

New history becomes:

X → Y → Z → Revert-Y (HEAD)

Important points:

Git does not delete Y

Instead, it adds a new commit that undoes Y’s changes

2. Is commit Y still in the history?

Yes.

When you run:

git log

You will see something like:

Revert "commit Y"
Z
Y
X

So:

Commit Y still exists

Git simply added another commit that cancels its effects.

3. How is git revert different from git reset?
Feature	git revert	git reset
History rewritten?	❌ No	✅ Yes
Creates a new commit?	✅ Yes	❌ No
Removes commits from history?	 No	Yes
Safe for shared repos?	 Yes	Usually not

Key idea:

git revert → undo by adding a new commit

git reset → undo by removing commits

4. Why is revert safer for shared branches?

git revert is safer because:

It does not rewrite Git history

Everyone's commit history stays consistent

No forced updates are needed

If you used git reset after pushing:

The commit history changes

Other developers may have the old history

This causes merge conflicts and broken histories

5. When would you use revert vs reset?
Use git revert when:

The commit is already pushed

You are working on a shared branch (main / develop)

You want to safely undo a change

Example:

git revert <commit-hash>
Use git reset when:

The commit is local only

You want to rewrite history

You made a mistake and want to redo commits

Example:

git reset --soft HEAD~1
Quick Summary

git revert

Keeps history

Adds a new commit that undoes changes

Safe for shared branches

git reset

Removes commits

Rewrites history

Best for local commits only

Task 3: Reset vs Revert — Summary

Create a comparison in your notes:

git reset	git revert
What it does	?	?
Removes commit from history?	?	?
Safe for shared/pushed branches?	?	?
When to use	?	?
Reset vs Revert — Summary
Feature	git reset	git revert
What it does	Moves the branch pointer backward and removes commits from the current history	Creates a new commit that undoes the changes from a previous commit
Removes commit from history?	Yes (history is rewritten)	 No (commit stays in history)
Safe for shared/pushed branches?	 No	 Yes
When to use	When working on local commits and you want to undo or modify them	When a commit is already pushed and you want to safely undo its changes
Simple Example
git reset

Before:

A → B → C (HEAD)

After git reset HEAD~1:

A → B (HEAD)

Commit C disappears from history.

git revert

Before:

A → B → C (HEAD)

After git revert B:

A → B → C → Revert B (HEAD)

Commit B stays, but a new commit cancels its changes.

✅ Quick memory trick for exams/labs

Reset = Rewrite history

Revert = Reverse changes safely


Task 4: Branching Strategies

Research the following branching strategies and document each in your notes with:

How it works (short description)
A simple diagram or flow (text-based is fine)
When/where it's used
Pros and cons
GitFlow — develop, feature, release, hotfix branches
GitHub Flow — simple, single main branch + feature branches
Trunk-Based Development — everyone commits to main, short-lived branches
Answer:
Which strategy would you use for a startup shipping fast?
Which strategy would you use for a large team with scheduled releases?
Which one does your favorite open-source project use? (check any repo on GitHub)
Branching Strategies
1. GitFlow
How it works

GitFlow is a structured branching strategy with multiple long-lived branches. Development happens on develop, while main stores production code. Feature, release, and hotfix branches manage development and fixes.

Diagram (flow)
main ────────────────●───────────────●
                      ↑ release       ↑ hotfix
develop ──●──●──●──●──────────●──●──●
            │  │  │
         feature feature feature

Branches:

main → production code

develop → integration branch

feature/* → new features

release/* → preparing releases

hotfix/* → urgent production fixes

When/Where it's used

Large teams

Projects with scheduled releases

Enterprise software development

Pros

Clear structure

Supports release planning

Good for large teams

Cons

Complex workflow

Too many branches

Slower development speed

2. GitHub Flow
How it works

GitHub Flow is a simple workflow using a single main branch and short-lived feature branches.

Steps:

Create a branch from main

Work on the feature

Open a pull request

Review and merge into main

Deploy

Diagram
main ──●────────●────────●
        │        │
     feature   feature
        │        │
        └───merge┘
When/Where it's used

Web apps

Continuous deployment teams

Open-source projects on GitHub

Pros

Simple and easy

Fast development

Encourages pull requests and reviews

Cons

Less structure for large releases

Harder to manage big projects with long release cycles

3. Trunk-Based Development
How it works

Trunk-Based Development means developers frequently commit to the main branch (trunk). Feature branches exist but are very short-lived.

Diagram
main (trunk)
 ──●─●─●─●─●─●─●
    │ │
  small branches
   merged quickly
When/Where it's used

Continuous integration environments

DevOps teams

Large tech companies practicing CI/CD

Pros

Faster integration

Reduces merge conflicts

Encourages frequent commits

Cons

Requires strong automated testing

Risky if testing is weak

Answers
Which strategy would you use for a startup shipping fast?

GitHub Flow

Reason:

Simple workflow

Faster releases

Works well with continuous deployment

Which strategy would you use for a large team with scheduled releases?

GitFlow

Reason:

Structured process

Supports release management

Good for large teams

Which one does your favorite open-source project use?

Example: Linux Kernel

Branching style:

Similar to Trunk-Based Development

Development happens on the main integration branch with many contributors merging frequently.

✅ Quick summary

Strategy	Best for
GitFlow	Large teams with scheduled releases
GitHub Flow	Startups & fast deployments
Trunk-Based Development	CI/CD environments


Task 5: Git Commands Reference Update

Update your git-commands.md to cover everything from Days 22–25:

Setup & Config
Basic Workflow (add, commit, status, log, diff)
Branching (branch, checkout, switch)
Remote (push, pull, fetch, clone, fork)
Merging & Rebasing
Stash & Cherry Pick
Reset & Revert
Git Commands Reference
1. Setup & Configuration

Initialize a new Git repository.

git init

Clone an existing repository.

git clone <repository-url>

Set username.

git config --global user.name "Your Name"

Set email.

git config --global user.email "your@email.com"

Check configuration.

git config --list
2. Basic Workflow

Check repository status.

git status

Add a file to staging area.

git add filename

Add all files.

git add .

Commit changes.

git commit -m "commit message"

View commit history.

git log

Short log format.

git log --oneline

See changes between commits.

git diff
3. Branching

Create a new branch.

git branch branch-name

Switch to a branch.

git checkout branch-name

Create and switch branch.

git checkout -b branch-name

Using the modern command:

git switch branch-name

List branches.

git branch

Delete branch.

git branch -d branch-name
4. Remote Repositories

Add remote repository.

git remote add origin <repo-url>

Push to remote.

git push origin branch-name

Push first time with upstream.

git push -u origin main

Pull latest changes.

git pull origin main

Fetch changes without merging.

git fetch

Clone repository.

git clone <repo-url>

Fork repository (done on GitHub UI).

5. Merging & Rebasing

Merge another branch into current branch.

git merge branch-name

Rebase current branch onto another.

git rebase branch-name

Abort rebase if conflicts occur.

git rebase --abort

Continue after resolving conflicts.

git rebase --continue
6. Stash & Cherry-Pick

Temporarily save changes.

git stash

View stashed changes.

git stash list

Apply stashed changes.

git stash apply

Delete stash.

git stash drop

Apply a specific commit to another branch.

git cherry-pick <commit-hash>
7. Reset & Revert

Reset to previous commit (keep staged changes).

git reset --soft HEAD~1

Reset and unstage changes.

git reset --mixed HEAD~1

Reset and remove changes completely.

git reset --hard HEAD~1

Revert a commit safely.

git revert <commit-hash>
Quick Summary
Command	Purpose
git init	Initialize repository
git add	Stage changes
git commit	Save changes
git branch	Manage branches
git push	Upload changes
git pull	Download changes
git merge	Combine branches
git stash	Temporarily save changes
git reset	Undo commits
git revert	Reverse commit safely




