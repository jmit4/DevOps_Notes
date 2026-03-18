# Git Basics

Core concepts and everyday Git commands.

---

## Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "vim"
git config --list              # Show all config
```

---

## Initialising a Repository

```bash
git init                       # Initialise a new local repo
git clone <url>                # Clone an existing remote repo
git clone <url> my-folder      # Clone into a specific folder
```

---

## Staging & Committing

```bash
git status                     # Show working tree status
git add file.txt               # Stage a specific file
git add .                      # Stage all changes
git add -p                     # Interactively stage hunks

git commit -m "message"        # Commit with a message
git commit --amend             # Amend the last commit (message or content)
```

---

## Viewing History

```bash
git log                        # Full commit log
git log --oneline              # Compact log (one line per commit)
git log --oneline --graph      # Graph view of branches
git diff                       # Unstaged changes
git diff --staged              # Staged changes vs last commit
git show <commit>              # Show a specific commit
```

---

## Undoing Changes

```bash
git restore file.txt           # Discard unstaged changes in a file
git restore --staged file.txt  # Unstage a file
git revert <commit>            # Create a new commit that undoes a commit
git reset --soft HEAD~1        # Undo last commit, keep changes staged
git reset --mixed HEAD~1       # Undo last commit, keep changes unstaged
git reset --hard HEAD~1        # Undo last commit and discard all changes (destructive)
```

---

## Remote Repositories

```bash
git remote -v                  # List remotes
git remote add origin <url>    # Add a remote
git fetch                      # Download remote changes (don't merge)
git pull                       # Fetch and merge remote changes
git push origin main           # Push to remote branch
git push -u origin main        # Push and set upstream tracking
```

---

## Tags

```bash
git tag v1.0.0                 # Create a lightweight tag
git tag -a v1.0.0 -m "Release" # Create an annotated tag
git push origin v1.0.0         # Push a tag to remote
git push origin --tags         # Push all tags
```

---

## Stashing

```bash
git stash                      # Stash current changes
git stash list                 # List all stashes
git stash pop                  # Apply latest stash and remove it
git stash apply stash@{0}      # Apply a specific stash without removing
git stash drop stash@{0}       # Delete a specific stash
```
