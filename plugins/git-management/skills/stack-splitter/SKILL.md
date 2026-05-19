---
name: stack-splitter
description: This skill should be used when the user asks to "split a branch", "break up a PR", "stack my changes", "create stacked branches", "decompose a branch into smaller PRs", "slice this branch into stacks", "this branch has grown too large", or wants to turn a large branch into a chain of independently mergeable, buildable, and shippable pieces. Operates entirely locally — never pushes or writes to remote.
allowed-tools:
  - Bash
  - AskUserQuestion
  - Read
---

Split a source branch into a sequential chain of stacked branches, each containing one focused, independently mergeable slice of the original work.

## Inputs

Extract source branch and base branch from the user's message if present (e.g. "split feat/my-branch from main"). If not supplied:
- **Source branch** — read the current branch via `git rev-parse --abbrev-ref HEAD`.
- **Base branch** — auto-detect (see Step 1). If detection fails, ask the user.

Both must be local branches. Validate they exist before proceeding.

## Step 1: Detect base branch

If not supplied, detect the default branch:

```bash
# Try origin HEAD pointer first
git symbolic-ref refs/remotes/origin/HEAD 2>/dev/null | sed 's@^refs/remotes/origin/@@'
```

If that returns nothing, check for `main`, then `master`:

```bash
git show-ref --verify --quiet refs/heads/main && echo main || \
git show-ref --verify --quiet refs/heads/master && echo master
```

If neither exists, ask the user which branch to treat as the base.

## Step 2: Pre-flight checks

Before gathering context or presenting a plan, verify these pass or stop early:

1. Working tree is clean:
   ```bash
   git status --porcelain
   ```
   If dirty, tell the user to stash or commit first. Do not auto-stash.

2. Source branch exists:
   ```bash
   git rev-parse --verify refs/heads/<source-branch>
   ```

## Step 3: Gather context

```bash
# Summarise commits
git log --oneline <base>..<source-branch>

# Full diff for content analysis
git diff <base>...<source-branch>
```

Read every commit message. Extract any ticket key from the branch name (e.g. `GX-27307` from `feat/GX-27307-payments`) — use it as a prefix in generated branch names so the stack stays traceable (see Step 4 naming rules).

Identify:
- The overall purpose of the branch
- Natural seams: independent concerns, layer separations (data model / API / UI), or risk tiers

If the purpose is ambiguous after reading commits and the diff, ask the user one question:

> "Can you briefly describe what this branch is trying to achieve?"

Do not ask more than one question — read the diff for everything else.

## Step 4: Plan the stack

Produce a breakdown plan as a markdown table:

| Part | Branch name | Commits | Description |
|------|-------------|---------|-------------|
| 1 | `feat/GX-27307-s1-data-model` | `abc1234`, `def5678` | Data model changes |
| 2 | `feat/GX-27307-s2-api-endpoints` | `bca9012` | API endpoint implementation |
| 3 | `feat/GX-27307-s3-ui-layer` | `cde3456` | UI layer |

**Branch naming:** `<base-prefix>/<ticket-key>-s<N>-<slug>` where:
- `<base-prefix>` is the type prefix from the source branch (e.g. `feat`, `fix`, `chore`)
- `<ticket-key>` is any ticket key found in the source branch name; omit if none
- `<slug>` is two or three lowercase kebab-case words describing this part's content

**Rules for a good partition:**
- Each part must build and pass CI independently (no dangling imports or missing migrations).
- Each part must be mergeable without the next part being present.
- Prefer separating by layer (schema → service → controller → UI) or by feature sub-domain.
- Keep the original commit order within each part.

Present the plan and ask:

> "Does this breakdown look right? Reply 'yes' to proceed, or describe any changes."

Wait for confirmation before creating any branches.

## Step 5: Create the stack

For each part in order:

```bash
# Start each part from base (part 1) or previous part (parts 2+)
git checkout -b <part-branch> <parent>

# Cherry-pick the commits for this part
git cherry-pick <hash1> [<hash2> ...]
```

Where `<parent>` for part 1 is `<base>`, and for part N is `<part-N-1-branch>`.

If cherry-pick conflicts arise on any part, stop immediately:
- Abort: `git cherry-pick --abort`
- Check out the original source branch: `git checkout <source-branch>`
- Report which part failed and which commits conflicted
- Note that any branches successfully created for earlier parts still exist — the user must delete them (`git branch -D <branch>`, see `references/git-commands.md`) before re-running with an adjusted plan
- Advise the user to resolve the conflict in the source branch first, or adjust the partition so the conflicting commit lands in a different part

After all parts succeed, check out the original source branch to leave the working tree in its original state.

## Step 6: Report

Print a summary of what was created:

```
Stack created from <source-branch> (base: <base>):

  <base>
    └── <s1-branch>   (<N> commits) — <description>
        └── <s2-branch>   (<N> commits) — <description>
            └── <s3-branch>   (<N> commits) — <description>

To raise PRs, open each branch sequentially:
  1. <s1-branch> → <base>
  2. <s2-branch> → <s1-branch>
  3. <s3-branch> → <s2-branch>

Use git-management:stack-rebase to keep the stack in sync after changes.
```

## Constraints

- Never push to remote.
- Never create, modify, or comment on GitHub/GitLab issues or PRs.
- Never force-push or delete branches without explicit user instruction.
- If something is ambiguous, ask rather than guess.

## Additional Resources

- **`references/git-commands.md`** — Reference for cherry-pick ranges, conflict resolution, and branch ancestry inspection.
