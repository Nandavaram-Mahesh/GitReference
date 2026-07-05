# Using Multiple GitHub Accounts with SSH on Windows

This guide explains how to configure multiple GitHub accounts on Windows using SSH so you can work with different repositories without repeatedly signing in and out.

---

## Prerequisites

- Git installed
- OpenSSH installed (comes with Windows 10/11)
- VS Code (optional)

---

# Step 1: Generate SSH Keys

## Personal Account

```bash
ssh-keygen -t ed25519 -C "personal-email@example.com"
```

When prompted for the file name:

```
C:\Users\<USERNAME>\.ssh\id_ed25519_personal
```

---

## Second Account

```bash
ssh-keygen -t ed25519 -C "second-email@example.com"
```

When prompted for the file name:

```
C:\Users\<USERNAME>\.ssh\id_ed25519_second
```

---

# Step 2: Verify the Keys

```powershell
dir $HOME\.ssh
```

Example:

```
id_ed25519_personal
id_ed25519_personal.pub

id_ed25519_second
id_ed25519_second.pub

known_hosts
```

---

# Step 3: Enable the SSH Agent

Check its status:

```powershell
Get-Service ssh-agent
```

Set it to start automatically:

```powershell
Set-Service ssh-agent -StartupType Automatic
```

Start the service:

```powershell
Start-Service ssh-agent
```

Verify:

```powershell
Get-Service ssh-agent
```

Expected:

```
Status   Name
------   ----
Running  ssh-agent
```

---

# Step 4: Add SSH Keys

```powershell
ssh-add $HOME\.ssh\id_ed25519_personal
```

```powershell
ssh-add $HOME\.ssh\id_ed25519_second
```

Verify:

```powershell
ssh-add -l
```

---

# Step 5: Add Public Keys to GitHub

Display the personal public key:

```powershell
type cat ~/.ssh/id_ed25519_personal.pub
```

Copy the output.

On GitHub:

- Settings
- SSH and GPG keys
- New SSH Key
- Paste the key
- Save

Repeat for the second account:

```powershell
type cat ~/.ssh/id_ed25519_second.pub
```

---

# Step 6: Create the SSH Config

Create the file:

```
C:\Users\<USERNAME>\.ssh\config
```

**Important**

The file name must be exactly:

```
config
```

NOT:

```
config.txt
```

Contents:

```text
Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal

Host github-second
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_second
```

---

# Step 7: Test the Configuration

Test the personal account:

```bash
ssh -T git@github-personal
```

Expected:

```
Hi <username>! You've successfully authenticated...
```

Test the second account:

```bash
ssh -T git@github-second
```

Expected:

```
Hi <username>! You've successfully authenticated...
```

---

# Step 8: Change Repository Remote

Check the current remote:

```bash
git remote -v
```

Example:

```
origin https://github.com/username/repository.git
```

---

## Personal Repository

```bash
git remote set-url origin git@github-personal:username/repository.git
```

Example:

```bash
git remote set-url origin git@github-personal:maheshnandavaram-96/CalendlyProject.git
```

---

## Second Account Repository

```bash
git remote set-url origin git@github-second:username/repository.git
```

Example:

```bash
git remote set-url origin git@github-second:Nandavaram-Mahesh/Project.git
```

---

# Step 9: Verify the Remote

```bash
git remote -v
```

Expected:

```
origin git@github-personal:maheshnandavaram-96/CalendlyProject.git
```

or

```
origin git@github-second:Nandavaram-Mahesh/Project.git
```

---

# Step 10: Test Git

```bash
git fetch
```

or

```bash
git push
```

Git should authenticate using the correct SSH key automatically.

---

# Optional: Set Commit Identity Per Repository

For personal repositories:

```bash
git config user.name "Your Name"
git config user.email "personal@example.com"
```

For work repositories:

```bash
git config user.name "Your Work Name"
git config user.email "work@example.com"
```

Verify:

```bash
git config user.name
git config user.email
```

---

# Useful Commands

## List SSH Keys

```powershell
dir $HOME\.ssh
```

## List Loaded Keys

```bash
ssh-add -l
```

## Test Personal Account

```bash
ssh -T git@github-personal
```

## Test Second Account

```bash
ssh -T git@github-second
```

## View Current Remote

```bash
git remote -v
```

## Change Remote

```bash
git remote set-url origin <new-url>
```

---

# Common Issues

## Error

```
ssh: Could not resolve hostname github-personal
```

Cause:

```
config.txt
```

instead of

```
config
```

Rename:

```powershell
Rename-Item $HOME\.ssh\config.txt config
```

---

## Error

```
Error connecting to agent
```

Solution:

```powershell
Start-Service ssh-agent
```

Then:

```powershell
ssh-add $HOME\.ssh\id_ed25519_personal
```

---

## Error

```
Permission denied (publickey)
```

Check:

- SSH key is added to GitHub.
- SSH agent is running.
- SSH config is correct.
- Repository remote uses the SSH alias.

---

# How It Works

```
Repository
     │
     ▼
git@github-personal:user/repo.git
     │
     ▼
SSH Config
     │
     ▼
Uses id_ed25519_personal
     │
     ▼
GitHub Personal Account
```

```
Repository
     │
     ▼
git@github-second:user/repo.git
     │
     ▼
SSH Config
     │
     ▼
Uses id_ed25519_second
     │
     ▼
GitHub Second Account
```

---

# Result

You can now:

- Work with multiple GitHub accounts simultaneously.
- Avoid repeatedly signing in and out.
- Push and pull from different repositories without credential conflicts.
- Use VS Code normally with Git over SSH.