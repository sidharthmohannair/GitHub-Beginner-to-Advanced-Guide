# GitHub Flow vs Git Flow

## 🎯 Learning Goals
By the end of this lesson, you'll understand:
- The difference between GitHub Flow and Git Flow
- When to use each workflow
- How to implement both workflows effectively
- Choosing the right workflow for your team
- Best practices for team collaboration

---

## 🌊 What are Git Workflows?

**Git workflows** are branching strategies that teams use to organize their development process. They provide:
- **Structure** for how features are developed
- **Rules** for when and how to merge code
- **Process** for releasing software
- **Coordination** between team members

### Popular Workflows
1. **GitHub Flow** - Simple, web-focused workflow
2. **Git Flow** - Feature-rich workflow for complex releases
3. **GitLab Flow** - Combines simplicity with environment-specific branches
4. **Forking Workflow** - Best for open source projects

---

## 🚀 GitHub Flow

**GitHub Flow** is a lightweight, branch-based workflow perfect for teams that deploy frequently.

### Core Principles
```
Main branch → Always deployable
Feature branches → Short-lived, focused changes
Pull Requests → Code review and discussion
Deploy → From main branch frequently
```

### The GitHub Flow Process

#### Step 1: Create a Branch
```bash
# Always branch from main
git checkout main
git pull origin main
git checkout -b feature/user-authentication
```

#### Step 2: Add Commits
```bash
# Make focused commits
git add .
git commit -m "Add login form component"
git commit -m "Implement password validation"
git commit -m "Add authentication API integration"
```

#### Step 3: Open Pull Request
- Create PR early (even as draft)
- Describe changes clearly
- Request specific reviewers
- Link to related issues

#### Step 4: Discuss and Review
- Team reviews code
- Automated tests run
- Make changes based on feedback
- Get approval from reviewers

#### Step 5: Deploy
```bash
# Deploy from feature branch for testing (optional)
# Or deploy after merge to main
```

#### Step 6: Merge
```bash
# Merge to main after approval
# Delete feature branch
git branch -d feature/user-authentication
```

### GitHub Flow Example

```bash
# Day 1: Start new feature
git checkout main
git pull origin main
git checkout -b feature/shopping-cart

# Work on feature
echo "Shopping cart implementation" > cart.js
git add cart.js
git commit -m "Add basic shopping cart functionality"
git push -u origin feature/shopping-cart

# Create PR immediately for early feedback
# Continue working based on reviews

# Day 3: Feature ready
git push origin feature/shopping-cart
# Merge PR after approval
# Deploy to production
```

### When to Use GitHub Flow
✅ **Perfect for:**
- Web applications with continuous deployment
- Small to medium teams
- Projects with frequent releases
- Teams comfortable with main-branch deployment
- Simple, straightforward projects

❌ **Not ideal for:**
- Projects with scheduled releases
- Multiple version maintenance
- Complex integration requirements
- Teams needing extensive QA processes

---

## 🌳 Git Flow

**Git Flow** is a more complex workflow designed for projects with scheduled releases and multiple environment support.

### Core Branches
1. **main/master** - Production-ready code
2. **develop** - Integration branch for features
3. **feature/** - Individual feature development
4. **release/** - Preparing new releases
5. **hotfix/** - Critical production fixes

### Git Flow Diagram
```
    main     ●───●───●───●───●  (production releases)
             │   │   │   │   │
   hotfix   ╱●───●╱ ╱●───●╱   (emergency fixes)
           ╱     ╱ ╱     ╱
  develop ●───●───●───●───●     (integration)
          │ ╲ │ ╱ │ ╲ │ ╱
 feature/ ●───● ●───● ●───●     (features)
```

### Git Flow Process

#### Feature Development
```bash
# Start new feature from develop
git checkout develop
git pull origin develop
git checkout -b feature/payment-integration

# Work on feature
git add .
git commit -m "Add payment form"
git commit -m "Integrate with payment API"

# Merge back to develop
git checkout develop
git merge --no-ff feature/payment-integration
git branch -d feature/payment-integration
git push origin develop
```

#### Release Process
```bash
# Start release branch
git checkout develop
git pull origin develop
git checkout -b release/v1.2.0

# Final preparations (version bumps, bug fixes)
echo "version = '1.2.0'" > version.py
git add version.py
git commit -m "Bump version to 1.2.0"

# Merge to main
git checkout main
git merge --no-ff release/v1.2.0
git tag -a v1.2.0 -m "Release version 1.2.0"

# Merge back to develop
git checkout develop
git merge --no-ff release/v1.2.0

# Clean up
git branch -d release/v1.2.0
```

#### Hotfix Process
```bash
# Critical bug in production
git checkout main
git pull origin main
git checkout -b hotfix/security-patch

# Fix the issue
git add .
git commit -m "Fix security vulnerability in login"

# Merge to main
git checkout main
git merge --no-ff hotfix/security-patch
git tag -a v1.2.1 -m "Hotfix version 1.2.1"

# Merge to develop
git checkout develop
git merge --no-ff hotfix/security-patch

# Clean up
git branch -d hotfix/security-patch
```

### Using Git Flow Tools

```bash
# Install git-flow extension
# macOS: brew install git-flow
# Ubuntu: sudo apt-get install git-flow

# Initialize git flow
git flow init

# Start feature
git flow feature start payment-integration

# Finish feature
git flow feature finish payment-integration

# Start release
git flow release start 1.2.0

# Finish release
git flow release finish 1.2.0

# Start hotfix
git flow hotfix start security-patch

# Finish hotfix
git flow hotfix finish security-patch
```

### When to Use Git Flow
✅ **Perfect for:**
- Software with scheduled releases
- Multiple version support
- Large teams with defined roles
- Projects requiring extensive QA
- Desktop/mobile applications
- Enterprise software

❌ **Not ideal for:**
- Continuous deployment projects
- Simple web applications
- Small teams
- Projects needing quick iterations

---

## ⚖️ GitHub Flow vs Git Flow Comparison

| Aspect | GitHub Flow | Git Flow |
|--------|-------------|----------|
| **Complexity** | Simple | Complex |
| **Branches** | main + features | 5 branch types |
| **Release Cycle** | Continuous | Scheduled |
| **Team Size** | Small-Medium | Medium-Large |
| **Learning Curve** | Easy | Steep |
| **Flexibility** | High | Structured |
| **Production Hotfixes** | From main | Dedicated hotfix branches |
| **Best For** | Web apps, SaaS | Enterprise, versioned software |

---

## 🛠️ Implementing Team Workflows

### Setting Up GitHub Flow

#### 1. Repository Configuration
```bash
# Protect main branch
# Go to GitHub → Settings → Branches
# Add rule for main branch:
# ✓ Require pull request reviews
# ✓ Dismiss stale reviews
# ✓ Require status checks
# ✓ Require branches to be up to date
# ✓ Include administrators
```

#### 2. Team Guidelines
```markdown
## GitHub Flow Team Rules

1. **Main branch is always deployable**
   - Never commit directly to main
   - All changes go through Pull Requests
   - Automated tests must pass

2. **Feature branches are short-lived**
   - Create from main
   - Focus on single feature/bug fix
   - Delete after merging

3. **Pull Request process**
   - Create early, even as draft
   - Request at least 1 reviewer
   - Address feedback promptly
   - Squash commits if many small ones

4. **Deployment**
   - Deploy from main branch
   - Monitor deployments
   - Rollback if issues detected
```

#### 3. Automation Setup
```yaml
# .github/workflows/main.yml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Run tests
      run: npm test
  
  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
    - name: Deploy to production
      run: ./deploy.sh
```

### Setting Up Git Flow

#### 1. Branch Protection Rules
```bash
# Protect both main and develop branches
# Main: Only release and hotfix merges allowed
# Develop: Only feature and release merges allowed
```

#### 2. Team Guidelines
```markdown
## Git Flow Team Rules

1. **Branch Usage**
   - main: Production code only
   - develop: Integration of features
   - feature/*: Individual feature development
   - release/*: Release preparation
   - hotfix/*: Production emergency fixes

2. **Feature Development**
   - Branch from develop
   - Merge back to develop with --no-ff
   - Delete feature branch after merge

3. **Release Process**
   - Create release branch from develop
   - Final testing and bug fixes only
   - Merge to both main and develop
   - Tag main with version number

4. **Hotfix Process**
   - Branch from main
   - Merge to both main and develop
   - Tag with patch version
```

---

## 🎯 Best Practices for Team Workflows

### 1. Communication
```markdown
✅ **Good Communication**
- Clear commit messages
- Descriptive PR titles and descriptions
- Regular team sync meetings
- Document workflow decisions

❌ **Poor Communication**
- Vague commit messages ("fix stuff")
- Empty PR descriptions
- Working in isolation
- Undocumented process changes
```

### 2. Code Quality
```bash
# Set up automated checks
- Linting (ESLint, Prettier, etc.)
- Testing (unit, integration, e2e)
- Security scanning
- Code coverage requirements

# Example: package.json scripts
{
  "scripts": {
    "lint": "eslint src/",
    "test": "jest",
    "test:coverage": "jest --coverage",
    "security": "npm audit"
  }
}
```

### 3. Documentation
```markdown
## Required Documentation

### For the Team
- Workflow choice and reasoning
- Branch naming conventions
- PR review process
- Deployment procedures
- Emergency procedures

### For New Team Members
- Setup instructions
- Development workflow
- Code style guidelines
- Review checklist
- Common problems and solutions
```

---

## 🔄 Transitioning Between Workflows

### From No Workflow to GitHub Flow
```bash
# Week 1: Set up branch protection
# Week 2: Introduce PR process
# Week 3: Add automated testing
# Week 4: Implement continuous deployment
```

### From GitHub Flow to Git Flow
```bash
# When you need Git Flow:
# - Multiple versions to maintain
# - Scheduled release cycles
# - More complex QA process
# - Larger team coordination

# Transition steps:
# 1. Create develop branch from main
# 2. Update team guidelines
# 3. Install git-flow tools
# 4. Train team on new process
```

---

## 🎓 Knowledge Check

**Question 1**: What's the main difference between GitHub Flow and Git Flow?
<details>
<summary>Click to see answer</summary>
- **GitHub Flow**: Simple workflow with main branch + feature branches, continuous deployment
- **Git Flow**: Complex workflow with multiple branch types (main, develop, feature, release, hotfix), scheduled releases
- GitHub Flow is simpler but Git Flow provides more structure for complex projects
</details>

**Question 2**: When should you use Git Flow instead of GitHub Flow?
<details>
<summary>Click to see answer</summary>
Use Git Flow when you have:
- Scheduled releases (not continuous deployment)
- Multiple versions to maintain
- Large teams needing more structure
- Complex QA processes
- Enterprise software requirements
</details>

**Question 3**: What's the purpose of the develop branch in Git Flow?
<details>
<summary>Click to see answer</summary>
The develop branch serves as:
- Integration point for completed features
- Base for creating new feature branches
- Source for creating release branches
- Always contains latest completed development work
- Buffer between unstable features and production (main)
</details>

---

## 🎯 Key Takeaways

1. **Choose workflow based on project needs** - not team preferences
2. **GitHub Flow is simple and effective** for most web projects
3. **Git Flow provides structure** for complex release cycles
4. **Consistency matters more** than the specific workflow chosen
5. **Document and train** your team on the chosen workflow
6. **Automate workflow enforcement** with branch protection and CI/CD
7. **Be willing to adapt** as project needs change

---

## ➡️ What's Next?

Now that you understand team workflows, let's explore GitHub's project management features for organizing and tracking work.

**Next lesson**: [Issues, Projects & Milestones](02-issues-projects-milestones.md)

---

## 🎮 Practice Exercises

### Exercise 1: Implement GitHub Flow
1. Create a new repository
2. Set up branch protection rules
3. Practice the complete GitHub Flow cycle
4. Add automated testing
5. Document your process

### Exercise 2: Try Git Flow
1. Install git-flow tools
2. Initialize a repository with Git Flow
3. Practice feature, release, and hotfix workflows
4. Compare complexity with GitHub Flow

### Exercise 3: Team Simulation
1. Work with a friend on the same repository
2. Practice both workflows
3. Experience merge conflicts and resolution
4. Evaluate which workflow works better for your collaboration style

---

## 📚 Additional Resources

- [GitHub Flow Guide](https://guides.github.com/introduction/flow/)
- [Git Flow Original Post](https://nvie.com/posts/a-successful-git-branching-model/)
- [GitLab Flow Documentation](https://docs.gitlab.com/ee/topics/gitlab_flow.html)
- [Comparing Git Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)