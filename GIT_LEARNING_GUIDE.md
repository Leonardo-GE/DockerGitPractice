# Git & Version Control System (VCS) Learning Guide

This guide will help you practice essential Git commands and concepts using the DockerTest repository.

---

## 1. Advantages of VCS (Version Control System)

**Why do we need VCS?**
- Track changes over time
- Collaborate with team members
- Revert to previous versions if needed
- Maintain history of who changed what and when
- Manage different versions/features simultaneously

---

## 2. Commit

A commit is a snapshot of your project at a specific point in time. It records changes with a message explaining what was modified.

### What commit consists of:
- **Author**: Who made the change
- **Timestamp**: When the change was made
- **Message**: Description of what changed
- **Hash**: Unique identifier for the commit
- **Changes**: The actual file modifications

### Practice Commands:

#### 2.1 Check current status
```bash
git status
```
**Explanation**: Shows which files have been modified, staged, or are untracked.

#### 2.2 Stage files for commit
```bash
# Stage a specific file
git add index.html

# Stage all changes
git add .

# Stage specific changes interactively
git add -p
```
**Explanation**: Staging prepares files to be included in the next commit. Use `git add .` to stage all changes.

#### 2.3 Create a commit
```bash
git commit -m "Add index.html for Nginx"
```
**Explanation**: Creates a snapshot with a descriptive message. The `-m` flag allows you to add a message directly.

#### 2.4 View commit history
```bash
# View last 5 commits
git log --oneline -5

# View detailed commit history
git log

# View commits for a specific file
git log -- index.html
```
**Explanation**: Shows the history of commits. `--oneline` makes it more readable.

#### 2.5 View changes in a specific commit
```bash
git show <commit-hash>
```
**Explanation**: Displays the changes made in a specific commit. Replace `<commit-hash>` with the actual hash from `git log`.

#### 2.6 Change/Amend a commit
```bash
# Modify the last commit message
git commit --amend -m "New commit message"

# Add forgotten files to the last commit
git add forgotten-file.txt
git commit --amend --no-edit
```
**Explanation**: Allows you to modify the most recent commit without creating a new one.

#### 2.7 Undo a commit
```bash
# Undo last commit but keep changes staged
git reset --soft HEAD~1

# Undo last commit and keep changes unstaged
git reset --mixed HEAD~1

# Undo last commit and discard changes (DANGEROUS!)
git reset --hard HEAD~1

# Revert a commit (creates a new commit that undoes changes)
git revert <commit-hash>
```
**Explanation**: 
- `--soft`: Keeps changes staged
- `--mixed`: Keeps changes but unstaged
- `--hard`: Discards changes completely
- `revert`: Safer option that creates a new commit

---

## 3. Branch

A branch is an independent line of development. It allows you to work on features without affecting the main code.

### What is a branch?
- A pointer to a specific commit
- Allows parallel development
- Default branch is usually `main` or `master`
- Branches can be merged back together

### Practice Commands:

#### 3.1 View branches
```bash
# View local branches
git branch

# View all branches (local and remote)
git branch -a

# View branches with last commit info
git branch -v
```
**Explanation**: Shows all available branches. The current branch is marked with `*`.

#### 3.2 Create a branch
```bash
# Create a new branch
git branch feature/add-dockerfile

# Create and switch to a new branch (recommended)
git checkout -b feature/add-dockerfile

# Modern alternative (Git 2.23+)
git switch -c feature/add-dockerfile
```
**Explanation**: Creates a new branch based on the current commit. The `-b` flag creates and switches in one command.

#### 3.3 Switch to a branch
```bash
# Switch to existing branch
git checkout feature/add-dockerfile

# Modern alternative
git switch feature/add-dockerfile
```
**Explanation**: Moves you to a different branch. All files will update to match that branch's state.

#### 3.4 Delete a branch
```bash
# Delete local branch (safe - won't delete if not merged)
git branch -d feature/add-dockerfile

# Force delete local branch
git branch -D feature/add-dockerfile

# Delete remote branch
git push origin --delete feature/add-dockerfile
```
**Explanation**: Removes a branch. Use `-d` for safety, `-D` to force delete.

#### 3.5 Rename a branch
```bash
# Rename current branch
git branch -m new-branch-name

# Rename specific branch
git branch -m old-name new-name
```
**Explanation**: Changes the name of a branch.

---

## 4. Merge/Rebase

Both merge and rebase integrate changes from one branch into another, but they work differently.

### Why do we need merge/rebase?
- Combine work from different branches
- Integrate features into main code
- Keep history clean and organized

### Differences between merge & rebase:

| Aspect | Merge | Rebase |
|--------|-------|--------|
| **History** | Creates a merge commit | Rewrites history (linear) |
| **Readability** | Shows branching visually | Cleaner, linear history |
| **Safety** | Safer, doesn't rewrite history | Can be risky if used on shared branches |
| **Use Case** | Public/shared branches | Local/feature branches |

### Practice Commands:

#### 4.1 Merge a branch
```bash
# Switch to target branch (usually main)
git checkout main

# Merge feature branch into main
git merge feature/add-dockerfile

# Merge with a custom message
git merge feature/add-dockerfile -m "Merge feature: add dockerfile"

# Abort a merge if conflicts arise
git merge --abort
```
**Explanation**: Combines changes from one branch into another. Creates a merge commit that shows the integration point.

#### 4.2 Rebase a branch
```bash
# Switch to feature branch
git checkout feature/add-dockerfile

# Rebase onto main
git rebase main

# Abort rebase if needed
git rebase --abort

# Continue after resolving conflicts
git rebase --continue
```
**Explanation**: Replays your commits on top of another branch. Creates a linear history but rewrites commit hashes.

#### 4.3 Handle merge conflicts
```bash
# View conflicted files
git status

# After resolving conflicts manually, stage the files
git add .

# Complete the merge
git commit -m "Resolve merge conflicts"

# Or for rebase
git rebase --continue
```
**Explanation**: When the same lines are changed in both branches, Git can't automatically merge. You must resolve conflicts manually.

#### 4.4 Interactive rebase (advanced)
```bash
# Rebase last 3 commits interactively
git rebase -i HEAD~3

# Rebase onto another branch interactively
git rebase -i main
```
**Explanation**: Allows you to edit, squash, or reorder commits. Useful for cleaning up history before merging.

---

## 5. Gitignore

A `.gitignore` file specifies which files and directories Git should ignore.

### Why do we need a gitignore file?
- Exclude build artifacts and dependencies
- Prevent committing sensitive files (passwords, API keys)
- Avoid tracking OS-specific files
- Keep repository clean

### Practice Commands:

#### 5.1 Create a .gitignore file
```bash
# Create the file
touch .gitignore

# Or create with initial content
echo "node_modules/" > .gitignore
```
**Explanation**: Creates a `.gitignore` file in the repository root.

#### 5.2 Gitignore syntax
```
# Comments start with #

# Ignore specific file
config.properties

# Ignore all files with extension
*.log
*.tmp

# Ignore directories
node_modules/
build/
dist/

# Ignore files in specific directory
src/main/resources/secrets/

# Negate a pattern (don't ignore this)
!important.log

# Ignore all except specific file
*.txt
!README.txt
```
**Explanation**: Each line is a pattern. Use `/` for directories, `*` for wildcards, `!` to negate.

#### 5.3 Check what's ignored
```bash
# Check if a file is ignored
git check-ignore -v filename.txt

# List all ignored files
git status --ignored
```
**Explanation**: Helps verify that your `.gitignore` is working correctly.

#### 5.4 Stop tracking a file that's already committed
```bash
# Remove from Git but keep locally
git rm --cached filename.txt

# Remove directory
git rm -r --cached directory/

# Commit the removal
git commit -m "Stop tracking filename.txt"
```
**Explanation**: Use this when you accidentally committed a file that should be ignored.

---

## 6. Cloning a Repository

Cloning creates a local copy of a remote repository with full history.

### Practice Commands:

#### 6.1 Clone a repository
```bash
# Clone with HTTPS
git clone https://github.com/username/repository.git

# Clone with SSH
git clone git@github.com:username/repository.git

# Clone into a specific directory
git clone https://github.com/username/repository.git my-folder

# Clone with limited history (faster)
git clone --depth 1 https://github.com/username/repository.git
```
**Explanation**: Downloads the entire repository including all branches and history.

#### 6.2 View remote information
```bash
# List all remotes
git remote -v

# Show detailed remote info
git remote show origin
```
**Explanation**: Shows where your repository is hosted (usually `origin`).

#### 6.3 Add a remote
```bash
git remote add origin https://github.com/username/repository.git
```
**Explanation**: Connects your local repository to a remote one.

#### 6.4 Fetch from remote
```bash
# Fetch all changes from remote
git fetch origin

# Fetch specific branch
git fetch origin main
```
**Explanation**: Downloads changes from remote without merging them locally.

#### 6.5 Pull from remote
```bash
# Fetch and merge in one command
git pull origin main

# Pull with rebase instead of merge
git pull --rebase origin main
```
**Explanation**: Downloads and integrates remote changes. Equivalent to `git fetch` + `git merge`.

#### 6.6 Push to remote
```bash
# Push current branch
git push origin main

# Push all branches
git push origin --all

# Push with tracking
git push -u origin feature/new-feature

# Force push (use with caution!)
git push --force origin main
```
**Explanation**: Uploads your commits to the remote repository. `-u` sets up tracking for future pushes.

---

## 7. Credentials

Managing authentication for accessing remote repositories.

### Why do we need credentials?
- Authenticate with remote repositories (GitHub, GitLab, etc.)
- Secure access without exposing passwords
- Different credential levels for different use cases

### Credential Levels:

| Level | Scope | Use Case |
|-------|-------|----------|
| **Local** | Single repository | Project-specific credentials |
| **Global** | All repositories on user account | Default for all your projects |
| **System** | All users on the machine | Shared machine setup |

### Practice Commands:

#### 7.1 Configure credentials
```bash
# Set global user name
git config --global user.name "Your Name"

# Set global user email
git config --global user.email "your.email@example.com"

# Set local credentials (for this repo only)
git config --local user.name "Local Name"
git config --local user.email "local.email@example.com"

# Set system credentials (all users)
git config --system user.name "System Name"
```
**Explanation**: Configures who you are when making commits. Global is most common.

#### 7.2 View credentials
```bash
# View all config
git config --list

# View global config
git config --global --list

# View local config
git config --local --list

# View specific setting
git config user.name
```
**Explanation**: Shows your current Git configuration.

#### 7.3 Store credentials securely
```bash
# Use credential helper (recommended)
git config --global credential.helper store

# Use macOS keychain
git config --global credential.helper osxkeychain

# Use Windows credential manager
git config --global credential.helper manager

# Use pass (Linux)
git config --global credential.helper pass
```
**Explanation**: Stores credentials securely so you don't have to enter them repeatedly.

#### 7.4 SSH Keys (alternative to passwords)
```bash
# Generate SSH key
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"

# Add SSH key to agent
ssh-add ~/.ssh/id_rsa

# Test SSH connection
ssh -T git@github.com
```
**Explanation**: SSH keys are more secure than passwords. Add the public key to your GitHub/GitLab account.

#### 7.5 Differences between credential levels
```bash
# Local (this repo only)
git config --local user.name "Project User"

# Global (all your repos)
git config --global user.name "Your Name"

# System (all users on machine)
git config --system user.name "System User"

# View which level a setting comes from
git config --list --show-origin
```
**Explanation**: Local overrides global, global overrides system. Use local for project-specific settings.

---

## Practice Workflow

Here's a complete workflow to practice all concepts:

```bash
# 1. Check current status
git status

# 2. Create a new branch for a feature
git checkout -b feature/update-nginx-config

# 3. Make changes to files
# (Edit index.html or create new files)

# 4. Stage changes
git add .

# 5. Create a commit
git commit -m "Update Nginx configuration"

# 6. View commit history
git log --oneline -5

# 7. Switch back to main
git checkout main

# 8. Merge the feature branch
git merge feature/update-nginx-config

# 9. Delete the feature branch
git branch -d feature/update-nginx-config

# 10. View final history
git log --oneline -10
```

---

## Quick Reference

| Task | Command |
|------|---------|
| Check status | `git status` |
| Stage files | `git add .` |
| Commit | `git commit -m "message"` |
| View history | `git log --oneline` |
| Create branch | `git checkout -b branch-name` |
| Switch branch | `git checkout branch-name` |
| Merge branch | `git merge branch-name` |
| Rebase branch | `git rebase main` |
| Create .gitignore | `touch .gitignore` |
| Clone repo | `git clone <url>` |
| Push to remote | `git push origin main` |
| Pull from remote | `git pull origin main` |
| Configure user | `git config --global user.name "Name"` |

---

## Tips & Best Practices

1. **Commit Often**: Make small, logical commits with clear messages
2. **Use Branches**: Always create a branch for new features
3. **Write Good Messages**: Use present tense, be descriptive
4. **Review Before Committing**: Use `git diff` to see changes
5. **Pull Before Push**: Always pull latest changes before pushing
6. **Use .gitignore**: Keep your repository clean
7. **Avoid Force Push**: Only use `--force` when absolutely necessary
8. **Backup Before Rebase**: Rebase rewrites history, so be careful

---

## Troubleshooting

### Accidentally committed to wrong branch?
```bash
git reset --soft HEAD~1
git checkout correct-branch
git commit -m "message"
```

### Need to undo a pushed commit?
```bash
git revert <commit-hash>
git push origin main
```

### Merge conflicts?
```bash
# View conflicts
git status

# Edit files to resolve conflicts, then:
git add .
git commit -m "Resolve conflicts"
```

### Lost commits?
```bash
# View all commits including deleted ones
git reflog

# Recover a commit
git checkout <commit-hash>
```

---

Happy learning! Practice these commands regularly to become proficient with Git.
