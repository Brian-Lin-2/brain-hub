# Git

Git is a version control system that let's you track changes in your code over time so you can manage, collaborate, and revert safely.

## Overview

![Git Workflow](assets/git/workflow.png)

Git mainly consists of your local environment and the remote upstream environment. The source of truth comes from the remote repo (upstream) that your local repo will sync with.

**Local Repository:**

`Working Directory` - This is where you code and make changes <br>
`Staging Area` - This is where you select code to be committed <br>
`Local Repository` - This is where you keep your locally committed code <br>

> Typically you write your code in branches and then merge into main through a PR request

> There's also a remote branch (local branch that tracks the upstream). When you fetch code from the upstream, it first populates into this where you can then merge into your local

## Resources

- [Tutorial](https://learngitbranching.js.org)

## Useful Commands

**Basic Workflow:**

1. `git add .`
2. `git commit -m "<your message>"`
3. `git push -u origin <branch>` / `git push`

**New Branch:**

1. `git checkout -b <branch>`

**Rebasing:**

1. `git checkout main`
2. `git pull`
3. `git checkout <branch>`
4. `git rebase main`
5. `git push --force-with-lease`

**Squash**

1. `git checkout main`
2. `git pull`
3. `git checkout <branch>`
4. `git rebase -i main`
5. `git push --force-with-lease`

**Adding to Previous Commit:**

1. `git add .`
2. `git commit --amend --no-edit`
3. `git push --force-with-lease`

**Reset Branch to Old Commit:**

1. `git reset --hard <commit-hash>`
2. `git push --force-with-lease`

**Clear Working Directory:**

1. `git clean -fd`

**Clear Staging Area:**

1. `git reset --hard`

**Cherrypick:**

1. `git cherrypick <commit-hash>`
