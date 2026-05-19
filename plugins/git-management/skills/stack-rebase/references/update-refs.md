# git rebase --update-refs — Reference

## How --update-refs works

Introduced in git 2.38 (released October 2022), `--update-refs` makes git aware of intermediate branch refs during a rebase. Without it, only the currently checked-out branch moves; all other branches in the chain stay pointing at their original commits and become stale.

With `--update-refs`, git:
1. Identifies all local branch refs that point to commits in the range being rebased.
2. Replays the commits as usual.
3. After each replayed commit, if an intermediate branch ref matched the original commit, advances that ref to the new replayed commit.

The result: the entire chain of branches moves atomically in a single `git rebase` invocation.

## Minimum git version

```bash
git --version   # must be >= 2.38.0
```

Parsing the version:
```bash
git_version=$(git --version | awk '{print $3}')
major=$(echo "$git_version" | cut -d. -f1)
minor=$(echo "$git_version" | cut -d. -f2)
if [ "$major" -lt 2 ] || { [ "$major" -eq 2 ] && [ "$minor" -lt 38 ]; }; then
  echo "git 2.38+ required"
fi
```

## When --update-refs will not work

| Situation | Why it fails | What to do |
|-----------|-------------|------------|
| Non-linear chain | Branches diverged; no shared ancestry | Manually rebase each branch individually |
| Detached HEAD rebase | Not rebasing a named branch | Checkout the tip branch first |
| Branch ref points outside the rebase range | git won't touch it | Expected — only refs in the replayed range move |
| Merge commits in the chain | Rebase re-linearises by default, removing merges | Use `--rebase-merges` with caution |

## Making --update-refs the default

Set it globally so every `git rebase` uses it:

```bash
git config --global rebase.updateRefs true
```

## Manual fallback for git < 2.38

For older git versions, rebase each branch in the chain individually from bottom to top:

```bash
# Chain: main → s1 → s2 → s3
git checkout s1
git rebase main

git checkout s2
git rebase s1

git checkout s3
git rebase s2
```

This is equivalent to `--update-refs` but requires running a separate rebase per branch and checking out each in turn.

## Checking the resulting graph

After rebasing, inspect the chain visually:

```bash
git log --oneline --graph main s1 s2 s3
```

Expected output format:
```
* abc1234 (HEAD -> s3) feat: UI layer
* def5678 (s2) feat: API endpoint
* ghi9012 (s1) feat: data model
* jkl3456 (main) chore: last main commit
```

Each branch ref should appear at a different commit with no gaps or duplications.

## Conflict recovery

If `git rebase --update-refs` stops at a conflict:

```bash
# See conflicted files
git status

# See three-way diff
git diff --cc

# After resolving manually
git add <resolved-files>
git rebase --continue

# Abandon entire rebase (returns to pre-rebase state)
git rebase --abort
```

After `--abort`, all branch refs revert to their positions before the rebase started — nothing is lost.

## Interaction with remote tracking branches

`--update-refs` only moves **local** branch refs. Remote tracking refs (`origin/s1`, etc.) are unaffected. After a successful stack rebase, the local branches have diverged from their remotes. To synchronise:

```bash
# Force-push each branch (requires push access and is a destructive remote operation)
git push --force-with-lease origin s1 s2 s3
```

This is outside the scope of the git-management plugin (local-only), but document it so the user knows what their next step is after the stack has been rebased.
