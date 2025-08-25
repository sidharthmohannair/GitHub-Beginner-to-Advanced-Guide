# Pull Requests & Code Reviews

## 🎯 Learning Goals
By the end of this lesson, you'll master:
- What Pull Requests are and why they're essential
- Creating effective Pull Requests on GitHub
- Writing good PR descriptions and titles
- Conducting thorough code reviews
- Handling feedback and making revisions
- Best practices for collaborative development

---

## 🔍 What are Pull Requests?

A **Pull Request (PR)** is a way to propose changes to a repository. Think of it as saying:

*"Hey team, I've made some changes on my branch. Please review them and consider merging them into the main branch."*

### Why Use Pull Requests?
1. **Code Quality**: Others review your code before it's merged
2. **Knowledge Sharing**: Team learns about changes before they go live
3. **Discussion**: Platform for discussing implementation details
4. **Documentation**: History of why changes were made
5. **Testing**: Opportunity to run automated tests before merge
6. **Collaboration**: Multiple people can contribute to a feature

### Pull Request Workflow
```
1. Create feature branch
2. Make changes and commits
3. Push branch to GitHub
4. Open Pull Request
5. Team reviews and discusses
6. Make revisions if needed
7. Get approval
8. Merge into main branch
9. Delete feature branch
```

---

## 🚀 Creating Your First Pull Request

Let's create a real Pull Request using a practical example:

### Step 1: Set Up Repository

```bash
# Create a new repository for a task management app
mkdir task-manager
cd task-manager
git init

# Create initial files
cat > index.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Task Manager</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>My Task Manager</h1>
        <div class="task-input">
            <input type="text" id="taskInput" placeholder="Enter a new task...">
            <button id="addTaskBtn">Add Task</button>
        </div>
        <ul id="taskList" class="task-list"></ul>
    </div>
    <script src="script.js"></script>
</body>
</html>
EOF

cat > styles.css << 'EOF'
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f5f5f5;
    color: #333;
}

.container {
    max-width: 600px;
    margin: 50px auto;
    background: white;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    padding: 30px;
}

h1 {
    text-align: center;
    margin-bottom: 30px;
    color: #2c3e50;
}

.task-input {
    display: flex;
    gap: 10px;
    margin-bottom: 30px;
}

#taskInput {
    flex: 1;
    padding: 12px;
    border: 2px solid #ddd;
    border-radius: 5px;
    font-size: 16px;
}

#addTaskBtn {
    padding: 12px 20px;
    background: #3498db;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
}

#addTaskBtn:hover {
    background: #2980b9;
}

.task-list {
    list-style: none;
}

.task-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px;
    border-bottom: 1px solid #eee;
    transition: background-color 0.2s;
}

.task-item:hover {
    background-color: #f8f9fa;
}

.delete-btn {
    background: #e74c3c;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 3px;
    cursor: pointer;
    font-size: 12px;
}

.delete-btn:hover {
    background: #c0392b;
}
EOF

cat > script.js << 'EOF'
document.addEventListener('DOMContentLoaded', function() {
    const taskInput = document.getElementById('taskInput');
    const addTaskBtn = document.getElementById('addTaskBtn');
    const taskList = document.getElementById('taskList');

    function addTask() {
        const taskText = taskInput.value.trim();
        
        if (taskText === '') {
            alert('Please enter a task!');
            return;
        }

        const listItem = document.createElement('li');
        listItem.className = 'task-item';
        
        listItem.innerHTML = `
            <span>${taskText}</span>
            <button class="delete-btn" onclick="deleteTask(this)">Delete</button>
        `;

        taskList.appendChild(listItem);
        taskInput.value = '';
        taskInput.focus();
    }

    window.deleteTask = function(button) {
        const listItem = button.parentElement;
        taskList.removeChild(listItem);
    };

    addTaskBtn.addEventListener('click', addTask);
    
    taskInput.addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            addTask();
        }
    });
});
EOF

# Create README
cat > README.md << 'EOF'
# Task Manager

A simple, clean task management application built with vanilla HTML, CSS, and JavaScript.

## Features

- Add new tasks
- Delete completed tasks
- Responsive design
- Clean, modern interface

## How to Use

1. Open `index.html` in your web browser
2. Type a task in the input field
3. Click "Add Task" or press Enter
4. Click "Delete" to remove completed tasks

## Development

This project uses vanilla JavaScript with no external dependencies.
EOF

# Initial commit and push
git add .
git commit -m "Add initial task manager application

- Create basic HTML structure with input and task list
- Add responsive CSS styling with modern design
- Implement JavaScript for adding and deleting tasks
- Add README with usage instructions"

# Create GitHub repository and push
git remote add origin https://github.com/yourusername/task-manager.git
git push -u origin main
```

### Step 2: Create Feature Branch

```bash
# Create branch for new feature
git checkout -b feature/task-completion

# Add task completion functionality
cat > script.js << 'EOF'
document.addEventListener('DOMContentLoaded', function() {
    const taskInput = document.getElementById('taskInput');
    const addTaskBtn = document.getElementById('addTaskBtn');
    const taskList = document.getElementById('taskList');

    function addTask() {
        const taskText = taskInput.value.trim();
        
        if (taskText === '') {
            alert('Please enter a task!');
            return;
        }

        const listItem = document.createElement('li');
        listItem.className = 'task-item';
        
        listItem.innerHTML = `
            <div class="task-content">
                <input type="checkbox" class="task-checkbox" onchange="toggleTask(this)">
                <span class="task-text">${taskText}</span>
            </div>
            <button class="delete-btn" onclick="deleteTask(this)">Delete</button>
        `;

        taskList.appendChild(listItem);
        taskInput.value = '';
        taskInput.focus();
    }

    window.toggleTask = function(checkbox) {
        const taskText = checkbox.nextElementSibling;
        const taskItem = checkbox.closest('.task-item');
        
        if (checkbox.checked) {
            taskText.style.textDecoration = 'line-through';
            taskText.style.opacity = '0.6';
            taskItem.classList.add('completed');
        } else {
            taskText.style.textDecoration = 'none';
            taskText.style.opacity = '1';
            taskItem.classList.remove('completed');
        }
    };

    window.deleteTask = function(button) {
        const listItem = button.parentElement;
        taskList.removeChild(listItem);
    };

    addTaskBtn.addEventListener('click', addTask);
    
    taskInput.addEventListener('keypress', function(e) {
        if (e.key === 'Enter') {
            addTask();
        }
    });
});
EOF

# Update CSS to support new features
cat >> styles.css << 'EOF'

.task-content {
    display: flex;
    align-items: center;
    gap: 10px;
}

.task-checkbox {
    width: 18px;
    height: 18px;
    cursor: pointer;
}

.task-text {
    flex: 1;
    transition: all 0.2s ease;
}

.task-item.completed {
    background-color: #f8f9fa;
}

.task-item.completed .task-text {
    color: #6c757d;
}
EOF

# Commit the changes
git add .
git commit -m "Add task completion functionality

- Add checkboxes to mark tasks as complete
- Implement toggle function with visual feedback
- Add strikethrough and opacity changes for completed tasks
- Update CSS to support task completion states
- Maintain existing delete functionality"

# Push the branch to GitHub
git push -u origin feature/task-completion
```

### Step 3: Open Pull Request on GitHub

1. **Go to your repository on GitHub**
2. **GitHub will show a banner**: "Compare & pull request" button
3. **Click the button** or go to "Pull requests" tab → "New pull request"
4. **Select branches**: 
   - Base: `main` (where you want to merge)
   - Compare: `feature/task-completion` (your feature branch)

---

## ✍️ Writing Effective Pull Request Descriptions

### PR Title Best Practices

```bash
# ✅ Good PR titles
"Add task completion functionality with checkboxes"
"Fix responsive layout issues on mobile devices"  
"Implement user authentication with JWT tokens"
"Update README with deployment instructions"

# ❌ Bad PR titles
"Update"
"Fix stuff"
"Changes"
"Feature"
```

### PR Description Template

```markdown
## 🎯 What This PR Does

Add task completion functionality to the task manager application.

## 📋 Changes Made

- Add checkboxes to each task item
- Implement `toggleTask()` function for completion state
- Add visual feedback (strikethrough, opacity) for completed tasks
- Update CSS with new styles for completed tasks
- Maintain existing add/delete functionality

## 🧪 How to Test

1. Open the application in your browser
2. Add several tasks
3. Click checkboxes to mark tasks as complete/incomplete
4. Verify visual changes (strikethrough, opacity)
5. Ensure delete functionality still works

## 📱 Screenshots

[Add screenshots showing before/after or new functionality]

## 🔗 Related Issues

Closes #15 - Add task completion feature

## ✅ Checklist

- [x] Code follows project style guidelines
- [x] Changes have been tested locally  
- [x] No console errors
- [x] Responsive design maintained
- [ ] Documentation updated (if needed)

## 🤔 Questions for Reviewers

- Should completed tasks be moved to bottom of list?
- Do you like the current visual styling for completed tasks?
- Should we add a "Clear Completed" button in future?
```

---

## 👁️ Conducting Code Reviews

### What to Look For in Code Reviews

#### 1. **Functionality**
```javascript
// ✅ Check: Does the code work as expected?
function toggleTask(checkbox) {
    const taskText = checkbox.nextElementSibling;
    // Good: Clear variable names and logic
    
    if (checkbox.checked) {
        taskText.style.textDecoration = 'line-through';
        // Good: Visual feedback for user
    }
}

// ❌ Red flag: Complex logic without comments
function x(a) {
    return a.map(b => b.filter(c => c.d > 5).reduce((e,f) => e + f.g, 0));
}
```

#### 2. **Code Quality**
```css
/* ✅ Good: Consistent naming and organization */
.task-item {
    display: flex;
    justify-content: space-between;
    padding: 15px;
    transition: background-color 0.2s;
}

.task-item.completed {
    background-color: #f8f9fa;
}

/* ❌ Bad: Inconsistent spacing and naming */
.taskitem{
padding:10px 5px 20px 15px;
background:#fff;
}
```

#### 3. **Security & Performance**
```javascript
// ✅ Good: Input validation
function addTask() {
    const taskText = taskInput.value.trim();
    
    if (taskText === '') {
        alert('Please enter a task!');
        return;
    }
    // Prevents empty tasks
}

// ❌ Security risk: No input sanitization
listItem.innerHTML = userInput; // Could allow XSS attacks
```

### How to Leave Helpful Review Comments

#### ✅ Good Review Comments

```markdown
**Suggestion**: Consider extracting this into a separate function for reusability:
```javascript
function createTaskElement(taskText) {
    const listItem = document.createElement('li');
    listItem.className = 'task-item';
    // ... rest of logic
    return listItem;
}
```

**Question**: Should we add a confirmation dialog before deleting tasks?

**Praise**: Great job adding the transition effects! The UX feels much smoother.

**Bug**: This might not work if `taskText` is null. Consider adding a null check.
```

#### ❌ Unhelpful Review Comments

```markdown
This is wrong.
Fix this.
I don't like this.
Change everything.
```

### Review Comment Types

```markdown
**🐛 Bug**: Points out actual problems
**💡 Suggestion**: Ideas for improvement  
**❓ Question**: Asking for clarification
**🎨 Style**: Formatting and consistency
**⚡ Performance**: Efficiency improvements
**🔒 Security**: Potential vulnerabilities
**👏 Praise**: Positive feedback
```

---

## 🔄 Handling Review Feedback

### Receiving Feedback Gracefully

```bash
# When reviewers request changes:

# 1. Read all feedback carefully
# 2. Ask questions if anything is unclear
# 3. Make requested changes
# 4. Push updates to the same branch

git checkout feature/task-completion

# Make the requested changes
echo "// Added error handling as requested" >> script.js

git add .
git commit -m "Address review feedback: add error handling"
git push origin feature/task-completion

# 5. Respond to comments explaining your changes
# 6. Request re-review when ready
```

### Responding to Review Comments

#### Example Conversation:

**Reviewer**: *"Should we add a confirmation dialog before deleting tasks?"*

**Your Response**: *"Good point! I initially thought it might be annoying for quick task deletion, but you're right that accidental deletions could be frustrating. I'll add a simple confirm() dialog in the next commit."*

**After implementing**:
*"✅ Added confirmation dialog in commit abc123. It only appears when deleting tasks that are not completed (assuming completed tasks are intentionally being removed)."*

---

## 🚀 Advanced Pull Request Features

### Draft Pull Requests

```bash
# When your work isn't ready for review yet
# Create as draft PR on GitHub:
# "Create draft pull request" instead of "Create pull request"

# Benefits:
# - Shows work in progress
# - Prevents accidental merges
# - Allows early feedback
# - Can run CI/CD tests

# Convert to ready when done
```

### PR Templates

Create `.github/pull_request_template.md`:

```markdown
## 🎯 What This PR Does
Brief description of changes

## 🧪 Testing
- [ ] Tested locally
- [ ] Added unit tests
- [ ] Tested in different browsers

## 🔗 Related Issues
Closes #

## 📱 Screenshots
[If applicable]

## 🤔 Questions
Any specific concerns or questions for reviewers?
```

### Linking PRs to Issues

```markdown
# In PR description, use keywords to auto-close issues:
Closes #123
Fixes #456
Resolves #789

# GitHub will automatically close these issues when PR is merged
```

---

## ⚙️ PR Merge Strategies

### 1. Merge Commit
```bash
# Creates merge commit preserving branch history
# Best for: Feature branches with multiple collaborators

git merge feature/task-completion
# Result: Preserves all individual commits + merge commit
```

### 2. Squash and Merge
```bash
# Combines all commits into single commit
# Best for: Clean history, feature branches with many small commits

# GitHub does this automatically when you choose "Squash and merge"
# Result: Single commit with combined changes
```

### 3. Rebase and Merge
```bash
# Replays commits without merge commit
# Best for: Linear history, well-organized commits

git rebase main
git checkout main
git merge feature/task-completion  # Fast-forward merge
# Result: Linear history, no merge commit
```

---

## 🎯 Pull Request Best Practices

### 1. Size and Scope
```bash
# ✅ Good: Focused, single-purpose PRs
- One feature per PR
- Under 400 lines of changes
- Clear, specific scope

# ❌ Bad: Large, multi-purpose PRs  
- Multiple unrelated changes
- Thousands of lines
- Mixed bug fixes and features
```

### 2. Commit Quality
```bash
# ✅ Good commits in PR
git commit -m "Add task completion checkboxes"
git commit -m "Implement toggle functionality"  
git commit -m "Add CSS styles for completed tasks"

# ❌ Bad commits in PR
git commit -m "wip"
git commit -m "fix"
git commit -m "more changes"
```

### 3. Communication
```markdown
# ✅ Good PR communication
- Clear title and description
- Screenshots/GIFs for UI changes
- Links to relevant issues
- Specific questions for reviewers
- Responsive to feedback

# ❌ Poor PR communication
- Vague titles
- No description
- Ignores reviewer questions
- Defensive about feedback
```

---

## 🎓 Knowledge Check

**Question 1**: What's the difference between a draft PR and a regular PR?
<details>
<summary>Click to see answer</summary>
- **Draft PR**: Work in progress, can't be merged, requests feedback on incomplete work
- **Regular PR**: Ready for review and potential merge
- Draft PRs prevent accidental merges and signal the PR isn't ready
</details>

**Question 2**: When should you use "Squash and merge" vs "Create merge commit"?
<details>
<summary>Click to see answer</summary>
- **Squash and merge**: When you want clean history and the individual commits aren't important
- **Create merge commit**: When you want to preserve the branch history and individual commit details
- **Rebase and merge**: When you want linear history without a merge commit
</details>

**Question 3**: What makes a good code review comment?
<details>
<summary>Click to see answer</summary>
Good comments are:
- Specific and actionable
- Explain the "why" not just "what"
- Offer suggestions or alternatives
- Include praise for good code
- Ask questions for clarification
- Focus on code quality, not personal preferences
</details>

---

## 🎯 Key Takeaways

1. **Pull Requests enable collaborative development** and maintain code quality
2. **Good PR descriptions** save reviewers time and improve communication
3. **Code reviews are about learning** and improving code quality together
4. **Handle feedback professionally** and see it as growth opportunity
5. **Keep PRs focused and reasonably sized** for effective reviews
6. **Use appropriate merge strategies** based on your team's workflow

---

## ➡️ What's Next?

Now that you can create Pull Requests and conduct code reviews, let's learn how to contribute to other people's projects through forking.

**Next lesson**: [Forking & Contributing to Projects](03-forking-contributing.md)

---

## 🎮 Practice Exercises

### Exercise 1: Create a Complete PR Workflow
1. Create a new feature in your task manager
2. Write a comprehensive PR description
3. Include screenshots and testing instructions
4. Practice reviewing your own code critically

### Exercise 2: Code Review Practice
1. Find a friend's repository or contribute to open source
2. Practice leaving constructive review comments
3. Focus on different aspects: functionality, style, performance
4. Experience both giving and receiving feedback

### Exercise 3: PR Template Creation
1. Create a custom PR template for your projects
2. Include sections relevant to your work
3. Test it by creating several PRs
4. Refine based on what information you actually need

---

## 📚 Additional Resources

- [About Pull Requests - GitHub Docs](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [Best Practices for Code Reviews](https://google.github.io/eng-practices/review/)
- [How to Write the Perfect Pull Request](https://github.blog/2015-01-21-how-to-write-the-perfect-pull-request/)
- [The Art of Readable Code](https://www.oreilly.com/library/view/the-art-of/9781449318482/)