# Top Git Commands for DevOps

Git is essential for version control, collaboration, and CI/CD pipelines in DevOps. This guide covers top commands with short descriptions, key flags, and practical DevOps examples.

## 1. Setup & Initialization

| Command    | Description                            | Key Flags                                                           | DevOps Example |
| ---------- | -------------------------------------- | ------------------------------------------------------------------- | -------------- |
| `git init` | Initialize a new local Git repository. | `--bare`<br>`--initial-branch=<name>`<br>`--shared[=<permissions>]` | ```            |

$ git init
Initialized empty Git repository in /path/to/repo/
$ git init --bare /shared/repo.git

````
<br>Init standard repo or bare for CI hooks. |

| `git clone <url>` | Clone a repository into a new directory. | `--depth <n>`<br>`-b <branch>`<br>`--single-branch`<br>`--recursive` | ```
$ git clone https://github.com/user/repo.git
$ cd repo
$ git clone --depth 1 --single-branch -b main https://github.com/org/project.git /tmp/project
````

<br>Full clone or shallow/single-branch for CI speed. |

| `git config` | Get/set repository or global options. | `--global`<br>`--local`<br>`--list`<br>`--show-origin` | ```
$ git config --global user.name "DevOps Bot"
$ git config --global user.email "devops@company.com"
$ git config --list

````
<br>Configure identity for pipeline commits. |


## 2. Branching & Merging

| Command        | Description                  | Key Flags                                  | DevOps Example                                                                           |
| -------------- | ---------------------------- | ------------------------------------------ | ---------------------------------------------------------------------------------------- |
| `git branch`   | List/create/delete branches. | `-l` (local), `-r` (remote), `-d` (delete) | `git branch -d feature/old`<br>Delete merged feature branch post-PR in GitHub Actions.   |
| `git checkout` | Switch branches/create.      | `-b <new>` (create+switch), `-B` (force)   | `git checkout -b hotfix/bug123`<br>Create hotfix branch for urgent prod deploy.          |
| `git switch`   | Switch branches (modern).    | `-c <new>` (create)                        | `git switch -c release/v1.2`<br>Switch to new release branch for tagging.                |
| `git merge`    | Merge branch into current.   | `--no-ff` (no fast-forward), `--ff-only`   | `git merge --no-ff main`<br>Merge feature with explicit commit for audit trail.          |
| `git rebase`   | Reapply commits on new base. | `-i` (interactive), `--interactive`, `main` | ```
$ git rebase main feature-branch
$ git rebase -i HEAD~3  # reorder/squash/drop
```
Detailed: Interactive rebase to squash commits:
1. `git rebase -i main` → editor opens with commits.
2. Change 'pick' to 'squash'/'drop'.
3. Save, edit messages.
<br>Linear history for main. |


## 3. Remotes & Collaboration

| Command      | Description           | Key Flags                                 | DevOps Example                                                                                                        |
| ------------ | --------------------- | ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `git remote add origin <url>` | Add remote repository origin. | `add <name> <url>`, `rename`, `remove` | ```
$ git remote add origin https://github.com/user/repo.git
$ git remote -v
origin  https://github.com/user/repo.git (fetch)
origin  https://github.com/user/repo.git (push)
```
<br>Setup origin for push/PRs in new repo. |
| `git remote -v` | List remotes verbose. | `-v` | See output above.<br>List remotes before fetch/pull. |

| `git fetch`  | Fetch remote changes. | `--all`, `--prune`                        | `git fetch --prune origin`<br>Clean stale remote branches in cron job.                                                |
| `git pull`   | Fetch+merge.          | `--rebase` (avoid merge commits)          | `git pull --rebase origin main`<br>Update local main without extra merges in dev env.                                 |
| `git push`   | Push commits.         | `-u` (set upstream), `--force-with-lease` | `git push -u origin feature/new`<br>Set upstream & push new feature for PR.                                           |


## 5. Staging & Committing

| Command | Description | Key Flags | DevOps Example |
|---------|-------------|-----------|----------------|
| `git add` | Add file contents to staging area. | `<file>` (specific), `.` (all), `-A` (all incl. delete), `-u` (updated) | ```
$ git status
new.txt
$ git add new.txt
$ git add .
$ git add -A
````

<br>Stage files before commit in CI. |
| `git commit` | Record staged changes to repo. | `-m <msg>`, `-a` (stage+commit), `--amend` | ```
$ git commit -m "feat: add auth"
$ git commit -a -m "fix: typo"
$ git commit --amend --no-edit

````
<br>Commit with conventional messages for semantic release. |

## 6. Inspect & Status

| Command      | Description              | Key Flags                                          | DevOps Example |
| ------------ | ------------------------ | -------------------------------------------------- | -------------- |
| `git status` | Show working dir status. | `-s` (short), `-b` (branch)                        | ```
$ git status -s
 M file1
?? file2
````

<br>Quick check in pre-commit hook. |
| `git log` | View commit history. | `--oneline`, `--graph`, `-n 5`, `--author` | ```
$ git log --oneline -n 5 --graph

````
<br>Audit changes post-deploy. |
| `git diff`   | Show unstaged/staged changes. | `HEAD`, `--staged`, `--cached`                    | ```
$ git diff
$ git diff --staged
````

<br>Review diffs in PR. |
| `git show` | Show object (commit/tag/file). | `<commit>` | ```
$ git show abc123

````
<br>Inspect commit details. |



## 6. Undo & Reset

| Command      | Description             | Key Flags                                   | DevOps Example                                                                              |
| ------------ | ----------------------- | ------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `git reset`  | Reset current HEAD to specified commit. | `--soft` (keep index), `--mixed` (default, unstage), `--hard` (discard all) | ```
$ git reset --soft HEAD~1
$ git reset --hard origin/main
````

<br>Undo commits or sync with remote. |

| `git revert` | Create revert commit. | `-m` (for merges) | `git revert abc123`<br>Safe undo pushed commit via new commit for main branch. |
| `git clean` | Remove untracked files. | `-f` (force), `-d` (dirs) | `git clean -fd`<br>Clean build artifacts in CI workspace. |
| `git stash` | Temporarily save changes. | `push [-m msg]`, `list`, `pop`, `apply`, `drop` | ```
$ git stash push -m "WIP"
$ git stash list
$ git stash pop

````
<br>Switch contexts in shared env. |


## 7. File Operations

| Command | Description | Key Flags | DevOps Example |
|---------|-------------|-----------|----------------|
| `git rm` | Remove files from working dir and staging. | `--cached` (untrack only) | ```
$ git rm oldfile.txt
$ git rm --cached secret.key
````

<br>Clean obsolete config files. |
| `git mv` | Move/rename file and stage. | N/A | ```
$ git mv README.md README.md.bak

````
<br>Rename in migration scripts. |

## 8. Cherry-pick & Advanced

| Command | Description | Key Flags | DevOps Example |
|---------|-------------|-----------|----------------|
| `git cherry-pick <commit>` | Apply specific commit from other branch. | `-e` (edit), `-x` (source cite) | ```
$ git cherry-pick abc123
$ git cherry-pick -x def456
````

<br>Backport hotfix to release branch. |


## Git Cheat Sheet

```

# Quick Status & Sync

git status && git pull --rebase && git push

# New Feature Workflow

git checkout -b feature/xyz

# ... develop ...

git add . && git commit -m \"feat: add xyz\" && git push -u origin feature/xyz

# Release

git checkout main && git pull && git checkout -b release/vX.Y
git merge --no-ff feature/\* && git tag -a vX.Y -m \"Release\" && git push --tags

```

## Best Practices in DevOps

- Use branch protection rules (main requires PRs).
- Conventional commits for semantic versioning.
- Signed commits/tags for security.
- Shallow clones + single-branch in CI for speed.

Resources: [Git Docs](https://git-scm.com/docs), [GitHub Flow](https://docs.github.com/en/get-started).

Updated: Generated for DevOps workflows.

```

```

```

```

```
