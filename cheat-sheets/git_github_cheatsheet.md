# 🚀 Ultimate Git & GitHub Cheat Sheet

## 📋 Table of Contents

- [Git Basics](#git-basics)
- [Repository Management](#repository-management)
- [Branching & Merging](#branching--merging)
- [Remote Operations](#remote-operations)
- [Staging & Committing](#staging--committing)
- [History & Logs](#history--logs)
- [Undoing Changes](#undoing-changes)
- [Stashing](#stashing)
- [Tags](#tags)
- [GitHub Specific](#github-specific)
- [Advanced Git](#advanced-git)
- [Git Configuration](#git-configuration)
- [Troubleshooting](#troubleshooting)

---

## 🎯 Git Basics

### Initial Setup

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main
```

### Create Repository

```bash
git init                    # Initialize new repo
git clone <url>            # Clone existing repo
git clone <url> <folder>   # Clone to specific folder
```

---

## 📁 Repository Management

### Status & Information

```bash
git status                 # Show working tree status
git status -s             # Short status format
git log                   # Show commit history
git log --oneline         # Compact log format
git log --graph --oneline # Visual branch graph
git show                  # Show latest commit details
git show <commit-hash>    # Show specific commit
git diff                  # Show unstaged changes
git diff --staged         # Show staged changes
git diff HEAD             # Show all changes since last commit
```

---

## 🌿 Branching & Merging

### Branch Operations

```bash
git branch                    # List local branches
git branch -a                # List all branches (local + remote)
git branch -r                # List remote branches
git branch <branch-name>     # Create new branch
git checkout <branch-name>   # Switch to branch
git checkout -b <branch>     # Create and switch to new branch
git switch <branch-name>     # Modern way to switch branches
git switch -c <branch>       # Create and switch (modern)
git branch -d <branch>       # Delete merged branch
git branch -D <branch>       # Force delete branch
git branch -m <old> <new>    # Rename branch
```

### Merging

```bash
git merge <branch>           # Merge branch into current
git merge --no-ff <branch>   # Merge with merge commit
git merge --squash <branch>  # Squash merge
git rebase <branch>          # Rebase current branch
git rebase -i <commit>       # Interactive rebase
git cherry-pick <commit>     # Apply specific commit
```

---

## 🌐 Remote Operations

### Remote Management

```bash
git remote                   # List remotes
git ls-remote                #List all remotes branches
git remote -v               # List remotes with URLs
git remote add <name> <url> # Add remote
git remote remove <name>    # Remove remote
git remote rename <old> <new> # Rename remote
git remote set-url <name> <url> # Change remote URL
```

### Sync Operations

```bash
git fetch                   # Fetch from all remotes
git fetch <remote>         # Fetch from specific remote
git pull                   # Fetch and merge
git pull --rebase         # Fetch and rebase
git push                  # Push to default remote
git push <remote> <branch> # Push to specific remote/branch
git push -u origin <branch> # Push and set upstream
git push --force-with-lease # Safer force push
git push --tags           # Push tags
```

---

## 📝 Staging & Committing

### Staging Files

```bash
git add <file>             # Stage specific file
git add .                  # Stage all changes
git add *.js              # Stage all JS files
git add -A                # Stage all (including deletions)
git add -p                # Interactive staging
git rm <file>             # Remove and stage deletion
git mv <old> <new>        # Move/rename and stage
```

### Committing

```bash
git commit -m "message"    # Commit with message
git commit -am "message"   # Stage and commit all tracked files
git commit --amend         # Modify last commit
git commit --amend -m "new message" # Change last commit message
git commit --no-verify     # Skip pre-commit hooks
```

---

## 📚 History & Logs

### Viewing History

```bash
git log --oneline                    # Compact format
git log --graph --decorate --all     # Visual graph of all branches
git log -p                          # Show patches/diffs
git log --stat                      # Show file change statistics
git log --author="Name"             # Filter by author
git log --since="2 days ago"        # Filter by date
git log --grep="pattern"            # Search commit messages
git log <file>                      # History of specific file
git blame <file>                    # Show who changed each line
git shortlog                        # Summary by author
```

### Searching

```bash
git grep "pattern"                  # Search in tracked files
git log -S "code"                   # Search for code changes
git log --pickaxe-regex -S "regex"  # Regex search in changes
```

---

## ↩️ Undoing Changes

### Working Directory

```bash
git checkout -- <file>     # Discard changes in file
git checkout .            # Discard all changes
git restore <file>        # Modern way to discard changes
git restore .            # Discard all changes (modern)
git clean -fd            # Remove untracked files and directories
```

### Staging Area

```bash
git reset <file>          # Unstage file
git reset                # Unstage all files
git restore --staged <file> # Modern way to unstage
```

### Commits

```bash
git reset --soft HEAD~1   # Undo last commit, keep changes staged
git reset HEAD~1          # Undo last commit, unstage changes
git reset --hard HEAD~1   # Undo last commit, delete changes
git revert <commit>       # Create new commit that undoes changes
git revert HEAD          # Revert last commit
```

---

## 💾 Stashing

### Basic Stashing

```bash
git stash                 # Stash current changes
git stash push -m "message" # Stash with message
git stash list           # List all stashes
git stash show           # Show latest stash changes
git stash show stash@{1} # Show specific stash
git stash apply          # Apply latest stash
git stash apply stash@{1} # Apply specific stash
git stash pop            # Apply and remove latest stash
git stash drop           # Delete latest stash
git stash clear          # Delete all stashes
```

### Advanced Stashing

```bash
git stash -u             # Include untracked files
git stash -a             # Include all files (even ignored)
git stash push <file>    # Stash specific file
git stash branch <name>  # Create branch from stash
```

---

## 🏷️ Tags

### Creating Tags

```bash
git tag                   # List all tags
git tag <tagname>        # Create lightweight tag
git tag -a <tagname> -m "message" # Create annotated tag
git tag -a <tagname> <commit> # Tag specific commit
```

### Managing Tags

```bash
git show <tagname>       # Show tag information
git tag -d <tagname>     # Delete local tag
git push origin <tagname> # Push specific tag
git push --tags          # Push all tags
git push origin --delete <tagname> # Delete remote tag
```

---

## 🐙 GitHub Specific

### Repository Operations

```bash
# Clone with SSH
git clone git@github.com:username/repo.git

# Add GitHub remote
git remote add origin https://github.com/username/repo.git

# Push to GitHub
git push -u origin main
```

### GitHub CLI (gh)

```bash
gh repo create <name>    # Create repository
gh repo clone <repo>     # Clone repository
gh pr create            # Create pull request
gh pr list              # List pull requests
gh pr checkout <number> # Checkout PR locally
gh pr merge <number>    # Merge pull request
gh issue create         # Create issue
gh issue list           # List issues
```

### Workflows & Actions

```bash
# Check workflow status
gh run list

# View workflow details
gh run view <run-id>

# Download artifacts
gh run download <run-id>
```

---

## 🔥 Advanced Git

### Interactive Rebase

```bash
git rebase -i HEAD~3     # Interactive rebase last 3 commits
# Commands in interactive mode:
# pick   = use commit
# reword = use commit, but edit message
# edit   = use commit, but stop for amending
# squash = use commit, but meld into previous commit
# fixup  = like squash, but discard commit message
# drop   = remove commit
```

### Bisect (Binary Search for Bugs)

```bash
git bisect start         # Start bisecting
git bisect bad          # Mark current commit as bad
git bisect good <commit> # Mark commit as good
git bisect reset        # End bisecting session
```

### Worktrees

```bash
git worktree add <path> <branch>  # Create new worktree
git worktree list                 # List worktrees
git worktree remove <path>        # Remove worktree
```

### Submodules

```bash
git submodule add <url> <path>    # Add submodule
git submodule init               # Initialize submodules
git submodule update             # Update submodules
git submodule update --recursive # Update nested submodules
```

---

## ⚙️ Git Configuration

### Configuration Levels

```bash
git config --system     # System-wide config
git config --global     # User-wide config
git config --local      # Repository config
```

### Useful Settings

```bash
# Editor
git config --global core.editor "code --wait"

# Merge tool
git config --global merge.tool vimdiff

# Aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.lg "log --oneline --graph --decorate"

# Line endings
git config --global core.autocrlf true   # Windows
git config --global core.autocrlf input  # Mac/Linux

# Default branch name
git config --global init.defaultBranch main

# Push behavior
git config --global push.default simple
```

---

## 🚨 Troubleshooting

### Common Issues

```bash
# Merge conflicts
git status              # See conflicted files
# Edit files to resolve conflicts
git add <resolved-file>
git commit

# Detached HEAD
git checkout <branch-name>  # Switch to a branch
git checkout -b <new-branch> # Create branch from current state

# Reset remote tracking
git branch --set-upstream-to=origin/<branch> <branch>

# Remove file from Git but keep locally
git rm --cached <file>

# Change remote URL (HTTPS to SSH)
git remote set-url origin git@github.com:username/repo.git

# Sync fork with upstream
git remote add upstream <original-repo-url>
git fetch upstream
git checkout main
git merge upstream/main
```

### Recovery Commands

```bash
git reflog              # Show reference log
git fsck --lost-found   # Find orphaned commits
git gc                  # Garbage collection
git prune               # Remove unreachable objects
```

---

## 🎨 Git Flow Commands

### Feature Development

```bash
git flow init           # Initialize git flow
git flow feature start <name>  # Start new feature
git flow feature finish <name> # Finish feature
git flow feature publish <name> # Publish feature
```

### Releases

```bash
git flow release start <version>  # Start release
git flow release finish <version> # Finish release
```

---

## 📋 Quick Reference Patterns

### Daily Workflow

```bash
git pull                # Get latest changes
git checkout -b feature/new-feature  # Create feature branch
# Make changes
git add .              # Stage changes
git commit -m "Add new feature"  # Commit
git push -u origin feature/new-feature  # Push branch
# Create PR on GitHub
git checkout main      # Switch back to main
git pull              # Update main
git branch -d feature/new-feature  # Delete feature branch
```

### Emergency Hotfix

```bash
git checkout main
git pull
git checkout -b hotfix/critical-bug
# Fix the bug
git add .
git commit -m "Fix critical bug"
git push -u origin hotfix/critical-bug
# Create PR and merge immediately
```

### Collaboration Workflow

```bash
git clone <repo-url>   # Clone project
git checkout -b my-feature  # Create feature branch
# Work on feature
git fetch origin       # Check for updates
git rebase origin/main # Rebase on latest main
git push --force-with-lease # Push rebased branch
# Create PR when ready
```

---

## 🔑 Pro Tips

1. **Always use `--force-with-lease` instead of `--force`** - safer force pushing
2. **Use `git stash` before switching branches** - avoid losing work
3. **Write descriptive commit messages** - your future self will thank you
4. **Use `.gitignore` early** - prevent committing unwanted files
5. **Rebase feature branches** - keep history clean
6. **Use signed commits** - verify your identity
7. **Set up SSH keys** - avoid password prompts
8. **Use git hooks** - automate workflows
9. **Learn interactive rebase** - powerful history editing
10. **Use `git bisect`** - efficiently find bugs

---

_Remember: Git is powerful but can be dangerous. Always make backups of important work before complex operations!_
