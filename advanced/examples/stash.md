# Stashing in Git

## What is Stashing?
Stashing allows you to save changes in a dirty working directory temporarily without committing them.

## Basic Stash
Save your changes to a new stash.

```bash
git stash
```

## Applying a Stash
Apply the most recent stash.

```bash
git stash apply
```

## Listing Stashes
List all the stashes you have saved.

```bash
git stash list
```

## Applying a Specific Stash
Apply a specific stash from the list.

```bash
git stash apply stash@{2}
```

## Dropping a Stash
Remove a specific stash from the list.

```bash
git stash drop stash@{2}
```

## Popping a Stash
Apply and remove the most recent stash.

```bash
git stash pop
```
