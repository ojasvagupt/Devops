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

## Merge vs Rebase - Scenarios & Flows

### Scenario 1: Feature to Main (PR Workflow)

**Merge Flow** (Recommended for protected main):

```
* b---c feature
 \
  a---d main --- M (merge commit)
```

```
git checkout main
git pull
git merge --no-ff feature-branch
git push
```

- Preserves original branch timeline.

**Rebase Flow** (Clean PR):

```
a---b---c main
    \
     d---e feature
```

```
git checkout feature-branch
git rebase main  # resolves conflicts now
git push --force-with-lease
```

- Linear history.

### Scenario 2: Hotfix Backport

**Cherry-pick + Merge**:

```
main: a---b---c
     \
release: a---d
```

```
git checkout release
git cherry-pick hotfix-commit
git merge --no-ff main
```

### Scenario 3: Shared Team Branch Sync

**Rebase** (if teammate pushed):

```
git fetch origin
git rebase origin/shared-branch  # replay your changes on top
git push --force-with-lease
```



