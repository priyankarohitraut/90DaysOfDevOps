
Task 1: Git Merge — Hands-On

Create a new branch feature-login from main, add a couple of commits to it
Switch back to main and merge feature-login into main
Observe the merge — did Git do a fast-forward merge or a merge commit?
Now create another branch feature-signup, add commits to it — but also add a commit to main before merging
Merge feature-signup into main — what happens this time?
Answer in your notes:
What is a fast-forward merge?
When does Git create a merge commit instead?
What is a merge conflict? (try creating one intentionally by editing the same line in both branches)
Task 1: Git Merge — Hands-On
1️⃣ Create a new branch feature-login

Make sure you are on main (or master depending on your repo).

git branch

Switch to main if needed:

git checkout main

Create a new branch:

git checkout -b feature-login
2️⃣ Add a couple commits in this branch

Edit your file:

nano git-commands.md

Add something like:

git branch
git checkout
git merge

Save the file.

Check changes:

git status

Stage and commit:

git add git-commands.md
git commit -m "Add branch commands"

Make another commit:

nano git-commands.md

Add more commands:

git pull
git push

Commit again:

git add .
git commit -m "Add remote commands"
3️⃣ Switch back to main
git checkout main

Merge the branch:

git merge feature-login
What happens?

Git will most likely show:

Fast-forward

Example:

Updating a1b2c3d..d4e5f6
Fast-forward

This means no merge commit was created.

4️⃣ Create another branch feature-signup
git checkout -b feature-signup

Add changes:

nano git-commands.md

Add:

git stash
git stash pop

Commit:

git add .
git commit -m "Add stash commands"
5️⃣ Add a commit to main BEFORE merging

Switch to main:

git checkout main

Edit the file:

nano git-commands.md

Add:

git tag

Commit:

git add .
git commit -m "Add tag command"
6️⃣ Merge feature-signup

Now run:

git merge feature-signup

Now Git will create something like:

Merge made by the 'ort' strategy.

This means a merge commit was created.

Check history:

git log --oneline --graph
7️⃣ Create a Merge Conflict (Important DevOps Skill)

Create a branch:

git checkout -b conflict-test

Edit a line in the file.

Example change:

git pull origin main

Commit:

git add .
git commit -m "Update pull command"

Go back to main:

git checkout main

Edit the same line but differently:

git pull origin master

Commit:

git add .
git commit -m "Modify pull command"

Now merge:

git merge conflict-test

Git will show:

CONFLICT (content): Merge conflict in git-commands.md

Fix the file manually, then:

git add git-commands.md
git commit -m "Resolve merge conflict"
Answers for day-22-notes.md

You can paste this.

What is a fast-forward merge?

A fast-forward merge happens when the main branch has no new commits after a feature branch is created. Git simply moves the main branch pointer forward to the latest commit of the feature branch without creating a new merge commit.

When does Git create a merge commit instead?

Git creates a merge commit when the main branch and the feature branch have different commits. Git must combine the histories of both branches, so it creates a new commit called a merge commit.

What is a merge conflict?

A merge conflict occurs when Git cannot automatically merge changes because the same line of a file was modified in two different branches. Git stops the merge and asks the developer to manually choose which change to keep.

Task 2: Git Rebase — Hands-On

Create a branch feature-dashboard from main, add 2-3 commits
While on main, add a new commit (so main moves ahead)
Switch to feature-dashboard and rebase it onto main
Observe your git log --oneline --graph --all — how does the history look compared to a merge?
Answer in your notes:
What does rebase actually do to your commits?
How is the history different from a merge?
Why should you never rebase commits that have been pushed and shared with others?
When would you use rebase vs merge?
Task 2: Git Rebase — Hands-On
1️⃣ Create a new branch from master/main

Your repo uses master, so run:

git checkout master
git checkout -b feature-dashboard
2️⃣ Add 2–3 commits to this branch

Edit your file:

nano git-commands.md

Add something like:

git rebase
git rebase --continue

Save and commit:

git add .
git commit -m "Add rebase commands"

Add another commit:

nano git-commands.md

Add:

git rebase --abort

Commit again:

git add .
git commit -m "Add rebase abort command"
3️⃣ Move the main branch ahead

Switch to master:

git checkout master

Edit file again:

nano git-commands.md

Add:

git cherry-pick

Commit:

git add .
git commit -m "Add cherry-pick command"

Now master is ahead of your feature branch.

4️⃣ Rebase feature branch onto master

Switch back:

git checkout feature-dashboard

Run rebase:

git rebase master

Git will replay your commits on top of master.

If there is a conflict:

fix file → git add file → git rebase --continue
5️⃣ Observe the history

Run:

git log --oneline --graph --all
Example History with Merge
*   Merge branch 'feature-dashboard'
|\
| * commit B
| * commit A
* | commit from master

History looks like a branch tree.

Example History with Rebase
* commit B
* commit A
* commit from master

History becomes clean and linear.

Answers for Notes
What does rebase actually do to your commits?

Rebase moves your branch commits to a new base commit.
Git takes the commits from your branch and replays them on top of another branch (usually main or master).

How is the history different from a merge?

With merge:

Git creates a merge commit

History looks like a branch graph

With rebase:

No merge commit is created

History becomes linear and cleaner

Why should you never rebase commits that have been pushed and shared with others?

Rebasing rewrites commit history.
If other developers already pulled those commits, rebasing will change the commit IDs and cause serious conflicts and broken history.

This can disrupt collaboration and force others to manually fix their repositories.

When would you use rebase vs merge?

Use rebase:

When cleaning up your local branch before merging

To maintain a clean, linear history

For personal/local branches

Use merge:

When working with shared branches

When preserving the true history of a project

For integrating feature branches safely

Quick DevOps Tip

Most teams use this workflow:

feature branch
      ↓
git rebase main
      ↓
git merge main

This keeps history clean but safe for teams.

Task 3: Squash Commit vs Merge Commit

Create a branch feature-profile, add 4-5 small commits (typo fix, formatting, etc.)
Merge it into main using --squash — what happens?
Check git log — how many commits were added to main?
Now create another branch feature-settings, add a few commits
Merge it into main without --squash (regular merge) — compare the history
Answer in your notes:
What does squash merging do?
When would you use squash merge vs regular merge?
What is the trade-off of squashing?
Task 3: Squash Commit vs Merge Commit
1️⃣ Create a branch feature-profile

First switch to your main branch (likely main or master).

Check branches:

git branch

Then create the branch:

git checkout -b feature-profile
2️⃣ Add 4–5 small commits

Edit your file several times.

Example workflow:

Commit 1
nano git-commands.md

Make a small change (typo fix).

git add .
git commit -m "Fix typo in commands"
Commit 2
nano git-commands.md

Formatting change.

git add .
git commit -m "Improve formatting"
Commit 3
git add .
git commit -m "Update profile section"
Commit 4
git add .
git commit -m "Add profile command example"

Now your branch has multiple small commits.

3️⃣ Squash merge into main

Switch back:

git checkout main

(or master if your repo uses that)

Now merge with squash:

git merge --squash feature-profile

Then commit:

git commit -m "Add feature profile"
What happened?

Git combined all commits into ONE commit before adding them to main.

4️⃣ Check history
git log --oneline

You will see only one new commit added to main.

Example:

f4a1c2 Add feature profile

Even though the branch had 4–5 commits.

5️⃣ Create another branch feature-settings
git checkout -b feature-settings

Add a few commits again:

commit 1
commit 2
commit 3
6️⃣ Merge normally (no squash)

Switch to main:

git checkout main

Merge normally:

git merge feature-settings
Check history
git log --oneline --graph

Now you will see all commits preserved.

Example:

* commit3
* commit2
* commit1
* Merge branch 'feature-settings'
Answers for Your Notes
What does squash merging do?

A squash merge combines all commits from a feature branch into a single commit before merging into the main branch.

This keeps the commit history clean by removing small or unnecessary commits.

When would you use squash merge vs regular merge?
Use squash merge when

A feature branch contains many small commits

You want a clean project history

The commits are not meaningful individually

Example:

fix typo
fix spacing
fix indentation

These can become one commit.

Use regular merge when

Each commit has important history

You want to preserve the development process

Multiple developers worked on the branch

What is the trade-off of squashing?

The main trade-off is loss of detailed commit history.

Advantages:

Cleaner Git history

Easier to read logs

Disadvantages:

Individual commits are lost

Harder to track how the feature evolved

✅ Summary

Merge Type	History	Use Case
Squash Merge	Single commit	Clean history
Regular Merge	All commits preserved	Full development history

Task 4: Git Stash — Hands-On

Start making changes to a file but do not commit
Now imagine you need to urgently switch to another branch — try switching. What happens?
Use git stash to save your work-in-progress
Switch to another branch, do some work, switch back
Apply your stashed changes using git stash pop
Try stashing multiple times and list all stashes
Try applying a specific stash from the list
Answer in your notes:
What is the difference between git stash pop and git stash apply?
When would you use stash in a real-world workflow?
Task 4: Git Stash — Hands-On
1️⃣ Start making changes but do NOT commit

Edit your file:

nano git-commands.md

Add something like:

git stash
git stash pop
git stash list

Check status:

git status

You should see:

modified: git-commands.md
2️⃣ Try switching branches

Now try:

git checkout feature-login

Sometimes Git allows switching, but if the changes conflict you will see a message like:

Please commit your changes or stash them before you switch branches
3️⃣ Save work using stash

Run:

git stash

Output example:

Saved working directory and index state WIP on main

Now check:

git status

Your working directory becomes clean.

4️⃣ Switch to another branch
git checkout feature-login

Make a small change:

nano git-commands.md

Commit it:

git add .
git commit -m "Update login feature"
5️⃣ Return to your previous branch
git checkout main
6️⃣ Restore stashed changes
git stash pop

Your previous uncommitted changes will return.

7️⃣ Create multiple stashes

Make another change:

nano git-commands.md

Then stash again:

git stash

Do it again for practice.

8️⃣ List all stashes
git stash list

Example:

stash@{0}: WIP on main
stash@{1}: WIP on feature-login
stash@{2}: WIP on main
9️⃣ Apply a specific stash

Example:

git stash apply stash@{1}

This applies that stash without removing it.

Answers for Your Notes
What is the difference between git stash pop and git stash apply?

git stash pop

Applies the stashed changes

Removes the stash from the stash list

Example:

stash@{0} disappears

git stash apply

Applies the stashed changes

Keeps the stash in the stash list

So the stash can be reused later.

When would you use stash in a real-world workflow?

Developers use stash when they need to temporarily save unfinished work without committing it.

Common scenarios:

1️⃣ Urgent bug fix

You are working on a feature but suddenly must fix a production bug.

git stash
git checkout hotfix-branch
2️⃣ Switching branches quickly

If you have unfinished work but must move to another branch.

3️⃣ Testing something quickly

You want to temporarily clear your working directory to test something.

Example Real DevOps Workflow
working on feature
      ↓
urgent bug appears
      ↓
git stash
      ↓
fix bug branch
      ↓
git checkout feature
      ↓
git stash pop


Task 5: Cherry Picking

Create a branch feature-hotfix, make 3 commits with different changes
Switch to main
Cherry-pick only the second commit from feature-hotfix onto main
Verify with git log that only that one commit was applied
Answer in your notes:
What does cherry-pick do?
When would you use cherry-pick in a real project?
What can go wrong with cherry-picking?
Task 5: Cherry Picking
1️⃣ Create a branch feature-hotfix

First switch to your main branch (main or master).

Check branches:

git branch

Then create the new branch:

git checkout -b feature-hotfix
2️⃣ Make 3 commits

Edit your file:

nano git-commands.md
Commit 1

Add something like:

git reset

Commit:

git add .
git commit -m "Add reset command"
Commit 2

Edit again:

git revert

Commit:

git add .
git commit -m "Add revert command"
Commit 3

Edit again:

git clean

Commit:

git add .
git commit -m "Add clean command"
3️⃣ View commits on this branch

Run:

git log --oneline

Example output:

c3f921 Add clean command
a8d210 Add revert command
4b2a11 Add reset command

You will copy the commit ID of the second commit.

Example:

a8d210
4️⃣ Switch back to main
git checkout main

(or master if that is your main branch)

5️⃣ Cherry-pick the second commit

Run:

git cherry-pick a8d210

Git will apply only that commit to your main branch.

6️⃣ Verify the result

Run:

git log --oneline

You should see only the cherry-picked commit added to main, not the other two commits.

Example:

f9c332 Add revert command
Answers for Your Notes
What does cherry-pick do?

git cherry-pick allows you to select a specific commit from one branch and apply it to another branch.

Instead of merging the entire branch, Git copies only the chosen commit.

When would you use cherry-pick in a real project?

Cherry-picking is useful when only one specific change is needed from another branch.

Common real-world cases:

1️⃣ Urgent bug fix

A bug fix exists in a development branch, but production needs it immediately.

Example:

develop branch → bug fix commit
main branch → cherry-pick that commit
2️⃣ Applying a hotfix to multiple branches

Sometimes a fix must be applied to several release branches.

3️⃣ Avoid merging unnecessary commits

If a branch contains many unrelated changes, cherry-pick lets you take only the required one.

What can go wrong with cherry-picking?
1️⃣ Merge conflicts

If the code around the commit is different in the target branch, Git may not apply it automatically.

2️⃣ Duplicate commits

Cherry-picking creates a new commit with a different commit ID, which can cause duplicate changes if the branch is later merged.

3️⃣ Confusing history

Overusing cherry-pick can make the Git history harder to understand.

Simple Summary
Command	Purpose
git cherry-pick <commit-id>	Copy a specific commit to the current branch


