---
name: stack-rebase
description: This skill should be used when the user asks to "rebase my stack", "update stacked branches", "propagate changes through my stack", "sync stacked branches", "rebase with update-refs", or has a chain of branches that need to stay in sync after a commit or rebase on one of them. Operates entirely locally — never pushes or writes to remote.
allowed-tools:
  - Bash
  - AskUserQuestion
---

Rebase an entire chain of stacked branches using `git rebase --update-refs`, so all intermediate branch refs move correctly when the stack is replayed on top of the base.

## What this skill does

Given a chain `base → s1 → s2 → s3`, after a new commit lands on `s1` (or `base` moves), running this skill checks out `s3` (the tip) and executes:

```bash
git rebase --update-refs <base>
```

Git replays all commits from `base` to `s3`. Because `s1` and `s2` appear as refs in the commit graph, `--update-refs` advances each of those refs to their replayed positions. The whole chain is kept sequential in one operation.

## Inputs

Extract the chain from the user's message if present (e.g. "rebase my stack: main → s1 → s2 → s3"). The chain must be ordered: base first, tip last.

If the chain is not in the message, ask:
> "Please provide the branch chain in order from base to tip, e.g.: `main feat/s1 feat/s2 feat/s3`"

The first item is always the **base** (never modified). The remaining items are stack branches from bottom to top. The last item is the **tip**.

## Step 1: Validate prerequisites

### Git version

Verify git ≥ 2.38 (version parser snippet in `references/update-refs.md`):

```bash
git --version
```

If less than 2.38, stop and tell the user to upgrade git before retrying.

### Working tree

```bash
git status --porcelain
```

If dirty, instruct the user to stash or commit changes first. Do not auto-stash.

### All branches exist

For each branch in the chain:
```bash
git rev-parse --verify refs/heads/<branch>
```

Report any missing branches before starting.

### Shared history check

Confirm each consecutive pair in the chain shares a common ancestor (i.e. has not become completely unrelated):

```bash
git merge-base <prev> <next>
```

If `merge-base` returns nothing (exit 1 with no output), the two branches share no history at all — report that link as broken and stop. **Do not** use `--is-ancestor` here: in the primary use case, `s1` has a new commit that `s2` has not yet included, so `s1` is intentionally NOT an ancestor of `s2`.

## Step 2: Show the current state

Print the stack before rebasing so the user can see what will change:

```bash
git log --oneline --graph <base>...<tip>
```

Follow it with:
> "Rebasing <tip> onto <base> with --update-refs. All intermediate branches will be updated. Proceed? (yes/no)"

Wait for confirmation.

## Step 3: Execute the rebase

```bash
git checkout <tip>
git rebase --update-refs <base>
```

Stream or capture output so conflicts can be detected.

### Conflict handling

If the rebase stops with conflicts:

1. Report which commit caused the conflict and which files are affected:
   ```bash
   git status
   git diff --cc
   ```
2. Tell the user:
   > "Rebase paused at '<commit-message>'. Conflicts in: <files>. Resolve the conflicts, then run `git rebase --continue`. To abandon, run `git rebase --abort`."
3. Do not attempt to resolve conflicts automatically.
4. Do not continue or abort without the user's instruction.

## Step 4: Verify and report

After a successful rebase, confirm the stack looks correct:

```bash
git log --oneline --graph <base>...<tip>
```

Print a success summary:

```
Stack rebased onto <base>:

  <base>  (<hash>)
    └── <s1>  (<hash>) — updated
        └── <s2>  (<hash>) — updated
            └── <tip>  (<hash>) — updated

Working branch: <tip>
```

## Constraints

- Never push to remote.
- Never create, modify, or comment on GitHub/GitLab issues or PRs.
- Never force-push, delete branches, or run `git reset --hard` without explicit user instruction.
- If a conflict arises, stop and report — do not auto-resolve.

## Additional Resources

- **`references/update-refs.md`** — Deep dive on how `--update-refs` works, version check parser, edge cases, manual fallback for git < 2.38, and conflict recovery.
