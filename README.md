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

# Git Reset

## What is Git Reset?

`git reset` is used to move the current branch (`HEAD`) to a specified commit. Depending on the option used, it can also modify the staging area (index) and working directory.

---

## Types of Git Reset

### 1. Soft Reset

Moves `HEAD` to a previous commit but keeps all changes in the staging area.

```bash
git reset --soft HEAD~1
```

### Example

Before:

```text
A --> B --> C (HEAD)
```

After:

```bash
git reset --soft HEAD~1
```

Result:

```text
A --> B (HEAD)
```

Changes from commit `C` remain staged and ready to commit again.

Check:

```bash
git status
```

Output:

```text
Changes to be committed
```

---

### 2. Mixed Reset (Default)

Moves `HEAD` and removes changes from the staging area, but keeps them in the working directory.

```bash
git reset --mixed HEAD~1
```

or

```bash
git reset HEAD~1
```

Result:

```text
A --> B (HEAD)
```

Changes from commit `C` remain in files but are not staged.

Check:

```bash
git status
```

Output:

```text
Changes not staged for commit
```

---

### 3. Hard Reset

Moves `HEAD`, clears the staging area, and updates the working directory to match the target commit.

```bash
git reset --hard HEAD~1
```

Result:

```text
A --> B (HEAD)
```

All changes introduced by commit `C` are removed from the working directory.

⚠️ Use with caution because uncommitted work can be lost.

---

## Practical Example

Commit History:

```text
3b127f4 soft reset3 is done
fa0d80d soft reset2 is done
2b9150d soft reset is done
6974566 fifth commit
```

Soft Reset:

```bash
git reset --soft HEAD~2
```

New History:

```text
2b9150d (HEAD)
6974566 fifth commit
```

Changes from commits:

```text
3b127f4
fa0d80d
```

remain staged and can be committed again.

---

## Hard Reset Example

Current History:

```text
506ac00 soft commit3
2b9150d soft reset is done
6974566 fifth commit
```

Reset:

```bash
git reset --hard 6974566
```

Result:

```text
6974566 (HEAD) fifth commit
32377b3 modified
9b9b674 first commit
```

The commits above `6974566` are removed from branch history.

---

## Recovering After Hard Reset

Git stores previous HEAD locations in the reflog.

View reflog:

```bash
git reflog
```

Example:

```text
6974566 HEAD@{0}: reset: moving to 6974566
506ac00 HEAD@{1}: commit: soft commit3
```

Restore the branch:

```bash
git reset --hard 506ac00
```

or

```bash
git reset --hard HEAD@{1}
```

This restores the branch to the commit that existed before the hard reset.

---

## Useful Commands

View current HEAD:

```bash
git log --oneline
```

Show exact commit hash:

```bash
git rev-parse HEAD
```

Show branch and commit:

```bash
git branch -v
```

View HEAD history:

```bash
git reflog
```

---

## Interview Questions

### Difference Between Soft, Mixed, and Hard Reset

| Command   | HEAD  | Staging Area | Working Directory |
| --------- | ----- | ------------ | ----------------- |
| `--soft`  | Moved | Preserved    | Preserved         |
| `--mixed` | Moved | Cleared      | Preserved         |
| `--hard`  | Moved | Cleared      | Cleared           |

### Difference Between Reset and Revert

| Git Reset                       | Git Revert               |
| ------------------------------- | ------------------------ |
| Rewrites history                | Creates a new commit     |
| Can remove commits from history | Preserves history        |
| Commonly used on local branches | Safe for shared branches |
| Can recover using reflog        | No history rewriting     |

### Mixed Reset Example
Before:
```text
A --> B --> C (HEAD)
```
Command:
```bash
git reset --mixed HEAD~1
```
or
```bash
git reset HEAD~1
```
After:
```text
A --> B (HEAD)
```
Changes from commit `C` are still present in the files but are removed from the staging area.
Check:
```bash
git status
```
Output:
```text
Changes not staged for commit
```
To stage the changes again:
```bash
git add .
```
Verify:
```bash
git status
```
Output:
```text
Changes to be committed
```
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/10552c3c-c5a7-45ad-a3be-2957e7a4605f" />

# Git Stash

## What is Git Stash?

`git stash` temporarily saves your uncommitted changes (tracked files) and restores your working directory to a clean state. It is useful when you need to switch branches or pull changes without committing your current work.

---

## Save Changes to Stash

```bash
git stash
```

Or provide a custom message:

```bash
git stash push -m "working on login feature"
```

---

## View Stashed Changes

```bash
git stash list
```

Example:

```text
stash@{0}: On main: working on login feature
stash@{1}: On feature1: fixing bug
```

---

## Apply Stashed Changes

Apply the latest stash without removing it from the stash list:

```bash
git stash apply
```

Apply a specific stash:

```bash
git stash apply stash@{0}
```

---

## Pop Stashed Changes

Apply the latest stash and remove it from the stash list:

```bash
git stash pop
```

---

## Delete a Specific Stash

```bash
git stash drop stash@{0}
```

---

## Delete All Stashes

```bash
git stash clear
```

---

## Stash Untracked Files

By default, untracked files are not stashed.

```bash
git stash -u
```

or

```bash
git stash --include-untracked
```

---

## Create a Branch from a Stash

```bash
git stash branch new-branch-name
```

This creates a new branch, applies the stash, and removes it from the stash list.

---

## Example

Current Status:

```bash
git status
```

Output:

```text
modified: abc.txt
modified: test.txt
```

Save Changes:

```bash
git stash
```

Output:

```text
Saved working directory and index state
```

Check Status:

```bash
git status
```

Output:

```text
nothing to commit, working tree clean
```

Restore Changes:

```bash
git stash pop
```

Output:

```text
modified: abc.txt
modified: test.txt
```

---

## Interview Questions

### Does git stash save untracked files?

**Answer:** No. Use:

```bash
git stash -u
```

### What is the difference between git stash apply and git stash pop?

| Command | Applies Changes | Removes Stash |
|----------|----------------|---------------|
| `git stash apply` | Yes | No |
| `git stash pop` | Yes | Yes |

### Can you stash a single file?

```bash
git stash push file.txt
```

or

```bash
git stash push -m "file stash" file.txt
```
# Git Revert
## What is Git Revert?
`git revert` is used to undo the changes introduced by a commit by creating a new commit. Unlike `git reset`, it does not remove commits from history.
---
## Revert the Latest Commit
```bash
git revert HEAD
```
Git creates a new commit that reverses the changes made by the latest commit.
---
## Example
### Commit History Before Revert
```bash
git log --oneline
```
Output:
```text
0a7b844 (HEAD -> feature1) revert added
94a99dc hi commited Revert "xxxy"
f35f195 xxxy
329387f abcd
32377b3 modified
9b9b674 first commit
```
### File Content Before Revert
```text
Hi this is sagar
hello how are you?
fine
ok bye
5th line added
one more line added
two more line added
revert added
```
### Revert the Latest Commit
```bash
git revert HEAD
```
Output:
```text
[feature1 5decb08] Revert "revert added"
1 file changed, 1 insertion(+), 2 deletions(-)
```
### Commit History After Revert
```bash
git log --oneline
```
Output:
```text
5decb08 (HEAD -> feature1) Revert "revert added"
0a7b844 revert added
94a99dc hi commited Revert "xxxy"
f35f195 xxxy
329387f abcd
32377b3 modified
9b9b674 first commit
```
### File Content After Revert
```text
Hi this is sagar
hello how are you?
fine
ok bye
5th line added
one more line added
two more line added
last line removed which is added previously
```
---
## Revert a Specific Commit
```bash
git revert <commit-id>
```
Example:
```bash
git revert f35f195
```
---
## Revert Multiple Commits
```bash
git revert HEAD~2..HEAD
```
---
## Difference Between Revert and Reset
| Git Revert | Git Reset |
|------------|-----------|
| Creates a new commit | Moves HEAD |
| Preserves history | Rewrites history |
| Safe for shared branches | Risky on shared branches |
| Can be pushed safely | May require force push |
---
## Interview Questions
### Does git revert delete a commit?
**Answer:** No. It creates a new commit that undoes the changes of the target commit.
### Is git revert safe on shared branches?
**Answer:** Yes. Since history is preserved, it is safe to use on shared branches.
### What happens when you run `git revert HEAD`?
**Answer:** Git creates a new commit that reverses the changes introduced by the latest commit.
