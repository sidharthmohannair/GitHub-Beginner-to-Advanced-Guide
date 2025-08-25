# Dev → Main Workflow: Professional Team Development

## 🎯 Real-World Scenario
You're part of a 5-person development team building a web application. Here's how professional teams manage the development → main branch workflow daily.

---

## 🏢 **Professional Team Setup**

### **Branch Strategy:**
```bash
main          # Production-ready code (protected)
develop       # Integration branch for features  
feature/*     # Individual feature development
hotfix/*      # Emergency production fixes
release/*     # Release preparation
```

### **Team Roles:**
- **Developer**: Creates features, fixes bugs
- **Tech Lead**: Reviews code, makes architecture decisions  
- **DevOps**: Manages deployments, CI/CD
- **QA**: Tests features, validates releases

---

## 🚀 **Daily Development Workflow**

### **Morning Routine (5 mins)**
```bash
# 1. Sync with latest changes
git checkout develop
git pull origin develop

# 2. Check what's happening
git log --oneline -5
git branch -a

# 3. Clean up merged branches
git branch --merged | grep -v develop | xargs git branch -d
```

### **Starting New Feature (2 mins)**
```bash
# Create feature branch from develop
git checkout develop
git pull origin develop  # Always sync first!
git checkout -b feature/user-profile-editing

# Push branch immediately (backup + visibility)
git push -u origin feature/user-profile-editing
```

### **Daily Development (Throughout day)**
```bash
# Regular commits with good messages
git add .
git commit -m "Add profile form validation

- Email format validation with regex
- Password strength requirements  
- Real-time validation feedback
- Unit tests for validation functions"

# Push regularly (don't lose work!)
git push origin feature/user-profile-editing
```

---

## 🔄 **Feature Completion Workflow**

### **Step 1: Pre-PR Checklist (10 mins)**
```bash
# 1. Sync with develop (important!)
git checkout develop
git pull origin develop
git checkout feature/user-profile-editing
git rebase develop  # or: git merge develop

# 2. Run all tests
npm test
npm run lint
npm run build

# 3. Clean up commit history (optional)
git rebase -i HEAD~3  # Squash small commits

# 4. Final push
git push --force-with-lease origin feature/user-profile-editing
```

### **Step 2: Create PR (5 mins)**
```markdown
## 🎨 Add User Profile Editing

**What**: Users can now edit their profile information including avatar, name, email, and bio.

**Why**: Top requested feature from user feedback (Issue #245)

**How**: 
- Built React form with real-time validation
- Added profile image upload with resize
- Integrated with user API endpoints
- Added comprehensive error handling

**Testing**:
- ✅ Unit tests: 15 new tests added
- ✅ Integration tests: Profile flow tested
- ✅ Manual testing: All browsers tested
- ✅ Accessibility: Screen reader tested

**Screenshots**: 
[Before/after images of profile page]

**Closes**: #245
**Related**: #156, #203
```

### **Step 3: Code Review Process (1-2 days)**
```bash
# Team members review your code
# Address feedback professionally:

# Make requested changes
git checkout feature/user-profile-editing
# ... make changes ...
git add .
git commit -m "Address code review feedback

- Extract validation logic to separate hook
- Add TypeScript types for profile data
- Improve error message accessibility
- Add missing unit test for edge case"

git push origin feature/user-profile-editing
```

---

## 🚀 **Merge & Deploy Process**

### **Step 1: PR Approval & Merge**
```bash
# After 2+ approvals, tech lead merges PR
# Merge options:
# - "Squash and merge" (clean history)
# - "Merge commit" (preserve branch history)
# - "Rebase and merge" (linear history)

# Auto-delete feature branch after merge
```

### **Step 2: Develop → Main (Release Process)**
```bash
# When ready for release (usually weekly/bi-weekly):

# 1. Create release branch
git checkout develop
git pull origin develop
git checkout -b release/v1.4.0

# 2. Final preparations
echo "1.4.0" > VERSION
git add VERSION
git commit -m "Bump version to 1.4.0"

# 3. Merge to main
git checkout main
git pull origin main
git merge --no-ff release/v1.4.0
git tag -a v1.4.0 -m "Release version 1.4.0"

# 4. Merge back to develop
git checkout develop
git merge --no-ff release/v1.4.0

# 5. Push everything
git push origin main
git push origin develop  
git push origin --tags

# 6. Clean up
git branch -d release/v1.4.0
```

---

## 🚨 **Hotfix Workflow (Emergency!)**

### **Critical Bug in Production**
```bash
# 1. Create hotfix from main (not develop!)
git checkout main
git pull origin main
git checkout -b hotfix/security-patch-login

# 2. Fix the critical issue
# ... make minimal, focused fix ...
git add .
git commit -m "URGENT: Fix SQL injection vulnerability in login

- Sanitize user input in authentication endpoint
- Add parameterized queries for all user data
- Tested against common injection attacks

Fixes security issue reported privately"

# 3. Test thoroughly
npm test
npm run security-audit

# 4. Merge to main immediately
git checkout main  
git merge --no-ff hotfix/security-patch-login
git tag -a v1.4.1 -m "Hotfix: Security patch"

# 5. Merge to develop too
git checkout develop
git merge --no-ff hotfix/security-patch-login

# 6. Deploy ASAP
git push origin main
git push origin develop
git push origin --tags
```

---

## 🎯 **Real-World Team Scenarios**

### **Scenario 1: Conflicting Features**
```bash
# Two developers working on related features
# Developer A: Working on user authentication
# Developer B: Working on user profile

# Solution: Regular communication + frequent syncing
git checkout develop
git pull origin develop
git checkout feature/my-feature
git rebase develop  # Do this 2-3 times per week
```

### **Scenario 2: Failed CI/CD**
```bash
# Your PR fails automated tests
# Don't merge until green ✅

# 1. Check CI logs
# 2. Fix issues locally
git checkout feature/my-feature
# ... fix test failures ...
git add .
git commit -m "Fix failing unit tests in CI"
git push origin feature/my-feature

# 3. Wait for green build, then merge
```

### **Scenario 3: Emergency Rollback**
```bash
# Production deployment broke something
# Quick rollback process:

# 1. Revert the problematic merge commit
git checkout main
git revert -m 1 abc123d  # Revert merge commit
git push origin main

# 2. Deploy previous version
# 3. Investigate and fix properly in develop
```

---

## ⚡ **Daily Team Practices**

### **Morning Standup Integration**
```bash
# What you did yesterday:
"Completed user authentication feature, PR is ready for review"

# What you'll do today:  
"Working on profile editing, will rebase with develop changes"

# Any blockers:
"Waiting for API changes from backend team"
```

### **End-of-Day Routine**
```bash
# Always push your work
git add .
git commit -m "WIP: Profile form styling - 70% complete

Still need to:
- Add responsive breakpoints
- Fix Safari layout issue  
- Add loading states"
git push origin feature/user-profile-editing

# Update project board
# Move your task from "In Progress" to "Ready for Review"
```

---

## 🛡️ **Branch Protection Rules**

### **Main Branch Protection:**
```yaml
# GitHub Settings → Branches → main
✅ Require pull request reviews (2 reviewers)
✅ Dismiss stale reviews when new commits pushed
✅ Require status checks to pass
✅ Require branches to be up to date
✅ Include administrators
✅ Restrict pushes (only through PRs)
```

### **Develop Branch Protection:**
```yaml
# GitHub Settings → Branches → develop  
✅ Require pull request reviews (1 reviewer)
✅ Require status checks to pass
✅ Allow force pushes (for rebasing)
```

---

## 📊 **Automation & CI/CD Integration**

### **Automated Workflows:**
```yaml
# .github/workflows/feature-branch.yml
name: Feature Branch CI
on:
  pull_request:
    branches: [develop]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run Tests
        run: |
          npm install
          npm test
          npm run lint
          npm run build
          
  deploy-preview:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy Preview
        run: ./deploy-preview.sh
        env:
          BRANCH_NAME: ${{ github.head_ref }}
```

### **Deployment Pipeline:**
```bash
# Automatic deployments:
develop branch → staging environment
main branch → production environment

# Manual deployments:
feature branches → preview environments (for testing)
```

---

## 🎯 **Common Team Challenges & Solutions**

### **Challenge: Merge Conflicts**
```bash
# Solution: Regular syncing
git checkout feature/my-feature
git fetch origin
git rebase origin/develop  # Do this daily!
```

### **Challenge: Large Feature Development**
```bash
# Solution: Break into smaller PRs
# Instead of 1 massive PR:
feature/user-system

# Create multiple focused PRs:
feature/user-authentication  
feature/user-profile
feature/user-preferences
feature/user-settings
```

### **Challenge: Code Review Bottlenecks**
```bash
# Solutions:
- Smaller PRs (< 400 lines changed)
- Clear PR descriptions  
- Self-review before requesting review
- Review others' PRs promptly
- Pair programming for complex changes
```

---

## 💡 **Pro Tips for Team Success**

### **1. Communication is Key**
```bash
# Good practices:
- Comment on complex code
- Explain architectural decisions
- Ask questions when unsure
- Share knowledge in PR descriptions
```

### **2. Consistent Code Quality**
```bash
# Team standards:
- Same linting rules (.eslintrc)
- Same formatting (.prettierrc) 
- Code review checklist
- Automated testing requirements
```

### **3. Documentation**
```bash
# Keep updated:
- README.md (setup instructions)
- API documentation  
- Architecture decisions (ADRs)
- Deployment processes
```

---

## 🎯 **Success Metrics**

### **Healthy Team Metrics:**
- **PR Review Time**: < 24 hours average
- **Build Success Rate**: > 95%
- **Deployment Frequency**: Multiple times per week
- **Rollback Rate**: < 5% of deployments

### **Individual Developer Success:**
- Regular commits (daily)
- Clean PR history
- Helpful code reviews
- Knowledge sharing

---

## 💡 **Key Takeaways**

1. **Sync frequently** - avoid big merge conflicts
2. **Small PRs** get reviewed faster and merged easier
3. **Communication** prevents most team conflicts
4. **Automation** catches issues before production
5. **Branch protection** enforces quality standards
6. **Regular releases** keep velocity high
7. **Team practices** matter more than individual skills

---

**Next**: [Handling Merge Conflicts Like a Pro](03-handling-merge-conflicts.md) - Master conflict resolution!