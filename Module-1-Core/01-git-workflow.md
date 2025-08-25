# Git Workflow: Add, Commit, Push, Pull

## 🎯 Learning Goals
By the end of this lesson, you'll master:
- The four fundamental Git operations
- When and why to use each command
- Best practices for commit messages
- Understanding the Git workflow cycle
- Troubleshooting common workflow issues

---

## 🔄 The Git Workflow Cycle

The core Git workflow follows a simple pattern that you'll repeat hundreds of times:

```
📝 Edit Files → 📦 Stage Changes → 💾 Commit → 🚀 Push → 🔄 Pull
```

Let's explore each step in detail.

---

## 📝 Step 1: Edit Files (Working Directory)

This is where you spend most of your time - actually writing code, editing files, and making changes.

### Example Project Setup
Let's create a todo app to practice the workflow:

```bash
# Create a new project
mkdir todo-app
cd todo-app
git init

# Create initial files
cat > app.js << 'EOF'
// Simple Todo App
let todos = [];

function addTodo(text) {
    todos.push({
        id: Date.now(),
        text: text,
        completed: false
    });
}

function displayTodos() {
    console.log('Todo List:');
    todos.forEach(todo => {
        const status = todo.completed ? '✅' : '⭕';
        console.log(`${status} ${todo.text}`);
    });
}

// Add some initial todos
addTodo('Learn Git workflow');
addTodo('Practice with real project');
displayTodos();
EOF

cat > index.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Todo App</title>
</head>
<body>
    <h1>My Todo App</h1>
    <div id="todo-list"></div>
    <script src="app.js"></script>
</body>
</html>
EOF
```

### Check Your Changes
```bash
# See what files exist
ls -la

# Check Git status
git status
```

**What you'll see:**
```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        app.js
        index.html

nothing added to commit but untracked files present (use "git add" to track)
```

---

## 📦 Step 2: Stage Changes (`git add`)

Staging is like putting items in a shopping cart before checkout. You're saying "these are the changes I want to include in my next commit."

### Adding Individual Files
```bash
# Add one file at a time
git add app.js

# Check status
git status
```

**Output:**
```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   app.js

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        index.html
```

### Adding Multiple Files
```bash
# Add remaining files
git add index.html

# Or add everything at once
git add .
# The dot (.) means "add all changes in current directory and subdirectories"

# Check final status
git status
```

**Output:**
```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   app.js
        new file:   index.html
```

### Staging Best Practices

#### 1. Review Before Staging
```bash
# See exactly what changed
git diff
# Shows changes in unstaged files

# See what's staged
git diff --cached
# Shows changes that are staged for commit
```

#### 2. Selective Staging
```bash
# Stage only specific parts of a file (interactive mode)
git add -p filename.js
# Git will show you chunks and ask which to stage

# Stage only modified files (not new or deleted)
git add -u

# Stage everything including new files
git add -A
```

---

## 💾 Step 3: Commit Changes (`git commit`)

A commit creates a permanent snapshot of your staged changes. Think of it as taking a photograph of your project at this moment.

### Making Your First Commit
```bash
# Commit with a message
git commit -m "Add initial todo app structure with HTML and JavaScript"

# Check what happened
git log
git status
```

**Output:**
```bash
git log
commit a1b2c3d4e5f6... (HEAD -> main)
Author: Your Name <your.email@example.com>
Date: Mon Jan 15 14:30:00 2024 -0500

    Add initial todo app structure with HTML and JavaScript
```

### Commit Message Best Practices

#### ✅ Good Commit Messages
```bash
# Clear and descriptive
git commit -m "Add user authentication with email validation"

# Use imperative mood (like giving a command)
git commit -m "Fix navigation menu alignment on mobile devices"

# Include context when helpful
git commit -m "Update API endpoint URL for production deployment"

# Reference issues/tickets when relevant
git commit -m "Fix memory leak in image processing (closes #123)"
```

#### ❌ Bad Commit Messages
```bash
# Too vague
git commit -m "update"
git commit -m "fix stuff"
git commit -m "changes"

# Past tense (avoid this)
git commit -m "Fixed the bug"
git commit -m "Added new feature"

# Too long for first line
git commit -m "This commit adds a new feature that allows users to do something really complex and also fixes a bunch of other things"
```

#### The 7 Rules of Great Commit Messages
1. **Separate subject from body with a blank line**
2. **Limit subject line to 50 characters**
3. **Capitalize the subject line**
4. **Don't end subject line with a period**
5. **Use imperative mood in subject line**
6. **Wrap body at 72 characters**
7. **Use body to explain what and why, not how**

Example of a detailed commit:
```bash
git commit -m "Add email validation to signup form

Users were able to register with invalid email addresses,
causing issues with password recovery and notifications.

This change adds client-side validation using regex pattern
and server-side validation using email verification service.

Fixes #456"
```

---

## 🚀 Step 4: Push to Remote (`git push`)

Pushing uploads your commits to a remote repository (like GitHub), creating a backup and making your code available to collaborators.

### Setting Up Remote (First Time)
```bash
# Add GitHub repository as remote
git remote add origin https://github.com/yourusername/todo-app.git

# Verify remote was added
git remote -v
# Shows: origin  https://github.com/yourusername/todo-app.git (fetch/push)

# Push and set up tracking
git push -u origin main
# -u flag sets up tracking between local and remote branch
```

### Regular Pushing
```bash
# After the first push, you can simply use:
git push

# Or be explicit:
git push origin main
```

### Understanding Push Output
```
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (5/5), 1.23 KiB | 1.23 MiB/s, done.
Total 5 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/yourusername/todo-app.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

---

## 🔄 Step 5: Pull from Remote (`git pull`)

Pulling downloads changes from the remote repository and merges them into your local branch.

### When to Pull
```bash
# At the start of each work session
git pull origin main

# Before pushing your changes (good practice)
git pull origin main
git push origin main

# When collaborating with others
git pull origin main  # Get their latest changes
```

### What Pull Actually Does
`git pull` is actually two commands combined:
1. `git fetch` - Downloads changes from remote
2. `git merge` - Merges those changes into your current branch

```bash
# These two commands are equivalent to git pull:
git fetch origin main
git merge origin/main
```

---

## 🎯 Complete Workflow Example

Let's practice the complete workflow by adding a new feature to our todo app:

### 1. Pull Latest Changes (Good Habit)
```bash
git pull origin main
```

### 2. Edit Files
```bash
# Add a new function to app.js
cat >> app.js << 'EOF'

function completeTodo(id) {
    const todo = todos.find(t => t.id === id);
    if (todo) {
        todo.completed = !todo.completed;
        console.log(`Todo "${todo.text}" marked as ${todo.completed ? 'completed' : 'incomplete'}`);
    }
}

function deleteTodo(id) {
    const index = todos.findIndex(t => t.id === id);
    if (index > -1) {
        const deleted = todos.splice(index, 1)[0];
        console.log(`Deleted todo: "${deleted.text}"`);
    }
}

// Test the new functions
addTodo('Test the new complete function');
const testTodo = todos[todos.length - 1];
completeTodo(testTodo.id);
displayTodos();
EOF
```

### 3. Check Status and Diff
```bash
# See what changed
git status
git diff app.js
```

### 4. Stage Changes
```bash
git add app.js
git status
```

### 5. Commit Changes
```bash
git commit -m "Add complete and delete functions for todo items

- Add completeTodo() function to toggle completion status
- Add deleteTodo() function to remove items from list
- Include test calls to verify functionality"
```

### 6. Push to Remote
```bash
git push origin main
```

### 7. Verify on GitHub
Visit your repository on GitHub to see the changes appear online.

---

## 🛠️ Advanced Workflow Commands

### Viewing History
```bash
# View commit history
git log

# Concise one-line format
git log --oneline

# Show last 5 commits
git log -5

# Show commits with file changes
git log --stat

# Beautiful graph view
git log --oneline --graph --decorate
```

### Checking Differences
```bash
# See unstaged changes
git diff

# See staged changes
git diff --cached

# Compare with specific commit
git diff HEAD~1

# Compare two commits
git diff commit1 commit2
```

### Working with Specific Files
```bash
# Stage specific file
git add filename.js

# See changes in specific file
git diff filename.js

# See history of specific file
git log filename.js

# Remove file from staging
git reset filename.js
```

---

## 🚨 Troubleshooting Common Issues

### 1. "Nothing to commit, working tree clean"
**Problem**: You forgot to make changes or stage them.
```bash
# Check if you have unstaged changes
git status
git diff

# Stage your changes
git add .
```

### 2. "Updates were rejected"
**Problem**: Remote has changes you don't have locally.
```bash
# Solution: Pull first, then push
git pull origin main
git push origin main
```

### 3. "Merge conflict"
**Problem**: Git can't automatically merge changes.
```bash
# Check which files have conflicts
git status

# Edit files to resolve conflicts (look for <<<<<<, ======, >>>>>> markers)
# Then stage and commit
git add resolved-file.js
git commit -m "Resolve merge conflict in app.js"
```

### 4. Accidentally Committed Wrong Files
```bash
# Remove file from last commit (keep file in working directory)
git reset --soft HEAD~1
git reset filename.js
git commit -m "Correct commit message"

# Or amend the last commit
git add correct-file.js
git commit --amend -m "Updated commit message"
```

---

## 📊 Workflow Status Commands

### Essential Status Commands
```bash
# Most important command - use frequently!
git status

# Quick overview
git status -s  # Short format

# See what branch you're on
git branch

# See remote connections
git remote -v

# Check if your branch is up to date
git status -uno  # Don't show untracked files
```

---

## 🎓 Knowledge Check

**Question 1**: What's the difference between `git add .` and `git add -A`?
<details>
<summary>Click to see answer</summary>
- `git add .` adds all changes in current directory and subdirectories
- `git add -A` adds all changes in the entire repository (including files in parent directories)
For most cases, they do the same thing when run from the repository root.
</details>

**Question 2**: What does `git pull` actually do?
<details>
<summary>Click to see answer</summary>
`git pull` combines two operations:
1. `git fetch` - downloads changes from remote
2. `git merge` - merges those changes into your current branch
</details>

**Question 3**: When should you write longer commit messages?
<details>
<summary>Click to see answer</summary>
When:
- The change is complex and needs explanation
- You want to document the reasoning behind the change
- The change fixes a bug and you want to explain the root cause
- You're making architectural decisions that affect other developers
</details>

**Question 4**: What happens if you `git push` without committing?
<details>
<summary>Click to see answer</summary>
Nothing gets pushed. Git only pushes committed changes. If you have staged or unstaged changes that aren't committed, they stay local until you commit them.
</details>

---

## 🎯 Key Takeaways

1. **The workflow is cyclical**: Edit → Stage → Commit → Push → Pull
2. **Use `git status` frequently** to understand what's happening
3. **Stage changes deliberately** - review what you're committing
4. **Write clear commit messages** that explain what and why
5. **Pull before pushing** when collaborating with others
6. **One commit per logical change** - don't mix unrelated changes

---

## ➡️ What's Next?

Now that you've mastered the core Git workflow, let's explore the relationship between local and remote repositories in more detail.

**Next lesson**: [Local vs Remote Repositories](02-local-vs-remote.md)

---

## 🎮 Practice Exercises

### Exercise 1: Multiple Feature Workflow
1. Add a search function to the todo app
2. Stage and commit it
3. Add styling to make it look better
4. Stage and commit styling separately
5. Push both commits to GitHub

### Exercise 2: Collaboration Simulation
1. Make a change directly on GitHub (edit a file through the web interface)
2. Make a different change locally
3. Try to push (it should fail)
4. Pull the remote changes
5. Push your changes

### Exercise 3: Commit Message Practice
Write good commit messages for these scenarios:
- Adding a login form
- Fixing a bug where users couldn't delete their account
- Updating the README with installation instructions
- Refactoring the database connection code

---

## 📚 Additional Resources

- [Conventional Commits](https://www.conventionalcommits.org/) - Standard for commit messages
- [Git Workflow Tutorial](https://www.atlassian.com/git/tutorials/comparing-workflows)
- [Interactive Git Tutorial](https://learngitbranching.js.org/)
- [GitHub Flow Guide](https://guides.github.com/introduction/flow/)