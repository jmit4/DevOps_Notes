# Git Workflow

Common branching strategies and collaborative workflows.

---

## Feature Branch Workflow

The most common workflow for teams:

```
main
 └── feature/my-feature
       └── (work, commits)
             └── merge/PR back to main
```

```bash
# Create and switch to a new feature branch
git checkout -b feature/my-feature

# Work, stage and commit
git add .
git commit -m "feat: add new feature"

# Keep branch up-to-date with main
git fetch origin
git rebase origin/main

# Push branch and open a Pull Request
git push origin feature/my-feature
```

---

## Branching

```bash
git branch                     # List local branches
git branch -a                  # List all branches (including remote)
git checkout -b new-branch     # Create and switch to a branch
git switch main                # Switch to main (modern syntax)
git switch -c new-branch       # Create and switch (modern syntax)
git branch -d branch-name      # Delete a merged branch
git branch -D branch-name      # Force-delete a branch
git push origin --delete branch-name  # Delete a remote branch
```

---

## Merging

```bash
git checkout main
git merge feature/my-feature          # Merge a branch into current
git merge --no-ff feature/my-feature  # Merge with a merge commit (no fast-forward)
git merge --squash feature/my-feature # Squash all commits into one before merging
```

---

## Rebasing

Rebasing rewrites history — use with care on shared branches.

```bash
git rebase main                # Rebase current branch onto main
git rebase -i HEAD~3           # Interactive rebase of last 3 commits (squash, fixup, etc.)
git rebase --abort             # Abort an in-progress rebase
git rebase --continue          # Continue after resolving conflicts
```

---

## Pull Requests (PR) / Code Review Tips

1. Keep PRs small and focused on one thing.
2. Write a clear title and description.
3. Link to the relevant issue.
4. Request reviewers early for large changes.
5. Squash trivial fixup commits before merging.

---

## Resolving Merge Conflicts

```bash
# After a conflict during merge/rebase, Git marks the file:
# <<<<<<< HEAD
# (your changes)
# =======
# (incoming changes)
# >>>>>>> feature/my-feature

# Edit the file to resolve, then:
git add resolved-file.txt
git commit                     # Complete the merge
# or:
git rebase --continue          # Complete the rebase
```

---

## Gitflow (Overview)

A structured branching model for releases:

| Branch | Purpose |
|---|---|
| `main` | Production-ready code |
| `develop` | Integration branch |
| `feature/*` | New features (branched from develop) |
| `release/*` | Release preparation (branched from develop) |
| `hotfix/*` | Urgent fixes (branched from main) |

---

## Useful Aliases

Add to `~/.gitconfig` under `[alias]`:

```ini
[alias]
  st   = status
  co   = checkout
  br   = branch
  lg   = log --oneline --graph --decorate --all
  undo = reset --soft HEAD~1
```
