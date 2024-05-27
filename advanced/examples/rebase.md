# Rebasing in Git

## What is Rebasing?
Rebasing is a way to integrate changes from one branch into another. Unlike merging, rebasing re-applies commits on top of another base commit.

## Basic Rebase
Rebase the current branch onto `main`.

```bash
git checkout feature-branch
git rebase main
```

## Interactive Rebase
Interactively rebase the last few commits to clean up your commit history.

```bash
git rebase -i HEAD~3
```

## Resolving Conflicts
During a rebase, you might encounter conflicts. Resolve them as you would during a merge.

```bash
# Resolve conflicts in files
git add resolved-file
git rebase --continue
```

## Aborting a Rebase
If you need to stop the rebase process, you can abort it.

```bash
git rebase --abort
```
