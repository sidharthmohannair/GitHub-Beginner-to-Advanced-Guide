# Cherry-Picking in Git

## What is Cherry-Picking?
Cherry-picking in Git means to choose a commit from one branch and apply it onto another branch.

## Basic Cherry-Pick
Apply a specific commit to the current branch.

```bash
git checkout feature-branch
git cherry-pick <commit-hash>
```

## Resolving Conflicts
If there are conflicts during a cherry-pick, resolve them and continue.

```bash
# Resolve conflicts in files
git add resolved-file
git cherry-pick --continue
```

## Aborting a Cherry-Pick
If you need to stop the cherry-pick process, you can abort it.

```bash
git cherry-pick --abort
```
