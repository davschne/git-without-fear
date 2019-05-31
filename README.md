# Git without Fear: demystifying reset, rebase, and other scary operations

## Basic, safest approach: treating branches as append-only logs

If you treat branches as append-only logs, Git is very safe; you can't mess up any work that's already happened!

Append-only means we only add new commits; we don't alter or remove existing commits.

We can make new branches from existing branches:
1. by copying the whole branch (`git branch` from `HEAD`)
2. by copying part of the branch (`git branch` from an earlier commit)

Going back in time is safe as long as we don't alter any existing commits.

### Revert: safely undo an earlier commit
`git revert` undoes an earlier commit (not necessarily the most recent) by making a new commit that reverses the changes in the target commit.

### Merge: safely combine two branches
`git merge` combines two branches, but it doesn't alter any commits. They retain their timestamps and unique identifiers (commit hashes).

## More powerful but more dangerous: operations that rewrite history

They're dangerous because they alter existing commits. If no other branch includes these commits, your prior work can be lost!

Handy hint: For safety, copy the branch (`git branch`) before rewriting history.

### Reset:

`git reset` allows us to delete the commit(s) at the end of a branch. It undoes the commits but (except for a hard reset) it doesn't change the contents of our working directory.

Types of resets:
- `soft`: undoes commit(s) but stages the changes they contained (SAFE)
- `mixed`: undoes commit(s) and clears the staging area (SAFE)
- `hard`: undoes commit(s) and discards all their changes (UNSAFE!)

### Rebase:

- `git rebase` is similar to `git merge` in that it combines two branches.
- Unlike a merge, though, a rebase alters existing commits.
- Conceptually, a rebase detaches part of your commit history and reattaches it on a new "base" or "parent" commit. It does this by starting at the new base commit, then applying each commit on top of it.
- Rebase can be useful for keeping the graph tidy and orderly.
	- Merging introduces special "merge" commits that can clutter up the history.
	- After merging, depending on how you're displaying your history, you may see commits from the two branches either interspersed or sorted in some other way (confusing!).
- A rebase, on the other hand, rewrites ALL the commits of one branch, moving them forward in time so that they appear to have been written after all the commits of the base branch.
- It alters the timestamps on the commits and gives them new hashes, but it doesn't change their content (unless using interactive rebase).
- Handy hints:
	- Be careful not to rebase on a shared branch (any branch multiple people are working on).
	- If a conflict occurs during a rebase (similar to a merge conflict), the rebase will pause, and you'll need to manually resolve the conflict. After doing so, don't commit again; CONTINUE the rebase instead.
	- As you probably know already, after a PR has been approved, it's good practice to merge the target branch into your PR branch, so that you can resolve merge conflicts, if any. But a tidier way of accomplishing the same goal is to REBASE your PR branch onto the target branch.
	- If in doubt, a wise developer will talk about the rebase versus merge approach with his or her collaborators.

### Interactive Rebase:

An interactive rebase allows far more flexibility.

Git provides a menu of choices:
- "reword" a commit message
- "squash" or "fixup" (combine two commits into one)
- "drop" (delete) a commit
- reorder commits
- "edit" the content of commits or add new commits (pauses replay)
