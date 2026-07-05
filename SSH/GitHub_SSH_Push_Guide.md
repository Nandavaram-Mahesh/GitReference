# Push a Local Git Project to GitHub Using SSH

## 1. Go to your project

``` bash
cd /path/to/your/project
```

If needed:

``` bash
git init
```

## 2. Commit your project

``` bash
git add .
git commit -m "Initial commit"
```

## 3. Create an empty GitHub repository

Create a new repository on GitHub without adding a README, `.gitignore`,
or license.

## 4. Add the SSH remote

Replace with your repository:

``` bash
git remote add origin git@github.com:YOUR_USERNAME/YOUR_REPO.git

-Example
git remote add origin git@github-personal:Nandavaram-Mahesh/SSH.git 
```

Verify:

``` bash
git remote -v
```

Expected:

``` text
origin  git@github.com:YOUR_USERNAME/YOUR_REPO.git (fetch)
origin  git@github.com:YOUR_USERNAME/YOUR_REPO.git (push)
```

## 5. Push to GitHub

For `main`:

``` bash
git branch -M main
git push -u origin main
```

For `master`:

``` bash
git push -u origin master
```
