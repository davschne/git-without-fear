# Git without Fear: demystifying reset, rebase, and other scary operations



## Basic, safest approach: treating branches as append-only logs

If you treat branches as append-only logs, Git is very safe; you can't mess up any work that's already happened!

Each branch is a log. We create the log by adding commits.

We can make new logs from existing logs:
1. by copying the whole log (`git branch` from `HEAD`)
2. by copying part of the log (`git branch` from an earlier commit)

Going back in time is safe as long as we don't alter any existing logs.

### Merge: safely combine two branches
`git merge` combines two logs, but it doesn't alter any commits. They retain their timestamps and unique identifiers (commit hashes). Depending on how you're displaying your history, you may see commits from the two branches interspersed or sorted in some other way (confusing!).

### Revert: safely undo an earlier commit
`git revert` undoes an earlier commit (not necessarily the most recent) by making a new commit that reverses the changes in the target commit.

## More powerful but more dangerous: operations that rewrite history

They're dangerous because they alter existing commits.

Handy hint: For safety, copy the branch (`git branch`) before rewriting history.

### Reset:

`git reset` allows us to delete the end of a branch.

Types of resets:
- soft
- mixed
- hard

### Rebase:

- `git rebase` is similar to `git merge` in that it combines two branches.
- Unlike a merge, though, a rebase alters existing commits.
- Rebase can be useful for keeping the graph tidy and orderly. Merging introduces special "merge" commits that can clutter up the history.
- A rebase, on the other hand, rewrites ALL the commits of one branch, moving them forward in time so that they appear to have been written after all the commits of the other branch.
- It alters the timestamps on the commits and gives them new hashes, but it doesn't change their content (unless using interactive rebase).
- Handy hints:
	- Be careful not to rebase on a shared branch (any branch multiple people are working on).
	- As you probably know already, after a PR has been approved, it's good practice to merge the target branch into your PR branch, so that you can resolve merge conflicts, if any. But a tidier way of accomplishing the same goal is to REBASE your PR branch onto the target branch.
	- If in doubt, a wise developer will talk about the rebase versus merge approach with his or her collaborators.

### Interactive Rebase:

Two different scenarios:
1. Combine two branches, as with an ordinary rebase, but with the ability to edit commits as they're replayed.
2. Edit the commits of a single branch.

Git provides a menu of editing choices:
- Edit a commit message
- Squash with previous (combine two commits into one)
- Delete a commit
- Reorder commits by moving them forward or backward in time.
