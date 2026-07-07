
# Step 1: Create a GitHub repository

Create a new repository.

Example:

```
Repository name: gitflow-practice
```

Initialize it with:

* ✅ README
* ✅ .gitignore (choose any, e.g. Node or Python)
* ✅ License (optional)

---

# Step 2: Clone it

```bash
git clone https://github.com/<your-username>/gitflow-practice.git

cd gitflow-practice
```

Verify:

```bash
git branch
```

Output:

```text
* main
```

---

# Step 3: Create the `develop` branch

```bash
git checkout -b develop
```

Push it:

```bash
git push -u origin develop
```

Now your repository has:

```
main
develop
```

This is the foundation of GitFlow.

---

# Step 4: Make `develop` the default branch (optional but recommended)

On GitHub:

```
Settings
    ↓
Default branch
    ↓
Change from main → develop
```

This makes new pull requests target `develop` by default.

---

# Step 5: Verify

Run:

```bash
git branch -a
```

You should see something like:

```text
* develop
  main
  remotes/origin/develop
  remotes/origin/main
```

---


Instead of isolated examples, let's simulate a complete product lifecycle. By the end, you'll have used **every GitFlow branch** exactly as a team would.

## Current state

```text
main
│
└── develop
      │
      └── feature/login   ✅ merged
```

Now we'll continue.

---

# Phase 1: Add another feature

Create another feature branch from `develop`.

```bash
git checkout develop
git pull
git checkout -b feature/signup
```

Create a file:

```text
signup.txt
```

Commit it:

```bash
git add .
git commit -m "Add signup feature"
git push -u origin feature/signup
```

Open a PR:

```text
feature/signup
        ↓
     develop
```

Merge it.

Now your history is:

```text
main

develop
├── Login
└── Signup
```

At this point, the next version is ready.

---

# Phase 2: Create a Release branch

This is where GitFlow becomes different from GitHub Flow.

Create:

```bash
git checkout develop
git pull
git checkout -b release/1.0.0
```

Push it:

```bash
git push -u origin release/1.0.0
```

Your branches now are:

```text
main

develop

release/1.0.0
```

### Why?

The release branch is for **stabilizing** the release.

Only things like:

* bug fixes
* documentation
* version number changes

are allowed here.

No new features.

---

# Phase 3: Simulate a release fix

Edit `README.md`.

Add:

```text
Version 1.0.0
```

Commit:

```bash
git add .
git commit -m "Prepare release 1.0.0"
git push
```

---

# Phase 4: Merge Release → Main

Open a PR:

```text
release/1.0.0
        ↓
       main
```

Merge it.

Congratulations 🎉

Your application is now "live".

---

# Phase 5: Tag the release

```bash
git checkout main
git pull

git tag v1.0.0
git push origin v1.0.0
```

Now GitHub will show:

```text
Releases

v1.0.0
```

This marks the exact commit that was deployed.

---

# Phase 6: Merge Release back into Develop

Why?

Because you fixed documentation/version numbers in the release branch.

Those fixes should also exist in future development.

Open another PR:

```text
release/1.0.0
        ↓
     develop
```

Merge it.

Then delete:

```text
release/1.0.0
```

Current state:

```text
main
│
├── v1.0.0
│
develop
```

---

# Phase 7: Production bug! 🚨

A customer reports:

> Login button crashes.

You **do not** fix it on `develop`.

Instead:

```bash
git checkout main
git pull
git checkout -b hotfix/1.0.1
```

Notice:

**Hotfix starts from `main`, not `develop`.**

---

# Phase 8: Fix the bug

Create a file:

```text
hotfix.txt
```

Write:

```text
Fixed login crash
```

Commit:

```bash
git add .
git commit -m "Fix login crash"
git push -u origin hotfix/1.0.1
```

---

# Phase 9: Merge Hotfix → Main

Open PR:

```text
hotfix/1.0.1
        ↓
       main
```

Merge.

Tag it:

```bash
git checkout main
git pull

git tag v1.0.1
git push origin v1.0.1
```

Production now runs version **1.0.1**.

---

# Phase 10: Merge Hotfix → Develop

The bug fix must also exist in future versions.

Open another PR:

```text
hotfix/1.0.1
        ↓
     develop
```

Merge.

Delete the hotfix branch.

---

# Final GitFlow picture

```text
                         feature/login
                              │
                              ▼
main ────────────────────────────────●──────────────●
                                     ▲              ▲
                                     │              │
                             release/1.0.0   hotfix/1.0.1
                                     ▲              ▲
                                     │              │
develop ─────●────────●──────────────●──────────────●
             ▲        ▲
             │        │
      feature/login  feature/signup
```

## GitFlow rules you'll remember

| Branch      | Created from | Merged into              | Purpose                 |
| ----------- | ------------ | ------------------------ | ----------------------- |
| `feature/*` | `develop`    | `develop`                | New features            |
| `release/*` | `develop`    | `main` **and** `develop` | Prepare a release       |
| `hotfix/*`  | `main`       | `main` **and** `develop` | Urgent production fixes |
| `develop`   | —            | `release/*`              | Next version            |
| `main`      | —            | Production               | Live code               |

## One more thing worth practicing

After you're comfortable with this flow, I recommend one final exercise: create **two feature branches at the same time** (for example, `feature/profile` and `feature/settings`), make conflicting edits to the same file, and merge them. That will teach you **merge conflicts**, which are one of the most important real-world Git skills. Once you've mastered that, you'll have experienced nearly every part of GitFlow in practice.
