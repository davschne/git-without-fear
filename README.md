# Git without Fear: demystifying reset, rebase, and other scary operations

A Git history is a directed, acyclic graph.
- Each commit is a node or vertex.
- Each branch is a path through the graph.
- It's a directed graph because nodes (commits) are ordered chronologically.

## Basic, safest approach: treating branches as append-only logs

If you treat branches as append-only logs, Git is very safe; you can't mess up any work that's already happened!

Each branch is a log. We create the log by adding commits.

We can make new logs from existing logs:
1. by copying the whole log (`git branch` from `HEAD`)
2. by copying part of the log (`git branch` from an earlier commit)

Going back in time is safe as long as we don't alter any existing logs.

Merges are a bit special. A merge combines two logs, preserving the precise chronology of commits, but it doesn't alter any commits. They retain their timestamps and unique identifiers (commit hashes).

## More powerful but more dangerous: operations that rewrite history

They're dangerous because they alter existing commits.

### Reset:
- `git reset` allows us to delete the end of a branch.
- Handy hint: For safety, copy the branch (`git branch`) before resetting.

### Rebase:
- `git rebase` is similar to `git merge` in that it combines two branches.
- Unlike a merge, though, a rebase alters existing commits.
- Rebase can be useful for keeping the graph tidy and orderly. After a merge, commits from the two branches that were merged may be interspersed, because Git always orders commits chronologically. This can make it more difficult for a reader of the history to understand what commits are logically related.
- A rebase, on the other hand, rewrites ALL the commits of one branch, moving them forward in time so that they appear to have been written after all the commits of the other branch.
- It alters the timestamps on the commits and gives them new hashes, but it doesn't change their content.
- Handy hints:
	- Be careful not to rebase on a shared branch (any branch multiple people are working on).
	- As you probably know already, after a PR has been approved, it's good practice to merge the target branch into your PR branch, so that you can resolve merge conflicts, if any. But a tidier way of accomplishing the same goal is to REBASE your PR branch onto the target branch.
	- If in doubt, a wise developer will talk about the rebase versus merge approach with his or her collaborators.

### Interactive Rebase:
- Actually quite different from "rebase". It's unfortunate the nomenclature makes them sound related. A better name for this operation might have been "Edit."
- An interactive rebase allows you to edit the commits of a single branch.
- Git provides a menu of editing choices:
	- Edit a commit message
	- Squash with previous (combine two commits into one)
	- Delete a commit
	- Reorder commits by moving them forward or backward in time.
