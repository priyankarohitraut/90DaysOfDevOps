
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


Task 2: Git Revert — Hands-On
Make 3 commits (commit X, Y, Z)
Revert commit Y (the middle one) — what happens?
Check git log — is commit Y still in the history?
Answer in your notes:
How is git revert different from git reset?
Why is revert considered safer than reset for shared branches?
When would you use revert vs reset?
Task 3: Reset vs Revert — Summary
Create a comparison in your notes:

git reset	git revert
What it does	?	?
Removes commit from history?	?	?
Safe for shared/pushed branches?	?	?
When to use	?	?
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
Task 5: Git Commands Reference Update
Update your git-commands.md to cover everything from Days 22–25:

Setup & Config
Basic Workflow (add, commit, status, log, diff)
Branching (branch, checkout, switch)
Remote (push, pull, fetch, clone, fork)
Merging & Rebasing
Stash & Cherry Pick
Reset & Revert
Hints
git reflog is your safety net — it shows everything Git has done, even after a hard reset
For branching strategies, look at how projects like Kubernetes, React, or Linux kernel manage branches
Submission
Add your day-25-notes.md to 2026/day-25/
Update git-commands.md — commit and push
Push to your fork
Learn in Public
Share your Reset vs Revert comparison or your branching strategy notes on LinkedIn.

#90DaysOfDevOps #DevOpsKaJosh #TrainWithShubham

Happy Learning! TrainWithShubham

