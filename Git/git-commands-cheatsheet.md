# Git Commands Cheatsheet for DevOps

## Quick Reference

### Setup

```
git init                 # New repo
git clone <url>          # Copy remote
git config --global user.name "Name"
git config --global user.email "email"
```

### Daily Workflow

```
git status               # See changes
git add <file>           # Stage file
git add .                # Stage all
git commit -m "msg"      # Commit
git log --oneline -n5    # Recent commits
```

### Branching

```
git branch               # List
git checkout -b feature  # New + switch
git switch main          # Switch
git merge feature        # Merge
git rebase main          # Rebase
```

### Remote

```
git remote add origin <url>
git remote -v
git fetch
git pull --rebase
git push -u origin branch
git push --force-with-lease
```

### Undo

```
git reset --hard HEAD    # Discard changes
git revert HEAD          # Safe revert
git stash                # Stash WIP
git stash pop
git clean -fd            # Clean untracked
```

### Advanced DevOps

```
git tag -a v1.0 -m "tag"
git cherry-pick commit
git rm --cached file     # Untrack
git mv old new           # Rename
git submodule update --init
```

## Merge vs Rebase Scenarios

### Merge (preserves history)

- **Use when**: Official merge to main/release (PR merge).
- Example:
  ```
  * b---c feature
   \
    a---d main (merge commit)
  ```
- `git merge --no-ff feature` - Creates merge commit for traceability.

### Rebase (linear history)

- **Use when**: Local feature cleanup before PR, shared branch sync.
- Example:
  ```
  a---b---c main
      \
       d---e feature -> rebase to a---d'--e' feature (linear)
  ```
- `git rebase main` then `git push --force-with-lease`.
- **Danger**: Never rebase shared/public commits.

**DevOps tip**: Rebase for clean PRs, merge for production releases.

See full guide in README.md
