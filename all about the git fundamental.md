# Git Fundamentals - Complete Study Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Initial Setup](#initial-setup)
3. [Git Workflow](#git-workflow)
4. [Commits & History](#commits--history)
5. [Staging Area](#staging-area)
6. [Tracking Changes](#tracking-changes)
7. [Working with Multiple Files](#working-with-multiple-files)
8. [File Management](#file-management)
9. [Remote Repository](#remote-repository)

---

## Introduction

Git is a **distributed version control system** that allows you to track changes in your code and collaborate with others.

### Check Git Version
```bash
git --version
```
Shows the currently installed Git version.

---

## Initial Setup

### Creating a New Project

1. **Create a project directory**
   ```bash
   mkdir first_project
   cd first_project
   ```

2. **Initialize Git Repository**
   ```bash
   git init
   ```
   - Creates an empty `.git` folder in your project directory
   - This folder contains all Git configuration and history

### Checking Repository Status
```bash
git status
```
- Shows the current branch and tracking status
- If NOT a Git repo, you'll see: `not a git repo`

---

## Git Branches

### Default Branch: Master vs Main

By default, `git init` creates a **master** branch. However, modern practice uses **main**.

### Initialize Git with Main Branch
```bash
git init --b main
```
- Creates an empty Git repository with **main** as the default branch
- You can see this in `git status`: `on branch main`

### Removing Existing Git (if needed)
```bash
rm -rf .git
```
- **Note**: Use GUI file manager if command fails
- Then reinitialize with: `git init --b main`

---

## Git Workflow Overview

Git has three main areas:

```
┌─────────────────────────────────────────────────────────────────┐
│                 PROJECT DIRECTORY (first_project)               │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────────┐   ┌──────────────┐   ┌──────────────────┐ │
│  │ WORKING          │   │  STAGING     │   │ LOCAL            │ │
│  │ DIRECTORY        │──→│  AREA        │──→│ REPOSITORY (.git)│ │
│  │ (Your Files)     │   │  (INDEX)     │   │ (Commit History) │ │
│  └──────────────────┘   └──────────────┘   └──────────────────┘ │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

**Flow**: Working Directory → Staging Area → Local Repository

---

## Staging Area

### What is the Staging Area?

The **staging area** (also called INDEX) is where you prepare files before committing them to the repository.

### Adding Files to Staging Area

**Add a specific file:**
```bash
git add filename.txt
```

**Add all modified and new files:**
```bash
git add .
```

### Checking What's in the Staging Area
```bash
git status
```

**Output example:**
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
    new file: FirstCode.txt
```

### Important Concept: You MUST Stage Before Commit

❌ **Wrong way:**
```bash
git commit -m "my commit"
# Error: nothing staged for commit
```

✅ **Correct way:**
```bash
git add filename.txt
git commit -m "my commit"
```

---

## Your First Commit

### Step 1: Create a File
```bash
echo "hello world" > first_code.txt
```

### Step 2: Check Status
```bash
git status
```
Output shows: `Untracked files: first_code.txt`

### Step 3: Stage the File
```bash
git add first_code.txt
```

### Step 4: Commit with a Message
```bash
git commit -m "my first commit"
```

**Output:**
```
[main (root-commit) fd914d5] my first commit
 1 file changed, 1 insertion(+)
 create mode 100644 first_code.txt
```

### Understanding the Output
- `fd914d5` - First 7 characters of the **commit checksum** (SHA-1 hash: 40 characters)
- `HEAD -> main` - Pointer to the latest commit on main branch
- Full checksum example: `fd914d5319d3aeb39c14c4f7da6501dd666b3ea3`

---

## Commits & History

### View Commit History
```bash
git log
```

**Output example:**
```
commit 769d21cda7d34e06ec82bad921755150248ef3ca (HEAD -> main)
Author: navin <email@example.com>
Date:   Fri Jun 23 15:40:01 2023 +0530

    My second commit

commit fd914d5319d3aeb39c14c4f7da6501dd666b3ea3
Author: navin <email@example.com>
Date:   Thu Jun 22 14:06:39 2023 +0530

    My first commit
```

### Understanding Commit Information
- **Commit hash**: Unique identifier for each commit
- **HEAD**: Pointer to your current position in history
- **Author**: Who made the commit
- **Date**: When the commit was created
- **Message**: The commit message you provided

---

## Tracking Changes

### Workflow: Modify → Stage → Commit

**Step 1: Modify a File**
```bash
# Edit first_code.txt
hello world !!
```

**Step 2: Check Status**
```bash
git status
```

**Output:**
```
Changes not staged for commit:
  modified: first_code.txt
```

This means the file has changed but is NOT in the staging area yet.

### Two Ways to Stage & Commit

#### Option 1: Stage then Commit (Recommended for beginners)
```bash
git add first_code.txt
git commit -m "My second commit"
```

#### Option 2: Stage & Commit in One Command
```bash
git commit -a -m "My third commit"
```

**Note:** The `-a` flag automatically stages all modified files (but NOT new untracked files)

---

## Viewing Changes with Git Diff

### See Changes in Working Directory
```bash
git diff
```

Shows differences between working directory and staging area.

**Output example:**
```
diff --git a/FirstCode.txt b/FirstCode.txt
index 5902a57..abd3c79 100644
--- a/FirstCode.txt
+++ b/FirstCode.txt
@@ -1 +1,3 @@
-Hello World !!!
\ No newline at end of file
+Hello World !!!
+take input from user
+add the values
\ No newline at end of file
```

**Symbols:**
- `+` - Added lines
- `-` - Removed lines
- `@@ -1 +1,3 @@` - Shows line numbers and changes

### See Changes in Staging Area
```bash
git diff --staged
```

Shows differences between staging area and last commit.

**Important**: After staging a file, `git diff` shows nothing. Use `git diff --staged` to see staged changes.

---

## Working with Multiple Files

### Create Multiple Files
```bash
echo "Hello World !!!" > FirstCode.txt
echo "# Project Documentation" > Readme.md
echo "username=admin\npassword=secret123" > creds.txt
```

### Check Status
```bash
git status
```

**Output:**
```
On branch main
Changes not staged for commit:
  modified: FirstCode.txt

Untracked files:
  Readme.md
  creds.txt
```

### Stage All Files at Once
```bash
git add .
```

### Commit All Changes
```bash
git commit -m "readme and story 3.2"
```

**Output:**
```
[main 4ab4b05] readme and story 3.2
 3 files changed, 8 insertions(+), 1 deletion(-)
 create mode 100644 Readme.md
 create mode 100644 creds.txt
```

---

## File Management: Removing Files

### Problem: Sensitive Files in Git

⚠️ **Important**: Never commit sensitive files (passwords, API keys, tokens) to Git!

If you accidentally committed a file like `creds.txt`, you need to:

### Step 1: Remove File from Git Staging
```bash
git rm --cached creds.txt
```

This removes the file from Git tracking but keeps it in your working directory (if you want to delete it).

### Step 2: Delete the File Physically
- Delete the file from your folder using file manager or:
  ```bash
  rm creds.txt
  ```

### Step 3: Check Status
```bash
git status
```

**Output:**
```
Changes to be committed:
  deleted: creds.txt
```

### Step 4: Commit the Deletion
```bash
git commit -m "removed the creds file"
```

**Output:**
```
[main a8eba6c] removed the creds file
 1 file changed, 2 deletions(-)
 delete mode 100644 creds.txt
```

### Result
- ✅ File is removed from Git history
- ✅ File is no longer in the project directory
- ✅ No passwords/secrets are exposed

---

## Remote Repository

### What is a Remote Repository?

A **remote repository** is a Git repository hosted on a server (like GitHub) where:
- Multiple people can collaborate
- Code is backed up centrally
- Changes can be shared and synchronized

### Why Do You Need It?

**Local Git** (on your computer):
- ✅ Version control
- ✅ Commit history
- ❌ No collaboration
- ❌ No backup
- ❌ Only accessible on your machine

**Remote Repository** (on GitHub/GitLab):
- ✅ Version control
- ✅ Commit history
- ✅ Collaboration with team members
- ✅ Automatic backups
- ✅ Accessible from anywhere

### Common Remote Repository Services
- **GitHub** - Most popular, free and paid plans
- **GitLab** - Open-source alternative
- **Bitbucket** - Integration with Jira
- **Azure DevOps** - Microsoft's solution

---

## Summary: Git Workflow

```
1. Initialize Repository
   git init --b main

2. Create/Modify Files
   (edit files in your text editor)

3. Check Status
   git status

4. Stage Changes
   git add .

5. View Changes (Optional)
   git diff --staged

6. Commit Changes
   git commit -m "Descriptive message"

7. View History (Optional)
   git log

8. Push to Remote (Next Topic)
   git push origin main
```

---

## Key Git Commands Reference

| Command | Purpose |
|---------|---------|
| `git init --b main` | Initialize a new repository with main branch |
| `git status` | Show current status of working directory |
| `git add <file>` | Stage a specific file |
| `git add .` | Stage all changes |
| `git commit -m "msg"` | Commit with a message |
| `git commit -a -m "msg"` | Stage & commit in one step |
| `git log` | View commit history |
| `git diff` | Show changes not staged |
| `git diff --staged` | Show staged changes |
| `git rm --cached <file>` | Remove file from Git (not from disk) |

---

## Next Steps: Remote Repositories

Coming up: How to connect your local repository to GitHub and collaborate with others!
