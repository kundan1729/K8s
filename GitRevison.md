
---

# 🧠 Git Cheat Sheet

A professional and practical reference for working with Git on local and remote repositories.

---

## 📁 Repository Setup

### ✅ Initialize a Local Git Repository

```bash
git init
```

### ✅ Clone a Repository

```bash
git clone <repo-url>
```

---

## 💾 Working with Files

### ✅ Check File Status

```bash
git status
```

### ✅ Stage Files for Commit

```bash
git add <file>          # Stage a specific file
git add .               # Stage all changes
```

### ✅ Commit Changes

```bash
git commit -m "Your commit message"
```

---

## 🌿 Branching

### ✅ Create a New Branch

```bash
git branch <branch-name>
```

### ✅ Switch to an Existing Branch

```bash
git checkout <branch-name>
```

### ✅ Create and Switch to a New Branch (Combined)

```bash
git checkout -b <branch-name>
```

### ✅ Rename Current Branch to `main`

```bash
git branch -M main
```

* `-M`: Force rename (even if `main` already exists)

---

## 🔄 Synchronizing with Remote

### ✅ Add a Remote Repository

```bash
git remote add origin https://github.com/yourusername/repo.git
```

---

### ✅ Push Code to Remote

```bash
git push -u origin main
```

#### 🔍 Why use `-u` (or `--set-upstream`)?

The `-u` flag tells Git to **link your local branch** with the corresponding **remote branch**. This means:

* Future `git push` and `git pull` commands **will automatically know which remote and branch to use**.
* You **don’t have to specify** the remote (`origin`) and branch (`main`) every time.

#### ❌ What if you *don’t* use `-u`?

If you run:

```bash
git push origin main
```

without `-u`, then:

* Git **does push the changes** to the remote branch.
* But **your local branch is NOT linked to the remote branch**.
* This means future pushes or pulls **require you to specify the remote and branch explicitly**, e.g.:

```bash
git push origin main
git pull origin main
```

* You lose the convenience of just running `git push` or `git pull` without arguments.

---

### ✅ Push Changes (after initial setup)

```bash
git push
```

### ✅ Pull Updates from Remote

```bash
git pull
```

---

## 🔁 Checking & Switching Branches

### ✅ List All Branches

```bash
git branch
```

### ✅ See Current Branch

```bash
git branch --show-current
```

### ✅ View Remote Branches

```bash
git branch -r
```

---

## 🧪 Viewing History and Changes

### ✅ View Commit History

```bash
git log
```

### ✅ One-line Commit Summary

```bash
git log --oneline
```

### ✅ View Changes (unstaged)

```bash
git diff
```

---

## 🧼 Undoing & Resetting

### ✅ Unstage a File

```bash
git reset <file>
```

### ✅ Revert Last Commit (keep changes)

```bash
git reset --soft HEAD~1
```

### ✅ Undo Last Commit (delete changes too)

```bash
git reset --hard HEAD~1
```

---

## ⚙️ Configuration

### ✅ Set Global Username & Email

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### ✅ Check Current Configuration

```bash
git config --list
```

---

## 🚀 Typical First-Time Push Workflow

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main                     # Rename branch to main (if needed)
git remote add origin <repo-url>      # Link to GitHub
git push -u origin main               # Push + set upstream
```

---

## 📂 .gitignore Example

```gitignore
node_modules/
.env
*.log
*.swp
.DS_Store
.idea/
.vscode/
```

---

## 🧾 Quick Command Reference

| Action                 | Command                       |
| ---------------------- | ----------------------------- |
| Init repo              | `git init`                    |
| Stage all changes      | `git add .`                   |
| Commit                 | `git commit -m "msg"`         |
| Rename branch          | `git branch -M main`          |
| Add remote             | `git remote add origin <url>` |
| Push with upstream     | `git push -u origin main`     |
| Push after setup       | `git push`                    |
| Switch branches        | `git checkout <branch>`       |
| Create & switch branch | `git checkout -b <branch>`    |
| See current branch     | `git branch --show-current`   |

---

---


# Git Branching, Merging, Stashing & Conflict Resolution Guide

A comprehensive guide to managing branches, merging changes, stashing work, and handling conflicts in Git.

---

## 🌿 Branching

Branching allows you to create separate lines of development to work on features, bug fixes, or experiments independently from the main codebase.

### Create a New Branch

```bash
git branch <branch-name>
```

### Switch to a Branch

```bash
git checkout <branch-name>
```

### Create and Switch to a New Branch

```bash
git checkout -b <branch-name>
```

### List All Branches

```bash
git branch
```

* `*` indicates the current branch.

### Delete a Branch (locally)

```bash
git branch -d <branch-name>
```

* Use `-D` to force delete if the branch hasn’t been merged.

---

## 🔀 Merging

Merging integrates changes from one branch into another.

### Merge a Branch into Current Branch

```bash
git merge <branch-name>
```

* Typically, you merge feature branches into `main` or `develop`.

### Fast-Forward Merge

If your current branch is behind and there are no divergent commits, Git performs a fast-forward merge (no new commit created).

### Merge Commit

If there are divergent commits, Git creates a **merge commit** that ties histories together.

---

## ⚠️ Merge Conflicts

Merge conflicts happen when changes in two branches affect the same lines of the same files, and Git can’t automatically resolve which change to keep.

### How to Identify Conflicts

* Git will stop the merge and show conflict markers in the files.
* Running `git status` will show files with conflicts as **“both modified”**.

### Conflict Markers in Files

```diff
<<<<<<< HEAD
Current branch changes
=======
Incoming branch changes
>>>>>>> branch-to-merge
```

### Steps to Resolve Conflicts

1. **Open conflicted files** and locate conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`).
2. **Edit the file** to combine or choose the correct code.
3. **Remove the conflict markers** after editing.
4. **Stage the resolved files**:

```bash
git add <resolved-file>
```

5. **Complete the merge** by committing:

```bash
git commit
```

* If you want to skip the merge commit message editor, use:

```bash
git commit --no-edit
```

---

## 🛑 Aborting a Merge

If you want to stop the merge and revert to the pre-merge state:

```bash
git merge --abort
```

---

## 🧳 Stashing

Stashing temporarily saves your uncommitted changes so you can switch branches or perform other operations without committing incomplete work.

### Save Changes to a New Stash

```bash
git stash
```

### List Stashes

```bash
git stash list
```

### Apply the Latest Stash (keep stash saved)

```bash
git stash apply
```

### Apply and Remove the Latest Stash

```bash
git stash pop
```

### Drop a Specific Stash

```bash
git stash drop stash@{index}
```

### Clear All Stashes

```bash
git stash clear
```

---

## ⚠️ Stash Conflicts

Applying a stash can also cause conflicts if the changes clash with the current working tree.

* Conflict markers appear as in merges.
* Resolve conflicts manually.
* After resolving, stage and commit changes as usual.

---

## 🔄 Resolving Conflicts — Summary Workflow

| Step                       | Command/Action                                   |
| -------------------------- | ------------------------------------------------ |
| Check conflicted files     | `git status`                                     |
| Open conflicted files      | Look for `<<<<<<<`, `=======`, `>>>>>>>` markers |
| Edit and resolve conflicts | Manually edit code                               |
| Stage resolved files       | `git add <file>`                                 |
| Complete merge or stash    | `git commit` (for merge) or continue work        |
| Abort merge (if needed)    | `git merge --abort`                              |

---

## 💡 Tips for Avoiding Conflicts

* Pull changes frequently: `git pull`
* Keep branches focused and small
* Communicate with your team
* Use rebasing (`git rebase`) carefully to keep history linear

---

## 🔧 Bonus: Rebase (Optional)

Rebase reapplies commits on top of another branch.

```bash
git checkout feature
git rebase main
```

* Useful for cleaner history.
* Conflicts may occur during rebase and must be resolved similarly.

Abort rebase if needed:

```bash
git rebase --abort
```

---

# Summary

| Topic             | Commands / Concepts                             |
| ----------------- | ----------------------------------------------- |
| Create branch     | `git branch <name>`, `git checkout -b <name>`   |
| Switch branch     | `git checkout <name>`                           |
| Merge branch      | `git merge <branch>`                            |
| Resolve conflicts | Edit files → `git add <file>` → `git commit`    |
| Abort merge       | `git merge --abort`                             |
| Stash changes     | `git stash`, `git stash apply`, `git stash pop` |
| List stashes      | `git stash list`                                |

---


