# git-management

Local-only git workflow skills for developers who work with stacked branches and pull requests.

All operations are strictly local. No pushes, no remote writes, no issue or PR creation.

## Skills

### `stack-splitter`

Takes a large branch and splits it into a chain of smaller, independently mergeable stacked branches — each focused on one concern, buildable on its own, and shippable without the rest.

**When to use:** You've built something substantial on one branch and want to raise smaller, more reviewable PRs rather than one giant diff.

**Invoke by:**
- Typing `/git-management:stack-splitter` — Claude will ask for the branch name
- Or naturally: _"split feat/my-big-branch from main into a stack"_

Branch and base can be supplied inline; if omitted, the skill asks.

**What it does:**
1. Diffs the branch against the base and reads all commit messages
2. Asks about intent if the purpose is ambiguous
3. Plans a partition into focused chunks and presents it for your approval
4. Creates a sequential chain of stacked branches via cherry-pick
5. Reports the stack with PR ordering instructions

---

### `stack-rebase`

Rebases an entire chain of stacked branches in one operation using `git rebase --update-refs` (requires git ≥ 2.38), keeping all intermediate branch refs in sync.

**When to use:** You've committed to one branch in a stack and need all downstream branches to rebase onto it cleanly.

**Invoke by:**
- Typing `/git-management:stack-rebase` — Claude will ask for the chain
- Or naturally: _"rebase my stack: main → feat/s1 → feat/s2 → feat/s3"_

Supply the chain inline as `<base> <branch-1> ... <tip>`; if omitted, the skill asks.

**What it does:**
1. Validates git version, working tree cleanliness, and linear ancestry
2. Shows the current stack graph and asks for confirmation
3. Checks out the tip and runs `git rebase --update-refs <base>`
4. Reports the updated stack on success; pauses for manual resolution on conflict

---

## Requirements

- git ≥ 2.38 (for `stack-rebase`; `stack-splitter` works with older versions)
- Clean working tree before running either skill

## Installation

Install via the Claude marketplace, or reference this directory with `--plugin-dir`.
