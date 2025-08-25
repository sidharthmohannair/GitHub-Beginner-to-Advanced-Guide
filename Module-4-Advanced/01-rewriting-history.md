# Rewriting History: Rebase, Reset, Cherry-pick

## 🎯 Learning Goals
By the end of this lesson, you'll master:
- Interactive rebase for cleaning up commit history
- Different types of reset and when to use them
- Cherry-picking commits between branches
- Advanced history manipulation techniques
- When and why to rewrite history safely

---

## ⚠️ Important Warning

**Never rewrite history that has been pushed to shared branches!**

Rewriting history changes commit SHAs and can break other developers' work. Only rewrite:
- Local commits not yet pushed
- Personal feature branches
- With team agreement on shared branches

---

## 🔄 Interactive Rebase

Interactive rebase lets you modify commits before they become permanent.

### Basic Interactive Rebase

```bash
# Rebase last 3 commits interactively
git rebase -i HEAD~3

# Rebase from specific commit
git rebase -i abc123

# Rebase onto another branch
git rebase -i main
```

### Interactive Rebase Commands

```bash
# When you run git rebase -i, you'll see:
pick 1a2b3c4 Add user authentication
pick 5d6e7f8 Fix login validation
pick 9g0h1i2 Update documentation

# Available commands:
# pick (p) = use commit as-is
# reword (r) = use commit but edit message
# edit (e) = use commit but stop for amending  
# squash (s) = combine with previous commit
# fixup (f) = like squash but discard message
# drop (d) = remove commit entirely
```

### Practical Rebase Example

```bash
# Start with messy commit history
git log --oneline
# 9g0h1i2 Update documentation
# 5d6e7f8 Fix typo in login
# 1a2b3c4 Add login feature
# 3c4d5e6 Fix login bug
# 7f8g9h0 More login fixes
# abc123d Previous work

# Clean up with interactive rebase
git rebase -i abc123d

# Change to:
pick 1a2b3c4 Add login feature
squash 3c4d5e6 Fix login bug  
squash 7f8g9h0 More login fixes
squash 5d6e7f8 Fix typo in login
reword 9g0h1i2 Update documentation

# Result: Clean, logical commits
git log --oneline
# 2b3c4d5 Add comprehensive documentation
# 8e9f0g1 Add complete login feature with bug fixes
# abc123d Previous work
```

### Common Rebase Scenarios

#### Scenario 1: Squash Related Commits
```bash
# Multiple small commits for same feature
git rebase -i HEAD~4

# Change to:
pick 1a2b3c4 Add shopping cart feature
squash 5d6e7f8 Fix cart calculation
squash 9g0h1i2 Add cart validation
squash 3c4d5e6 Update cart styling

# Results in single, clean commit
```

#### Scenario 2: Fix Commit Messages
```bash
# Fix poorly written commit messages
git rebase -i HEAD~3

# Change:
pick 1a2b3c4 wip
reword 5d6e7f8 fix stuff
reword 9g0h1i2 more changes

# Update messages to:
# "Implement user profile editing"
# "Fix profile validation errors" 
# "Add profile image upload feature"
```

#### Scenario 3: Remove Unwanted Commits
```bash
# Remove commits that shouldn't exist
git rebase -i HEAD~5

# Change:
pick 1a2b3c4 Add feature A
drop 5d6e7f8 Add debug logging
pick 9g0h1i2 Add feature B
drop 3c4d5e6 Temporary test code
pick 7f8g9h0 Add feature C
```

---

## 🔄 Git Reset Deep Dive

### Three Types of Reset

```bash
# --soft: Move HEAD, keep staging and working directory
git reset --soft HEAD~1

# --mixed (default): Move HEAD, reset staging, keep working directory
git reset HEAD~1
git reset --mixed HEAD~1

# --hard: Move HEAD, reset staging AND working directory
git reset --hard HEAD~1
```

### Reset Visualization

```
Before reset:
HEAD -> A -> B -> C
        ↑    ↑    ↑
     commit commit commit

After git reset --soft HEAD~2:
HEAD ---------> A -> B -> C
                ↑    (orphaned)
             commit
Staging: B + C changes
Working: B + C changes

After git reset --mixed HEAD~2:
HEAD ---------> A -> B -> C  
                ↑    (orphaned)
             commit
Staging: (empty)
Working: B + C changes

After git reset --hard HEAD~2:
HEAD ---------> A -> B -> C
                ↑    (orphaned)
             commit  
Staging: (empty)
Working: (clean)
```

### Practical Reset Examples

#### Undo Last Commit (Keep Changes)
```bash
# Undo commit but keep changes staged
git reset --soft HEAD~1

# Make additional changes
echo "more changes" >> file.txt
git add file.txt

# Commit everything together
git commit -m "Better commit message with all changes"
```

#### Undo Multiple Commits
```bash
# View recent commits
git log --oneline -5

# Reset to specific commit
git reset --mixed abc123d

# Or reset by count
git reset --mixed HEAD~3
```

#### Emergency Reset (Dangerous!)
```bash
# Completely undo recent work
git reset --hard HEAD~2

# ⚠️ This permanently deletes uncommitted changes!
# Only use when you're absolutely sure
```

---

## 🍒 Cherry-Pick Mastery

Cherry-pick applies specific commits to your current branch.

### Basic Cherry-Pick

```bash
# Apply specific commit to current branch
git cherry-pick abc123d

# Cherry-pick multiple commits
git cherry-pick abc123d def456e ghi789f

# Cherry-pick range of commits
git cherry-pick abc123d..ghi789f
```

### Cherry-Pick Use Cases

#### Scenario 1: Hotfix from Feature Branch
```bash
# You're on main branch
git checkout main

# Critical bug fix exists in feature branch
git log --oneline feature/user-auth
# def456e Fix critical security vulnerability
# abc123d Add user authentication

# Cherry-pick only the security fix
git cherry-pick def456e

# Now main has the security fix without the full feature
```

#### Scenario 2: Selective Feature Migration
```bash
# Feature branch has multiple commits
git log --oneline feature/dashboard
# 9g0h1i2 Add dashboard styling
# 5d6e7f8 Add dashboard charts
# 1a2b3c4 Add dashboard layout
# 3c4d5e6 Add experimental feature (not ready)

# Cherry-pick only stable parts
git checkout main  
git cherry-pick 1a2b3c4  # Layout
git cherry-pick 5d6e7f8  # Charts
git cherry-pick 9g0h1i2  # Styling
# Skip experimental feature
```

### Handling Cherry-Pick Conflicts

```bash
# Cherry-pick with conflicts
git cherry-pick abc123d
# Conflict detected

# View conflict status
git status
# both modified:   src/app.js

# Resolve conflicts manually
vim src/app.js

# Stage resolved files
git add src/app.js

# Continue cherry-pick
git cherry-pick --continue

# Or abort if needed
# git cherry-pick --abort
```

---

## 🔧 Advanced History Techniques

### Filter-Branch (Dangerous but Powerful)

```bash
# Remove file from entire history
git filter-branch --tree-filter 'rm -f passwords.txt' HEAD

# Change author for all commits
git filter-branch --env-filter '
if [ "$GIT_AUTHOR_EMAIL" = "old@email.com" ]; then
    export GIT_AUTHOR_NAME="New Name"
    export GIT_AUTHOR_EMAIL="new@email.com"
fi
' HEAD
```

### BFG Repo-Cleaner (Better Alternative)

```bash
# Install BFG (safer than filter-branch)
# Download from: https://rtyley.github.io/bfg-repo-cleaner/

# Remove large files from history
java -jar bfg.jar --strip-blobs-bigger-than 50M my-repo.git

# Remove sensitive files
java -jar bfg.jar --delete-files passwords.txt my-repo.git

# Clean up
cd my-repo.git
git reflog expire --expire=now --all
git gc --prune=now --aggressive
```

### Reflog for Recovery

```bash
# View reflog (local action history)
git reflog
# Shows every HEAD movement

# Recover "lost" commits
git reflog
# abc123d HEAD@{0}: reset: moving to HEAD~3
# def456e HEAD@{1}: commit: important work

# Recover the lost work
git cherry-pick def456e
# or
git reset --hard def456e
```

---

## ⚡ Performance and Cleanup

### Git Maintenance Commands

```bash
# Clean up unnecessary files
git gc --aggressive --prune=now

# Remove unreachable objects
git prune

# Clean working directory
git clean -fd  # Remove untracked files and directories

# Verify repository integrity
git fsck --full
```

### Repository Statistics

```bash
# Check repository size
du -sh .git/

# Count objects
git count-objects -v

# Find largest files
git rev-list --objects --all | git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | grep '^blob' | sort -k3 -n -r | head -10
```

---

## 📋 History Rewriting Checklist

### Before Rewriting History

```markdown
- [ ] Confirm commits haven't been pushed to shared branches
- [ ] Create backup branch: `git branch backup-branch`
- [ ] Understand what changes you want to make
- [ ] Have a rollback plan
- [ ] Inform team members if affecting shared work
```

### Safe History Rewriting

```markdown
- [ ] Work on feature branches, not main
- [ ] Use `--force-with-lease` instead of `--force`
- [ ] Test changes before pushing
- [ ] Document major history changes
- [ ] Communicate with team about rewritten commits
```

---

## 🎓 Knowledge Check

**Question 1**: What's the difference between `git reset --soft` and `git reset --hard`?
<details>
<summary>Click to see answer</summary>
- `--soft`: Only moves HEAD pointer, keeps staging area and working directory intact
- `--hard`: Moves HEAD pointer AND resets staging area AND working directory to match
- `--soft` is safer as it preserves your changes, `--hard` permanently deletes them
</details>

**Question 2**: When should you use cherry-pick vs merge?
<details>
<summary>Click to see answer</summary>
- **Cherry-pick**: When you need specific commits from another branch (selective)
- **Merge**: When you want all commits from another branch (complete integration)
- Cherry-pick is good for hotfixes, merge is good for feature integration
</details>

**Question 3**: How do you recover commits after an accidental reset?
<details>
<summary>Click to see answer</summary>
1. Use `git reflog` to see recent HEAD movements
2. Find the commit SHA before the reset
3. Use `git cherry-pick SHA` or `git reset --hard SHA` to recover
4. Git keeps unreachable objects for ~30 days by default
</details>

---

## 🎯 Key Takeaways

1. **Never rewrite shared history** - only rewrite local/personal branches
2. **Interactive rebase** creates cleaner, more logical commit history  
3. **Reset types serve different purposes** - understand --soft, --mixed, --hard
4. **Cherry-pick is selective** - use for specific commits, not entire branches
5. **Always have a backup plan** - create backup branches before major changes
6. **Reflog is your safety net** - Git remembers everything for recovery
7. **Communication is key** - inform team of any history changes

---

## ➡️ What's Next?

Next, we'll explore Git hooks for automating workflows and enforcing standards.

**Next lesson**: [Git Hooks for Automation](02-git-hooks.md)

---

## 🎮 Practice Exercises

### Exercise 1: Interactive Rebase Practice
1. Create a branch with 5 messy commits
2. Use interactive rebase to clean up history
3. Practice squashing, rewording, and dropping commits
4. Compare before/after commit history

### Exercise 2: Cherry-Pick Scenarios
1. Create two feature branches with different commits
2. Practice cherry-picking specific commits between branches
3. Handle cherry-pick conflicts
4. Create a hotfix using cherry-pick

### Exercise 3: Reset Recovery
1. Make several commits
2. Practice different types of reset
3. Use reflog to recover "lost" commits
4. Create a recovery strategy document

---

## 📚 Additional Resources

- [Git Rebase Documentation](https://git-scm.com/book/en/v2/Git-Branching-Rebasing)
- [Git Reset Documentation](https://git-scm.com/docs/git-reset)
- [BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)
- [Oh Shit, Git!?!](https://ohshitgit.com/) - Common Git problems and solutions