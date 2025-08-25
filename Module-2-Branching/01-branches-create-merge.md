# Branches: Create, Merge & Rebase

## 🎯 Learning Goals
By the end of this lesson, you'll master:
- What branches are and why they're essential
- Creating and switching between branches
- Merging branches with different strategies
- Understanding when to use merge vs rebase
- Resolving merge conflicts
- Best practices for branch management

---

## 🌿 What Are Git Branches?

Think of branches as **parallel timelines** for your project. Each branch represents an independent line of development.

### Real-World Analogy: Building a House
```
Main Branch (main):     🏠 Stable house foundation
Feature Branch:         🚪 Adding a new room
Bugfix Branch:          🔧 Fixing a broken window
Hotfix Branch:          🚨 Emergency roof repair
```

### Why Use Branches?
1. **Isolation**: Work on features without breaking main code
2. **Collaboration**: Multiple people can work simultaneously
3. **Experimentation**: Try ideas without risk
4. **Organization**: Keep different types of work separate
5. **Code Review**: Changes can be reviewed before merging

---

## 🎬 Setting Up Practice Repository

Let's build a simple blog website to practice branching:

```bash
mkdir blog-website
cd blog-website
git init

# Create initial structure
cat > index.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Blog</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Welcome to My Blog</h1>
        <nav>
            <a href="#home">Home</a>
            <a href="#about">About</a>
        </nav>
    </header>
    <main>
        <article>
            <h2>My First Blog Post</h2>
            <p>This is the beginning of my blogging journey!</p>
        </article>
    </main>
</body>
</html>
EOF

cat > style.css << 'EOF'
body {
    font-family: Arial, sans-serif;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    line-height: 1.6;
}

header {
    border-bottom: 1px solid #ddd;
    padding-bottom: 20px;
    margin-bottom: 20px;
}

nav a {
    margin-right: 20px;
    text-decoration: none;
    color: #333;
}

article {
    margin-bottom: 30px;
}
EOF

# Initial commit
git add .
git commit -m "Add initial blog structure with HTML and CSS"

# Check current branch
git branch
# Shows: * main
```

---

## 🌱 Creating and Managing Branches

### Creating Branches

```bash
# Method 1: Create branch and switch to it
git checkout -b feature/dark-mode

# Method 2: Create branch without switching
git branch feature/contact-form

# Method 3: Modern syntax (Git 2.23+)
git switch -c feature/search-functionality

# List all branches
git branch
# Shows:
# * feature/dark-mode
#   feature/contact-form
#   feature/search-functionality
#   main
```

### Branch Naming Conventions

```bash
# Feature branches
git checkout -b feature/user-login
git checkout -b feature/shopping-cart
git checkout -b feat/payment-integration

# Bug fix branches  
git checkout -b bugfix/header-alignment
git checkout -b fix/login-validation
git checkout -b hotfix/security-patch

# Release branches
git checkout -b release/v1.2.0
git checkout -b release/2024-01-15

# Personal branches
git checkout -b john/experiment-new-ui
git checkout -b sarah/refactor-database
```

### Switching Between Branches

```bash
# Switch to existing branch (older syntax)
git checkout main

# Switch to existing branch (modern syntax)
git switch main

# Create and switch in one command
git switch -c feature/new-feature

# Switch to previous branch
git switch -
```

---

## 🎨 Working on Feature Branches

Let's create a dark mode feature:

```bash
# Create and switch to dark mode branch
git switch -c feature/dark-mode

# Add dark mode CSS
cat >> style.css << 'EOF'

/* Dark mode styles */
@media (prefers-color-scheme: dark) {
    body {
        background-color: #1a1a1a;
        color: #e0e0e0;
    }
    
    header {
        border-bottom-color: #444;
    }
    
    nav a {
        color: #ccc;
    }
    
    nav a:hover {
        color: #fff;
    }
}

/* Dark mode toggle */
.dark-mode-toggle {
    position: fixed;
    top: 20px;
    right: 20px;
    background: #333;
    color: white;
    border: none;
    padding: 10px 15px;
    border-radius: 5px;
    cursor: pointer;
}
EOF

# Update HTML to include toggle button
sed -i 's|</header>|    <button class="dark-mode-toggle">🌙 Dark Mode</button>\n</header>|' index.html

# Commit the feature
git add .
git commit -m "Add dark mode styles and toggle button"

# Check branch status
git log --oneline
git branch
```

---

## 🔀 Merging Branches

### Types of Merges

#### 1. Fast-Forward Merge
When the target branch hasn't changed since branching:

```bash
# Switch to main branch
git switch main

# Merge dark mode feature (fast-forward)
git merge feature/dark-mode

# Check the result
git log --oneline --graph
# Shows linear history: no merge commit created
```

#### 2. Three-Way Merge
When both branches have new commits:

```bash
# Create another feature while on main
echo "<footer><p>&copy; 2024 My Blog</p></footer>" >> index.html
git add index.html
git commit -m "Add footer to main page"

# Create and work on contact form branch
git switch -c feature/contact-form

cat > contact.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Contact - My Blog</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Contact Me</h1>
        <nav>
            <a href="index.html">Home</a>
            <a href="#about">About</a>
        </nav>
    </header>
    <main>
        <form>
            <label for="name">Name:</label>
            <input type="text" id="name" required>
            
            <label for="email">Email:</label>
            <input type="email" id="email" required>
            
            <label for="message">Message:</label>
            <textarea id="message" required></textarea>
            
            <button type="submit">Send Message</button>
        </form>
    </main>
</body>
</html>
EOF

git add contact.html
git commit -m "Add contact form page"

# Switch back to main and merge
git switch main
git merge feature/contact-form

# This creates a merge commit because both branches had changes
git log --oneline --graph
```

---

## 🔄 Understanding Rebase

Rebase is an alternative to merge that creates a cleaner, linear history.

### Merge vs Rebase Comparison

```
# Before merge/rebase:
    A---B---C main
     \
      D---E feature

# After merge:
    A---B---C---F main
     \         /
      D---E---

# After rebase:
    A---B---C---D'---E' main
```

### When to Use Rebase

```bash
# Create a feature branch
git switch -c feature/search-box

# Add search functionality
cat > search.js << 'EOF'
function searchBlog(query) {
    const articles = document.querySelectorAll('article');
    const searchTerm = query.toLowerCase();
    
    articles.forEach(article => {
        const title = article.querySelector('h2').textContent.toLowerCase();
        const content = article.querySelector('p').textContent.toLowerCase();
        
        if (title.includes(searchTerm) || content.includes(searchTerm)) {
            article.style.display = 'block';
        } else {
            article.style.display = 'none';
        }
    });
}

document.addEventListener('DOMContentLoaded', function() {
    const searchInput = document.getElementById('search-input');
    if (searchInput) {
        searchInput.addEventListener('input', (e) => {
            searchBlog(e.target.value);
        });
    }
});
EOF

# Update HTML to include search box
sed -i 's|<nav>|<nav>\n        <input type="search" id="search-input" placeholder="Search blog...">\n        <div>|' index.html

git add .
git commit -m "Add search functionality with JavaScript"

# Meanwhile, someone updated main branch
git switch main
echo "<!-- Updated main branch -->" >> index.html
git add index.html
git commit -m "Add comment to main branch"

# Now rebase feature branch onto main
git switch feature/search-box
git rebase main

# Result: search feature commits are moved to tip of main
# Switch to main and merge (will be fast-forward)
git switch main
git merge feature/search-box
```

---

## ⚔️ Handling Merge Conflicts

Conflicts occur when Git can't automatically merge changes.

### Creating a Conflict Scenario

```bash
# Create two conflicting branches
git switch main

# Branch 1: Update header
git switch -c feature/new-header
sed -i 's|Welcome to My Blog|My Awesome Developer Blog|' index.html
git add index.html
git commit -m "Update blog title to be more specific"

# Branch 2: Different header update
git switch main
git switch -c feature/better-header  
sed -i 's|Welcome to My Blog|The Ultimate Coding Blog|' index.html
git add index.html
git commit -m "Update blog title to be more engaging"

# Try to merge - this will create a conflict
git switch main
git merge feature/new-header  # This works fine
git merge feature/better-header  # This creates conflict!
```

### Resolving Conflicts

```bash
# Git shows conflict status
git status
# Shows: both modified: index.html

# View the conflict
cat index.html
# You'll see conflict markers:
# <<<<<<< HEAD
# My Awesome Developer Blog
# =======
# The Ultimate Coding Blog
# >>>>>>> feature/better-header

# Edit the file to resolve conflict
sed -i 's|<<<<<<< HEAD||; s|=======||; s|>>>>>>> feature/better-header||; s|My Awesome Developer Blog|The Ultimate Developer Blog|; s|The Ultimate Coding Blog||' index.html

# Or use a merge tool
git mergetool

# After resolving, stage and commit
git add index.html
git commit -m "Resolve header title conflict"

# Check the result
git log --oneline --graph
```

### Conflict Resolution Tools

```bash
# Configure merge tool (VS Code example)
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Use merge tool when conflicts occur
git mergetool

# Other popular merge tools:
git config --global merge.tool kdiff3    # KDiff3
git config --global merge.tool meld      # Meld
git config --global merge.tool vimdiff   # Vim
```

---

## 🧹 Branch Cleanup and Management

### Deleting Branches

```bash
# Delete merged branch (safe)
git branch -d feature/dark-mode

# Force delete unmerged branch (dangerous)
git branch -D feature/experimental-feature

# Delete remote branch
git push origin --delete feature/old-feature

# List branches with merge status
git branch --merged     # Shows branches merged into current
git branch --no-merged  # Shows branches not yet merged
```

### Branch Management Commands

```bash
# Show all branches (local and remote)
git branch -a

# Show branch with last commit
git branch -v

# Show branches that contain specific commit
git branch --contains abc123

# Rename current branch
git branch -m new-branch-name

# Rename other branch
git branch -m old-name new-name
```

---

## 🎯 Best Practices for Branching

### 1. Branch Naming Strategy
```bash
# Use descriptive, lowercase names with hyphens
git checkout -b feature/user-authentication
git checkout -b bugfix/login-form-validation
git checkout -b hotfix/security-vulnerability

# Include ticket numbers when applicable
git checkout -b feature/JIRA-123-shopping-cart
git checkout -b bugfix/ISSUE-456-payment-error
```

### 2. Keep Branches Focused
```bash
# ✅ Good: One feature per branch
git checkout -b feature/add-user-login
git checkout -b feature/add-password-reset
git checkout -b feature/add-email-verification

# ❌ Bad: Multiple unrelated changes
git checkout -b feature/login-and-styling-and-bugfixes
```

### 3. Regular Maintenance
```bash
# Daily routine: Update main branch
git switch main
git pull origin main

# Before starting work: Create fresh branch from latest main
git switch -c feature/new-feature

# Regular cleanup: Delete merged branches
git branch --merged | grep -v main | xargs git branch -d
```

### 4. Merge Strategy Guidelines
```bash
# Use merge for:
# - Feature branches with significant changes
# - When you want to preserve branch history
# - Collaborative branches where multiple people contributed

git merge feature/major-redesign

# Use rebase for:
# - Personal feature branches
# - When you want clean, linear history  
# - Small, focused changes

git rebase main
```

---

## 📊 Visualizing Branch History

### Useful Log Commands
```bash
# Pretty graph view
git log --oneline --graph --decorate

# Show all branches
git log --oneline --graph --all

# Custom format
git log --graph --pretty=format:'%C(auto)%h%d %s %C(cyan)(%cr) %C(blue)<%an>'

# Create alias for complex commands
git config --global alias.tree "log --oneline --graph --decorate --all"
git tree  # Now use the alias
```

---

## 🎓 Knowledge Check

**Question 1**: What's the difference between `git merge` and `git rebase`?
<details>
<summary>Click to see answer</summary>
- **Merge**: Combines branches while preserving branch history, creates merge commit
- **Rebase**: Moves commits to a new base, creating linear history, no merge commit
- Use merge for collaborative branches, rebase for personal feature branches
</details>

**Question 2**: When should you use fast-forward vs three-way merge?
<details>
<summary>Click to see answer</summary>
- **Fast-forward**: Happens automatically when target branch hasn't changed
- **Three-way merge**: Happens when both branches have new commits
- You can force three-way merge with `git merge --no-ff` to preserve branch history
</details>

**Question 3**: How do you resolve a merge conflict?
<details>
<summary>Click to see answer</summary>
1. Identify conflicted files with `git status`
2. Edit files to remove conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
3. Choose which changes to keep or combine them
4. Stage resolved files with `git add`
5. Complete merge with `git commit`
</details>

**Question 4**: What's the safest way to delete a branch?
<details>
<summary>Click to see answer</summary>
Use `git branch -d branch-name` which only deletes if the branch is fully merged. Git will warn you if there are unmerged changes. Use `-D` only if you're sure you want to lose unmerged work.
</details>

---

## 🎯 Key Takeaways

1. **Branches enable parallel development** without interfering with main code
2. **Use descriptive naming conventions** for better organization
3. **Merge preserves history**, rebase creates clean linear history
4. **Conflicts are normal** - learn to resolve them confidently
5. **Clean up branches regularly** to avoid clutter
6. **One feature per branch** keeps changes focused and reviewable

---

## ➡️ What's Next?

Now that you can create and merge branches, let's learn how to collaborate with others using Pull Requests and code reviews.

**Next lesson**: [Pull Requests & Code Reviews](02-pull-requests-reviews.md)

---

## 🎮 Practice Exercises

### Exercise 1: Feature Development
1. Create a blog website project
2. Create separate branches for: navigation menu, sidebar, footer
3. Develop each feature independently
4. Merge them back to main in different ways (merge, rebase)
5. Practice resolving conflicts when they occur

### Exercise 2: Conflict Resolution
1. Create two branches that modify the same file
2. Make conflicting changes
3. Practice resolving conflicts manually
4. Try using a visual merge tool
5. Experiment with different resolution strategies

### Exercise 3: Branch Management
1. Create 10 different branches
2. Make various commits on each
3. Use different branch listing commands
4. Practice renaming branches
5. Clean up by deleting unnecessary branches

---

## 📚 Additional Resources

- [Pro Git: Branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
- [Atlassian Git Branching Tutorial](https://www.atlassian.com/git/tutorials/using-branches)
- [GitHub Flow](https://guides.github.com/introduction/flow/)
- [Git Branching Strategy](https://nvie.com/posts/a-successful-git-branching-model/)