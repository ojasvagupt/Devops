# Top Git Commands for DevOps - Simple Explanations

## Setup

**git init**  
Creates a new Git repository in current directory by initializing .git folder. Use for new projects. Bare (--bare) for central servers.

**git clone <url>**  
Copies a remote repository to local machine with full history. Use --depth 1 for shallow/fast CI clones.

**git config**  
Sets Git options like user name/email. Global (--global) for all repos, local for current repo.

## Branching

**git branch**  
Lists, creates, deletes branches. -l local, -r remote, -d delete merged.

**git checkout -b <branch>**  
Creates and switches to new branch. Use for features/hotfixes.

**git switch <branch>**  
Switches to existing branch (Git 2.23+).

**git merge <branch>**  
Integrates changes from branch into current. --no-ff preserves history.

**git rebase <base>**  
Replays commits from current branch on top of base branch for linear history. -i for interactive edit/squash.

## Remotes

**git remote add origin <url>**  
Adds remote repository named origin for push/pull.

**git remote -v**  
Lists remotes with URLs.

**git fetch**  
Downloads changes from remote without merging. --prune removes deleted remotes.

**git pull**  
Fetch + merge/rebase. Use --rebase to avoid merge commits.

**git push**  
Uploads local branches/commits to remote. -u sets upstream, --force-with-lease safe force.

## Staging & Commit

**git status**  
Shows working directory state: staged, unstaged, untracked files.

**git add <file>**  
Stages specific file for commit. . for all, -A all changes incl deletes.

**git commit**  
Records staged snapshot. -m message, --amend edit last.

## Inspect

**git log**  
Shows commit history. --oneline short, --graph visual.

**git diff**  
Shows unstaged changes. --staged for staged vs commit.

**git show <commit>**  
Displays specific commit details/changes.

## Undo

**git reset <commit>**  
Moves HEAD. --soft keeps staged, --hard discards all.

**git revert <commit>**  
Creates new commit undoing target (safe for shared).

**git clean -fd**  
Removes untracked files/dirs.

**git stash**  
Saves WIP changes. list/pop/apply/drop.

## File Ops

**git rm <file>**  
Removes from working/staging. --cached untracks only.

**git mv <old> <new>**  
Renames/moves and stages automatically.

## Advanced

**git cherry-pick <commit>**  
Applies specific commit to current branch (backport).

**git tag -a <tag>**  
Creates annotated tag for releases.

**Shared Branch Steps** (if others pushed):

1. git fetch origin
2. git rebase origin/branch (resolve conflicts)
3. git push --force-with-lease

**Merge vs Rebase**:

- Merge: Preserves exact history (use for PRs to main).
- Rebase: Linear (use for feature cleanup before PR).
