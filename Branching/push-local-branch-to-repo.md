# Git: Create a New Branch and Push to Remote

## Create and switch to a new branch

``` bash
git switch -c my-new-branch

# Or (older syntax)
git checkout -b my-new-branch
```

## Commit your changes

``` bash
git add .
git commit -m "Initial commit on new branch"
```

## Push the branch and set the upstream

``` bash
git push -u origin my-new-branch
```

After this, future pushes and pulls can be done with:

``` bash
git push
git pull
```

## Verify

List local branches:

``` bash
git branch
```

List remote branches:

``` bash
git branch -r
```

See tracking information:

``` bash
git branch -vv
```

See all branches:

``` bash
git branch -a
```

## Quick Summary

``` bash
git switch -c my-new-branch
git push -u origin my-new-branch
```
