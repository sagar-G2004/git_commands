# Git Branch Management
## Creating a Branch
### Create a new branch and switch to it
```bash
git checkout -b feature-branch
```
Or using the newer syntax:
```bash
git switch -c feature-branch
```
### Create a branch without switching
```bash
git branch feature-branch
```
---
## Renaming a Branch
### Rename the current branch
```bash
git branch -m new-branch-name
```
### Rename another branch
```bash
git branch -m old-branch-name new-branch-name
```
---
## Deleting a Branch
### Delete a local branch (safe delete)
```bash
git branch -d branch-name
```
### Force delete a local branch
```bash
git branch -D branch-name
```
### Delete a remote branch
```bash
git push origin --delete branch-name
```
---
## Useful Commands
### List all local branches
```bash
git branch
```
### List local and remote branches
```bash
git branch -a
```
### Check the current branch
```bash
git branch --show-current
```
---
## Interview Question
### Can you delete the branch you are currently working on?
**Answer:** No. Git does not allow you to delete the branch that is currently checked out. First, switch to another branch and then delete it.
```bash
git checkout main
git branch -d feature-branch
```
