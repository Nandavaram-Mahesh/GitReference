# Git Fork Sync Cheat Sheet

## Remotes

-   **origin** = your fork
-   **upstream** = original repository

```{=html}
<!-- -->
```
    Original Repo ----> upstream
            ^
            |
          Fork
            ^
            |
         origin

## One-time setup

``` bash
git remote -v
git remote add upstream https://github.com/ORIGINAL_OWNER/REPO.git
git remote -v
```

## Daily sync flow

``` text
Checkout main
    ↓
Fetch upstream
    ↓
Merge/Rebase upstream/main
    ↓
Push to origin
```

``` bash
git switch main
git fetch upstream       # 
git merge upstream/main   # or: git rebase upstream/main    Merging changes from Original Owner Fork Repo to your local fork repo 
git push origin main      # Pushing your local merged changes to your forked repo
```

## Branch commands

``` bash
git branch
git switch branch-name
git switch -c new-branch
```

## Pull latest

``` bash
git pull origin main
git fetch origin
```

## Quick reference

  Task              Command
  ----------------- ---------------------------------
  Show remotes      `git remote -v`
  Add upstream      `git remote add upstream <url>`
  Fetch upstream    `git fetch upstream`
  Merge upstream    `git merge upstream/main`
  Rebase upstream   `git rebase upstream/main`
  Push fork         `git push origin main`
  Switch branch     `git switch <branch>`
  Create branch     `git switch -c <branch>`
