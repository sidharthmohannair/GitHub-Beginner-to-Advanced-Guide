# Undoing Changes & .gitignore

## 🎯 Learning Goals
By the end of this lesson, you'll master:
- Different ways to undo changes in Git
- When to use each undo method
- Creating and managing .gitignore files
- Best practices for preventing unwanted files in your repository

---

## 🔄 The Three States of Git Files

Before learning to undo changes, understand where files can be:

```
┌─────────────────┐    git add     ┌─────────────────┐    git commit    ┌─────────────────┐
│ Working         │─────────────→  │ Staging Area    │─────────────→   │ Repository      │
│ Directory       │                │ (Index)         │                 │ (Committed)     │
│ (Modified)      │                │ (Staged)        │                 │ (History)       │
└─────────────────┘                └─────────────────┘                 └─────────────────┘
```

Each state has different undo methods!

---

## 🎬 Setting Up Practice Environment

Let's create a project to practice undoing changes:

```bash
mkdir undo-practice
cd undo-practice
git init

# Create initial files
cat > app.py << 'EOF'
def calculate_sum(a, b):
    return a + b

def calculate_product(a, b):
    return a * b

if __name__ == "__main__":
    print("Calculator App")
    print(f"Sum: {calculate_sum(5, 3)}")
    print(f"Product: {calculate_product(5, 3)}")
EOF

cat > config.py << 'EOF'
DATABASE_URL = "sqlite:///app.db"
DEBUG = True
SECRET_KEY = "dev-key-123"
EOF

# Initial commit
git add .
git commit -m "Add initial calculator app with config"
```

---

## ↩️ Undoing Changes in Working Directory

### Scenario: You've modified files but haven't staged them yet

```bash
# Make some changes
echo "# This is a mistake" >> app.py
echo "BROKEN_CONFIG = True" >> config.py

# Check what changed
git status
git diff
```

### Method 1: Restore Specific Files
```bash
# Undo changes to specific file
git restore app.py

# Or using older syntax (still works)
git checkout -- app.py

# Check that changes were undone
git diff app.py  # Should show no differences
```

### Method 2: Restore All Modified Files
```bash
# Undo all changes in working directory
git restore .

# Or restore specific types of files
git restore "*.py"

# Check status
git status  # Should show "working tree clean"
```

### ⚠️ Warning: These Operations Are Destructive
Once you restore files, the changes are permanently lost!

---

## 📦 Undoing Staged Changes (Staging Area)

### Scenario: You've staged files but haven't committed yet

```bash
# Make changes and stage them
echo "def new_function(): pass" >> app.py
git add app.py

# Check status
git status
# Shows: Changes to be committed: modified: app.py
```

### Method 1: Unstage Specific Files
```bash
# Remove from staging area (keep changes in working directory)
git restore --staged app.py

# Or using older syntax
git reset app.py

# Check status
git status
# Shows: Changes not staged for commit: modified: app.py
```

### Method 2: Unstage All Files
```bash
# Stage multiple files
git add .

# Unstage everything
git restore --staged .

# Or using reset
git reset
```

### Method 3: Unstage and Discard Changes
```bash
# Stage some changes
echo "# Another mistake" >> app.py
git add app.py

# Unstage AND discard changes in one command
git restore --staged --worktree app.py

# Check status
git status  # Should show "working tree clean"
```

---

## 💾 Undoing Committed Changes

### Scenario 1: Fix the Last Commit (Amend)

```bash
# Make a commit with a typo
echo "def divide(a, b): return a / b" >> app.py
git add app.py
git commit -m "Add divison function"  # Oops, typo in message

# Fix the commit message
git commit --amend -m "Add division function"

# Or add more changes to the last commit
echo "# Fixed division function" >> app.py
git add app.py
git commit --amend -m "Add division function with comment"

# View the updated commit
git log --oneline -1
```

### Scenario 2: Undo Last Commit (Keep Changes)

```bash
# Make a commit we want to undo
echo "def bad_function(): pass" >> app.py
git add app.py
git commit -m "Add bad function"

# Undo the commit but keep the changes
git reset --soft HEAD~1

# Check status
git status
# Shows: Changes to be committed: modified: app.py

# The changes are still staged, you can modify and recommit
```

### Scenario 3: Undo Last Commit (Discard Changes)

```bash
# Make another commit
echo "def another_bad_function(): pass" >> app.py
git add app.py
git commit -m "Add another bad function"

# Completely remove the commit and its changes
git reset --hard HEAD~1

# Check status
git status  # Should show "working tree clean"

# Check file content
cat app.py  # The bad function should be gone
```

### Understanding Reset Options

| Command | Commit History | Staging Area | Working Directory |
|---------|---------------|--------------|-------------------|
| `--soft` | Reset | Keep | Keep |
| `--mixed` (default) | Reset | Reset | Keep |
| `--hard` | Reset | Reset | Reset |

---

## 🔄 Undoing Multiple Commits

### Reset to Specific Commit

```bash
# View commit history
git log --oneline

# Reset to a specific commit (replace abc123 with actual hash)
git reset --hard abc123

# Or reset to 3 commits ago
git reset --hard HEAD~3
```

### Revert Commits (Safe for Shared Repositories)

```bash
# Create a new commit that undoes a previous commit
git revert HEAD

# Revert multiple commits
git revert HEAD~2..HEAD

# Revert specific commit by hash
git revert abc123
```

**Key Difference:**
- **Reset**: Changes history (dangerous if already pushed)
- **Revert**: Creates new commits to undo changes (safe for shared repos)

---

## 📋 Understanding .gitignore

`.gitignore` tells Git which files to ignore. These files won't be tracked, staged, or committed.

### Creating Your First .gitignore

```bash
# Create .gitignore file
cat > .gitignore << 'EOF'
# Python
__pycache__/
*.pyc
*.pyo
*.pyd
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg

# Virtual Environment
venv/
env/
ENV/

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Application specific
*.log
config-local.py
.env
EOF

# Add and commit .gitignore
git add .gitignore
git commit -m "Add comprehensive .gitignore for Python project"
```

### Testing .gitignore

```bash
# Create files that should be ignored
mkdir __pycache__
touch __pycache__/test.pyc
touch debug.log
mkdir .vscode
touch .vscode/settings.json

# Check git status
git status
# Should NOT show the ignored files

# Create a file that should be tracked
echo "print('Hello World')" > hello.py
git status
# Should show hello.py but not the ignored files
```

---

## 🎯 .gitignore Patterns and Rules

### Basic Patterns

```bash
# Ignore specific file
config-secret.py

# Ignore all files with extension
*.log
*.tmp

# Ignore directory
node_modules/
build/

# Ignore files in specific directory
logs/*.log

# Ignore all .txt files except important.txt
*.txt
!important.txt
```

### Advanced Patterns

```bash
# Ignore all files in any directory named temp
**/temp/

# Ignore .log files in root directory only
/*.log

# Ignore files matching pattern in any subdirectory
**/*.cache

# Character ranges
*.[oa]    # Matches *.o and *.a
*.[0-9]   # Matches files ending with any digit
```

### Global .gitignore

Create a global gitignore for your system:

```bash
# Create global gitignore
git config --global core.excludesfile ~/.gitignore_global

# Add common patterns
cat > ~/.gitignore_global << 'EOF'
# OS generated files
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# Editor files
*.swp
*.swo
*~
.vscode/
.idea/

# Temporary files
*.tmp
*.temp
EOF
```

---

## 🚨 Handling Files Already Tracked

### Problem: File is in .gitignore but still tracked

```bash
# If you add a file to .gitignore after it's already tracked
echo "secret-api-key.txt" >> .gitignore
echo "my-secret-key" > secret-api-key.txt

# Git still tracks it!
git status  # Will show secret-api-key.txt as modified

# Solution: Remove from tracking but keep file
git rm --cached secret-api-key.txt
git commit -m "Stop tracking secret API key file"

# Now .gitignore will work
echo "new-secret" > secret-api-key.txt
git status  # File won't appear
```

### Stop Tracking Directory

```bash
# Remove directory from tracking
git rm -r --cached logs/
git commit -m "Stop tracking logs directory"

# Add to .gitignore
echo "logs/" >> .gitignore
git add .gitignore
git commit -m "Ignore logs directory"
```

---

## 🔍 Common .gitignore Templates

### Node.js Project
```bash
# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Build outputs
dist/
build/

# Environment variables
.env
.env.local
.env.development.local
.env.test.local
.env.production.local
```

### Python Project
```bash
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class

# Virtual environments
venv/
env/
ENV/

# Package files
*.egg-info/
dist/
build/
```

### Java Project
```bash
# Compiled class files
*.class

# Package files
*.jar
*.war
*.ear

# Build directories
target/
build/

# IDE files
.project
.classpath
.settings/
```

---

## 🛠️ Advanced Undo Scenarios

### Scenario 1: Undo Merge Commit

```bash
# If you just merged and want to undo
git reset --hard HEAD~1

# Or if merge created conflicts and you want to abort
git merge --abort
```

### Scenario 2: Recover Deleted Commits

```bash
# View all recent actions
git reflog

# Find the commit you want to recover
# Restore it using its hash
git reset --hard abc123
```

### Scenario 3: Interactive Rebase to Fix History

```bash
# Fix last 3 commits interactively
git rebase -i HEAD~3

# This opens an editor where you can:
# - pick (use commit)
# - reword (change commit message)
# - edit (modify commit)
# - squash (combine with previous)
# - drop (remove commit)
```

---

## 🎓 Best Practices

### For Undoing Changes
1. **Check status first**: Always run `git status` before undoing
2. **Start conservative**: Try `--soft` reset before `--hard`
3. **Use revert for shared repos**: Don't rewrite public history
4. **Test in a copy**: Practice dangerous operations on a copy first

### For .gitignore
1. **Add early**: Create .gitignore before first commit
2. **Use templates**: Start with language-specific templates
3. **Be specific**: Don't ignore too broadly
4. **Document exceptions**: Comment why specific files are ignored

```bash
# Good .gitignore with comments
# IDE files (team uses different editors)
.vscode/
.idea/

# Build artifacts (regenerated during build)
dist/
build/

# Environment-specific configs (contains secrets)
.env
config-local.py
```

---

## 🎓 Knowledge Check

**Question 1**: What's the difference between `git reset --soft` and `git reset --hard`?
<details>
<summary>Click to see answer</summary>
- `--soft`: Only moves the branch pointer, keeps staging area and working directory intact
- `--hard`: Moves branch pointer AND resets staging area AND working directory to match
</details>

**Question 2**: When should you use `git revert` instead of `git reset`?
<details>
<summary>Click to see answer</summary>
Use `git revert` when:
- The commits have already been pushed to a shared repository
- You want to preserve the history of what happened
- You're working with others who might have based work on those commits
</details>

**Question 3**: A file is being tracked by Git but you want to ignore it. What do you do?
<details>
<summary>Click to see answer</summary>
1. Add the file to `.gitignore`
2. Remove it from Git's tracking: `git rm --cached filename`
3. Commit the change: `git commit -m "Stop tracking filename"`
</details>

**Question 4**: What's the difference between `.gitignore` and `git rm --cached`?
<details>
<summary>Click to see answer</summary>
- `.gitignore`: Prevents files from being tracked in the future
- `git rm --cached`: Stops tracking files that are already being tracked
- You often need both when you want to ignore files that were previously committed
</details>

---

## 🎯 Key Takeaways

1. **Know the three states**: Working directory, staging area, repository
2. **Choose the right undo method**: Based on where your changes are
3. **Be careful with destructive operations**: `--hard` resets can't be undone
4. **Use .gitignore proactively**: Set it up before committing sensitive files
5. **Revert vs Reset**: Revert for shared history, reset for local history
6. **Practice makes perfect**: Try these operations in a test repository first

---

## ➡️ What's Next?

You now have a solid foundation in Git fundamentals! Next, we'll explore how to work with branches and collaborate with others.

**Next module**: [Module 2: Branching & Collaboration](../Module-2-Branching/)

---

## 🎮 Practice Exercises

### Exercise 1: Undo Practice
1. Create a test repository
2. Make several commits
3. Practice each type of reset (soft, mixed, hard)
4. Practice amending commits
5. Use `git reflog` to recover "lost" commits

### Exercise 2: .gitignore Mastery
1. Create a project with multiple file types
2. Create a comprehensive .gitignore
3. Test that ignored files don't appear in `git status`
4. Practice removing already-tracked files from Git

### Exercise 3: Real-World Scenario
1. Simulate accidentally committing a password file
2. Remove it from history completely
3. Add it to .gitignore
4. Verify it stays ignored

---

## 📚 Additional Resources

- [Git Reset Documentation](https://git-scm.com/docs/git-reset)
- [gitignore.io](https://gitignore.io) - Generate .gitignore files
- [GitHub's .gitignore Templates](https://github.com/github/gitignore)
- [Oh Shit, Git!?!](https://ohshitgit.com/) - Common Git problems and solutions