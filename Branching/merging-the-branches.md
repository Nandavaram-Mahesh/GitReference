# Git Merge – Complete Guide with Examples

## What is `git merge`?

`git merge` combines changes from one branch into another. It is one of the most commonly used Git commands.

---

# Basic Idea

Suppose you have the following branches:

```text
main
  A---B---C

feature
       \
        D---E
```

- `main` contains stable code.
- `feature` contains your new work.

When the feature is complete, merge it into `main`.

```bash
git checkout main
git merge feature
```

Result:

```text
main
  A---B---C-------M
       \         /
        D-------E

feature
       \
        D---E
```

`M` is called a **Merge Commit**.

---

# Typical Git Merge Workflow

```bash
# Create a feature branch
git checkout -b feature/login

# Work on your feature
git add .
git commit -m "Add login page"

# Switch back to main
git checkout main

# Get the latest changes
git pull

# Merge the feature branch
git merge feature/login

# Delete the merged branch
git branch -d feature/login

# Push changes to remote
git push
```
---

# Example 1: Normal Merge

## Step 1: Initialize a Repository

```bash
git init
```

Create a file named `app.txt`

```text
Hello
```

Commit it.

```bash
git add .
git commit -m "Initial commit"
```

---

## Step 2: Create a Feature Branch

```bash
git checkout -b feature
```

Modify `app.txt`

```text
Hello
Feature added
```

Commit the changes.

```bash
git add .
git commit -m "Add feature"
```

---

## Step 3: Switch Back to Main

```bash
git checkout main
```

Merge the feature branch.

```bash
git merge feature
```

Output:

```text
Updating abc123..def456
Fast-forward
```

Now the `main` branch contains the feature changes.

---

# Example 2: Fast-Forward Merge

If the `main` branch has not changed after creating the feature branch, Git performs a **Fast-Forward Merge**.

Before merging:

```text
A---B  main
     \
      C---D feature
```

After merging:

```text
A---B---C---D main
              feature
```

Notice that there is **no merge commit**.

Command:

```bash
git checkout main
git merge feature
```

Output:

```text
Fast-forward
```

Git simply moves the `main` pointer forward.

---

# Example 3: Three-Way Merge

Suppose both branches have new commits.

Before merging:

```text
A---B---C main
     \
      D---E feature
```

Merge:

```bash
git checkout main
git merge feature
```

Result:

```text
A---B---C------M main
     \        /
      D------E feature
```

Since both branches contain unique commits, Git creates a **Merge Commit**.

---

# Example 4: Merge Conflict

Suppose both branches edit the same line.

On `main`

```text
Hello World
```

On `feature`

```text
Hello Git
```

Merge:

```bash
git merge feature
```

Git displays:

```text
CONFLICT (content): Merge conflict in app.txt
Automatic merge failed.
```

Open the file:

```text
<<<<<<< HEAD
Hello World
=======
Hello Git
>>>>>>> feature
```

Explanation:

- `<<<<<<< HEAD` → Current branch (`main`)
- `=======` → Separator
- `>>>>>>> feature` → Incoming branch (`feature`)

Resolve the conflict manually.

Example:

```text
Hello World and Git
```

Then stage and commit.

```bash
git add app.txt
git commit
```

The merge is now complete.

---

# Useful Merge Commands

Merge another branch:

```bash
git merge feature
```

Abort a merge:

```bash
git merge --abort
```

View commit history:

```bash
git log --oneline --graph --all
```

Delete a merged branch:

```bash
git branch -d feature
```

Force Git to create a merge commit even when a fast-forward is possible:

```bash
git merge --no-ff feature
```

---

# Visual Workflow

## Step 1

```text
A---B main
     \
      C feature
```

## Step 2

```text
A---B---D main
     \
      C---E feature
```

## Step 3

```text
A---B---D------M main
     \        /
      C------E feature
```

---

# Merge vs Rebase

| Merge | Rebase |
|--------|--------|
| Preserves branch history | Rewrites commit history |
| Creates a merge commit (when needed) | Creates a linear history |
| Safe for shared branches | Best before sharing commits |
| Shows exactly where branches joined | Makes history cleaner |

---

# Best Practices

- Commit your work before merging.
- Pull the latest changes before merging.

```bash
git checkout main
git pull
```

- Resolve merge conflicts carefully.
- Test your application after merging.
- Delete feature branches after they are merged.

---



---

# Summary

- `git merge` combines one branch into another.
- **Fast-Forward Merge** happens when the target branch has no new commits.
- **Three-Way Merge** happens when both branches have new commits.
- **Merge Conflicts** occur when Git cannot automatically combine changes.
- Resolve conflicts manually, then `git add` and `git commit`.
- Use `git merge --abort` to cancel a conflicted merge.
- Use `git log --oneline --graph --all` to visualize branch history.

Following these practices will help you safely integrate features and collaborate effectively using Git.