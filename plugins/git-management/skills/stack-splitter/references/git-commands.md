# Git Command Reference — stack-splitter

## Inspecting commits and diffs

```bash
# List commits between base and branch (one-line summary)
git log --oneline <base>..<branch>

# Full commit details including file changes
git log --stat <base>..<branch>

# Diff of all changes (three-dot syntax: compares branch tip to merge-base)
git diff <base>...<branch>

# Diff of specific commit
git show <hash>

# Files changed in a commit
git show --name-only --format="" <hash>

# Commits that touched a specific file
git log --oneline <base>..<branch> -- <path>
```

## Branch ancestry

```bash
# Check if commit A is an ancestor of commit B
git merge-base --is-ancestor <A> <B> && echo "yes" || echo "no"

# Find the common ancestor
git merge-base <base> <branch>

# Check current branch name
git rev-parse --abbrev-ref HEAD

# Verify a branch ref exists
git rev-parse --verify refs/heads/<branch>
```

## Creating branches and cherry-picking

```bash
# Create branch from a specific point
git checkout -b <new-branch> <start-point>

# Cherry-pick a single commit
git cherry-pick <hash>

# Cherry-pick a range (inclusive of both ends)
git cherry-pick <first-hash>^..<last-hash>

# Cherry-pick multiple specific commits
git cherry-pick <hash1> <hash2> <hash3>

# Cherry-pick without committing (stage only)
git cherry-pick --no-commit <hash>
```

## Conflict handling during cherry-pick

```bash
# See which files are conflicted
git status

# After manually resolving conflicts
git add <resolved-files>
git cherry-pick --continue

# Abandon the cherry-pick entirely
git cherry-pick --abort

# Show the three-way diff (ours / base / theirs)
git diff --cc
```

## Detecting default branch

```bash
# Preferred: read origin HEAD pointer
git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@'

# Fallback: check if main or master exists locally
git show-ref --verify --quiet refs/heads/main && echo main
git show-ref --verify --quiet refs/heads/master && echo master
```

## Cleaning up if the split goes wrong

```bash
# Delete a branch that was partially created (must not be checked out)
git branch -D <branch>

# Return to original branch
git checkout <source-branch>

# Check working tree is clean before starting
git status --porcelain
```

## Visualising a stack after creation

```bash
# Show branch topology for the stack
git log --oneline --graph <base> <s1> <s2> <s3>

# Show only commits reachable from s3 but not base
git log --oneline <base>..<s3>
```
