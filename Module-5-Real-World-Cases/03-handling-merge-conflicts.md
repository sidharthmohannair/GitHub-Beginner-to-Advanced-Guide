# Handling Merge Conflicts Like a Pro

## 🎯 Real-World Scenario
It's Monday morning. You've been working on a feature for 3 days, ready to merge, but Git says: **"CONFLICT: Merge conflict in app.js"**. Here's how to handle it like a senior developer.

---

## 🔥 **Understanding Conflicts (The Why)**

### **What Causes Conflicts:**
```bash
# You changed:          Teammate changed:
function login() {      function login() {
    return "v1";            return "v2";
}                       }

# Git says: "I can't decide which version to keep!"
```

### **Common Conflict Scenarios:**
1. **Same line, different changes** (most common)
2. **One person deletes, another modifies**
3. **File renamed vs. file modified**
4. **Dependencies/imports changed**

---

## ⚡ **Quick Conflict Resolution (5-Minute Fix)**

### **Step 1: Don't Panic, Assess**
```bash
# When you see conflict error:
git status
# Shows:
# both modified:   src/app.js
# both modified:   package.json

# Check what conflicts exist:
git diff --name-only --diff-filter=U
# Lists only conflicted files
```

### **Step 2: Open Conflicted File**
```javascript
// src/app.js - What you'll see:
function authenticateUser(email, password) {
<<<<<<< HEAD
    // Your changes
    const hashedPassword = bcrypt.hash(password, 12);
    return validateLogin(email, hashedPassword);
=======
    // Their changes  
    const saltedPassword = password + process.env.SALT;
    return checkCredentials(email, saltedPassword);
>>>>>>> feature/improved-auth
}
```

### **Step 3: Choose Resolution Strategy**
```javascript
// Strategy 1: Keep yours (if you're right)
function authenticateUser(email, password) {
    const hashedPassword = bcrypt.hash(password, 12);
    return validateLogin(email, hashedPassword);
}

// Strategy 2: Keep theirs (if they're right)  
function authenticateUser(email, password) {
    const saltedPassword = password + process.env.SALT;
    return checkCredentials(email, saltedPassword);
}

// Strategy 3: Combine both (best solution)
function authenticateUser(email, password) {
    const saltedPassword = password + process.env.SALT;
    const hashedPassword = bcrypt.hash(saltedPassword, 12);
    return validateLogin(email, hashedPassword);
}
```

### **Step 4: Mark as Resolved**
```bash
# Remove conflict markers, save file
git add src/app.js

# Check remaining conflicts
git status  # Should show no more conflicts

# Complete the merge
git commit -m "Resolve merge conflict in authentication

Combined both hashing approaches:
- Keep salting from feature/improved-auth  
- Keep bcrypt hashing from current branch
- Tested both login flows work correctly"
```

---

## 🛠️ **Advanced Conflict Resolution Tools**

### **Using VS Code (Most Popular)**
```bash
# Open conflicted file in VS Code
code src/app.js

# VS Code shows:
# [Accept Current Change] [Accept Incoming Change] [Accept Both Changes]

# Use buttons or:
# - "Accept Current Change" = your version
# - "Accept Incoming Change" = their version  
# - "Accept Both Changes" = combine automatically
# - Or manually edit the result
```

### **Using Git Mergetool**
```bash
# Configure your preferred merge tool
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Or use other tools:
git config --global merge.tool kdiff3    # KDiff3
git config --global merge.tool vimdiff   # Vim
git config --global merge.tool meld      # Meld (Linux)

# Launch merge tool
git mergetool
# Opens visual 3-way merge interface
```

### **Command Line Resolution**
```bash
# See detailed conflict info
git show :1:app.js > app.js.common   # Common ancestor
git show :2:app.js > app.js.ours     # Your changes
git show :3:app.js > app.js.theirs   # Their changes

# Compare and manually resolve
diff app.js.ours app.js.theirs
```

---

## 🎯 **Real-World Conflict Scenarios**

### **Scenario 1: Import Conflicts**
```javascript
// CONFLICT in imports (super common!)

<<<<<<< HEAD
import React from 'react';
import axios from 'axios';
import { validateEmail } from '../utils/validation';
import LoginForm from './LoginForm';
=======
import React from 'react';
import fetch from 'node-fetch';
import { checkEmail } from '../helpers/validation';  
import AuthForm from './AuthForm';
>>>>>>> feature/new-auth

// RESOLUTION: Combine what's needed
import React from 'react';
import axios from 'axios';
import fetch from 'node-fetch';  // Keep both HTTP libraries
import { validateEmail, checkEmail } from '../utils/validation';
import LoginForm from './LoginForm';
import AuthForm from './AuthForm';
```

### **Scenario 2: Package.json Conflicts**
```json
// CONFLICT in dependencies
{
  "dependencies": {
<<<<<<< HEAD
    "react": "^18.0.0",
    "axios": "^1.3.0"
=======
    "react": "^18.2.0",  
    "lodash": "^4.17.21"
>>>>>>> feature/add-lodash
  }
}

// RESOLUTION: Keep latest versions + new dependencies
{
  "dependencies": {
    "react": "^18.2.0",    // Use latest version
    "axios": "^1.3.0",     // Keep existing
    "lodash": "^4.17.21"   // Add new dependency
  }
}
```

### **Scenario 3: Logic Conflicts**
```javascript
// CONFLICT in business logic
function calculatePrice(item, user) {
<<<<<<< HEAD
    let price = item.basePrice;
    if (user.isPremium) {
        price *= 0.9;  // 10% premium discount
    }
    return price;
=======
    let price = item.basePrice;
    if (user.loyaltyLevel > 5) {
        price *= 0.85;  // 15% loyalty discount  
    }
    return price + item.tax;
>>>>>>> feature/loyalty-program

// RESOLUTION: Support both discount types
function calculatePrice(item, user) {
    let price = item.basePrice;
    
    // Apply premium discount
    if (user.isPremium) {
        price *= 0.9;  // 10% premium discount
    }
    
    // Apply loyalty discount (stacks with premium)
    if (user.loyaltyLevel > 5) {
        price *= 0.95;  // 5% additional loyalty discount
    }
    
    return price + item.tax;
}
```

---

## 🚨 **Conflict Prevention Strategies**

### **1. Frequent Syncing (Best Practice)**
```bash
# Do this daily (or even twice daily)
git checkout main
git pull origin main
git checkout your-feature-branch
git rebase main  # or: git merge main

# Result: Smaller, easier conflicts
```

### **2. Coordinate with Team**
```bash
# Before major changes:
# 1. Discuss in team chat
# 2. Check who else is working on similar areas
# 3. Consider pair programming for overlapping work
```

### **3. Modular Development**
```javascript
// Instead of everyone editing same large file:
// BEFORE: Everyone modifies app.js (conflict-prone)
// app.js (1000+ lines)

// AFTER: Split into focused modules (conflict-free)
// auth/login.js
// auth/signup.js  
// auth/validation.js
// utils/api.js
```

---

## ⚡ **Pro Conflict Resolution Tips**

### **1. Understand the Context**
```bash
# Before resolving, understand WHY changes were made
git log --oneline feature/other-branch

# Check commit messages for context:
# "Fix security vulnerability in login"  → Important!
# "Update button color to blue"          → Less critical
```

### **2. Test After Resolution**
```bash
# ALWAYS test after resolving conflicts
npm test           # Run unit tests
npm run e2e        # Run integration tests  
npm start          # Test manually

# Don't just resolve and commit blindly!
```

### **3. Ask for Help When Needed**
```bash
# If conflict involves complex business logic:
# 1. Slack the original author: "Hey @john, can you help with this conflict?"
# 2. Schedule quick call to resolve together
# 3. Use pair programming approach
```

---

## 🔄 **Rebase vs. Merge Conflicts**

### **Merge Conflicts:**
```bash
# Merging feature into main
git checkout main
git merge feature/my-branch
# CONFLICT: Fix once, create merge commit

# Timeline looks like:
main:    A---B---C---M
              \     /
feature:       D---E
```

### **Rebase Conflicts:**
```bash
# Rebasing feature onto main  
git checkout feature/my-branch
git rebase main
# CONFLICT: May need to fix multiple times (once per commit)

# Timeline looks like:
main:    A---B---C
                  \
feature:           D'---E'

# Cleaner history but potentially more conflict resolution
```

### **Which to Choose:**
```bash
# Use MERGE when:
- Working on shared/public branches
- Want to preserve branch history
- Complex conflicts (easier to resolve once)

# Use REBASE when:  
- Personal feature branches
- Want clean linear history
- Simple conflicts
```

---

## 🎯 **Complex Conflict Scenarios**

### **Scenario 1: File Rename Conflicts**
```bash
# You renamed: user.js → person.js
# They modified: user.js content

# Git shows:
# deleted by us:   user.js
# added by them:   user.js

# Resolution:
git add person.js           # Keep your rename
git rm user.js              # Remove old file
# Manually copy their changes into person.js
```

### **Scenario 2: Binary File Conflicts**
```bash
# Two people changed same image/binary file
# CONFLICT in: logo.png

# You MUST choose one version:
git checkout --ours logo.png    # Keep your version
# OR
git checkout --theirs logo.png  # Keep their version

git add logo.png
```

### **Scenario 3: Merge Conflict During Rebase**
```bash
# During rebase, conflicts on multiple commits
git rebase main
# CONFLICT on commit 1

# Fix conflict, then:
git add .
git rebase --continue

# CONFLICT on commit 2
# Fix conflict, then:
git add .  
git rebase --continue

# Repeat until rebase completes
```

---

## 🛠️ **Emergency Conflict Solutions**

### **Nuclear Option: Start Over**
```bash
# If conflicts are too complex and you're stuck:

# 1. Save your work
git stash  # or copy files manually

# 2. Reset to clean state
git checkout main
git pull origin main
git branch -D your-feature-branch
git checkout -b your-feature-branch

# 3. Re-apply your changes manually
git stash pop  # or copy files back
# Clean up and commit properly
```

### **Get Help from Original Author**
```bash
# If conflict involves someone else's complex changes:
git log --oneline --author="john@company.com" main..HEAD

# Message them:
"Hi @john! I'm getting merge conflicts on the authentication 
changes you made. Can we pair on resolving this? The conflicts 
are in auth.js lines 45-67."
```

---

## 📊 **Conflict Resolution Checklist**

### **Before Resolving:**
- [ ] Understand what both changes are trying to achieve
- [ ] Check if changes can coexist or need integration
- [ ] Identify the business impact of each change
- [ ] Consider asking original authors for context

### **While Resolving:**
- [ ] Remove all conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
- [ ] Ensure resulting code makes logical sense
- [ ] Maintain consistent code style
- [ ] Don't accidentally delete important changes

### **After Resolving:**
- [ ] Test the functionality manually
- [ ] Run automated tests
- [ ] Review the final result
- [ ] Write clear commit message explaining resolution

---

## 💡 **Key Takeaways**

1. **Conflicts are normal** - every developer deals with them
2. **Prevention is better** - sync frequently with main branch
3. **Understand before resolving** - know what both sides are doing
4. **Use tools** - VS Code, merge tools make it easier
5. **Test after resolving** - don't break functionality
6. **Ask for help** - better to ask than guess wrong
7. **Learn from conflicts** - they show team coordination opportunities

---

**Next**: [GitHub Pages Deployment](04-github-pages-deploy.md) - Deploy your projects for free!