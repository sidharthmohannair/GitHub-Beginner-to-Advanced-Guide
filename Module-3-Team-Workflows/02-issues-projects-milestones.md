# Issues, Projects & Milestones

## 🎯 Learning Goals
By the end of this lesson, you'll master:
- Creating and managing GitHub Issues effectively
- Using GitHub Projects for project organization
- Setting up Milestones for release planning
- Linking Issues, PRs, and Projects together
- Best practices for team project management

---

## 🎫 GitHub Issues

**GitHub Issues** are the foundation of project management on GitHub. Think of them as tickets in a help desk system.

### What Are Issues Good For?
- **Bug Reports**: Document problems in your code
- **Feature Requests**: Propose new functionality  
- **Tasks**: Track work that needs to be done
- **Questions**: Ask for help or clarification
- **Discussions**: Brainstorm ideas with your team

### Creating Effective Issues

#### Basic Issue Structure
```markdown
## 🐛 Bug Report: Login button not working on mobile

### Description
The login button is not clickable on mobile Safari (iOS 15+).

### Steps to Reproduce
1. Open the app on mobile Safari
2. Navigate to login page
3. Try to tap the login button
4. Nothing happens

### Expected Behavior
Button should trigger login process

### Actual Behavior
Button appears clickable but doesn't respond to taps

### Environment
- Device: iPhone 12 Pro
- OS: iOS 15.2
- Browser: Safari
- App Version: 1.2.3

### Screenshots
[Add screenshots here]

### Additional Context
This might be related to the recent CSS changes in PR #145
```

#### Feature Request Template
```markdown
## ✨ Feature Request: Dark mode support

### Problem
Users have requested a dark mode option for better nighttime viewing.

### Proposed Solution
Add a toggle button in the settings that switches between light and dark themes.

### Alternative Solutions
- Auto-detect system theme preference
- Time-based automatic switching

### Benefits
- Better user experience
- Reduced eye strain
- Modern app appearance

### Acceptance Criteria
- [ ] Toggle button in settings
- [ ] Dark theme for all pages
- [ ] Preference saved in localStorage
- [ ] Smooth transition between themes
```

### Issue Labels and Organization

#### Default Labels
- `bug` - Something isn't working
- `enhancement` - New feature or request
- `question` - Further information is requested
- `good first issue` - Good for newcomers
- `help wanted` - Extra attention is needed

#### Custom Labels for Teams
```bash
# Priority Labels
priority:high    # Urgent issues
priority:medium  # Normal priority
priority:low     # Nice to have

# Type Labels  
type:bug        # Bug reports
type:feature    # New features
type:docs       # Documentation
type:refactor   # Code refactoring

# Status Labels
status:blocked   # Cannot proceed
status:in-progress # Currently being worked on
status:needs-review # Ready for review

# Area Labels
area:frontend   # Frontend related
area:backend    # Backend related  
area:database   # Database related
area:security   # Security concerns
```

### Advanced Issue Features

#### Issue Templates
Create `.github/ISSUE_TEMPLATE/bug_report.md`:
```markdown
---
name: Bug report
about: Create a report to help us improve
title: '[BUG] '
labels: 'bug'
assignees: ''
---

## Describe the bug
A clear and concise description of what the bug is.

## To Reproduce
Steps to reproduce the behavior:
1. Go to '...'
2. Click on '....'
3. Scroll down to '....'
4. See error

## Expected behavior
A clear and concise description of what you expected to happen.

## Screenshots
If applicable, add screenshots to help explain your problem.

## Environment:
 - OS: [e.g. iOS]
 - Browser [e.g. chrome, safari]
 - Version [e.g. 22]

## Additional context
Add any other context about the problem here.
```

#### Linking Issues and PRs
```markdown
# In PR descriptions:
Closes #123
Fixes #456  
Resolves #789

# In commit messages:
git commit -m "Fix login validation issue

Addresses the validation bug reported in #123 by adding
proper email format checking.

Fixes #123"

# In issue comments:
This is related to #456 and might also fix #789
```

---

## 📋 GitHub Projects

**GitHub Projects** provide kanban-style boards for organizing and tracking work across issues and pull requests.

### Project Types

#### Repository Projects
- Attached to a single repository
- Good for smaller projects
- Easier to set up and manage

#### Organization Projects  
- Can span multiple repositories
- Good for larger initiatives
- More complex setup but more powerful

### Setting Up a Project

#### 1. Create New Project
1. Go to repository → Projects tab → New project
2. Choose template:
   - **Basic kanban**: Simple To do/In progress/Done
   - **Automated kanban**: Auto-moves cards based on PR status  
   - **Automated kanban with reviews**: Includes review columns
   - **Bug triage**: Optimized for bug management

#### 2. Customize Columns
```
📝 Backlog      → Ideas and future work
🏃 To Do        → Ready to start  
⚡ In Progress  → Currently being worked on
👀 In Review    → PRs waiting for review
✅ Done         → Completed work
```

#### 3. Add Cards
- **Create issues** directly from project board
- **Add existing issues** by dragging from issues tab
- **Add pull requests** to track development progress
- **Add notes** for quick tasks that don't need full issues

### Project Automation

#### Automated Actions
```markdown
## Built-in Automations

### To do column
- Newly added issues → Move here
- Reopened issues → Move here

### In progress column  
- Issues assigned → Move here
- PRs opened → Move here

### In review column
- PRs ready for review → Move here

### Done column
- Issues closed → Move here
- PRs merged → Move here
```

#### GitHub Actions for Projects
```yaml
# .github/workflows/project-automation.yml
name: Move issues to project board

on:
  issues:
    types: [opened, assigned]
  pull_request:
    types: [opened, ready_for_review]

jobs:
  move-to-project:
    runs-on: ubuntu-latest
    steps:
      - uses: alex-page/github-project-automation-plus@v0.8.1
        with:
          project: Development Board
          column: To Do
          repo-token: ${{ secrets.GITHUB_TOKEN }}
```

### Project Best Practices

#### 1. Clear Column Definitions
```markdown
## Column Guidelines

### 📝 Backlog
- Ideas not yet planned
- Low priority items  
- Future considerations

### 🏃 To Do
- Issues ready to start
- All requirements clear
- Properly sized for sprint

### ⚡ In Progress
- Currently being worked on
- Assigned to team member
- Should have linked PR

### 👀 In Review
- PRs waiting for review
- Ready for testing
- Documentation complete

### ✅ Done
- Completed and deployed
- Tests passing
- Stakeholder approved
```

#### 2. Regular Maintenance
```bash
# Weekly project board cleanup:
# - Move stale cards to appropriate columns
# - Close completed issues
# - Update card descriptions
# - Assign team members to new work
# - Remove duplicate or outdated cards
```

---

## 🎯 Milestones

**Milestones** group issues and pull requests for releases or project phases.

### Creating Effective Milestones

#### Release Milestone Example
```markdown
Title: Version 2.1.0 Release
Description: Major feature release including dark mode, 
improved performance, and bug fixes from user feedback.

Due Date: March 15, 2024

Target Issues: 15-20 issues
Completion: 0% (0 closed, 20 open)
```

#### Sprint Milestone Example  
```markdown
Title: Sprint 12 - User Authentication
Description: Focus on completing user authentication 
system including login, registration, and password reset.

Due Date: February 1, 2024

Target Issues: 8-12 issues
Completion: 60% (6 closed, 4 open)
```

### Milestone Planning

#### 1. Release Planning
```bash
# Create milestones for each planned release
v2.0.0 - Major feature release
v2.0.1 - Bug fix patch
v2.1.0 - Minor feature addition
v3.0.0 - Breaking changes release

# Add realistic due dates
# Group related issues together
# Include both features and bug fixes
```

#### 2. Sprint Planning  
```bash
# For agile teams using sprints
Sprint 1 - Setup and Infrastructure
Sprint 2 - Core Features Development  
Sprint 3 - User Interface Implementation
Sprint 4 - Testing and Bug Fixes
Sprint 5 - Performance and Polish
```

### Milestone Management

#### Tracking Progress
```markdown
## Milestone Dashboard View

### 📊 Sprint 12 Progress
- **Due Date**: February 1, 2024
- **Progress**: 75% complete
- **Open Issues**: 2 of 8
- **Days Remaining**: 5 days

### 🎯 Remaining Work
- [ ] #156: Fix password validation
- [ ] #162: Add "Remember me" checkbox

### ✅ Completed Work  
- [x] #145: Create login form
- [x] #148: Add registration endpoint
- [x] #151: Implement JWT authentication
- [x] #153: Add password reset flow
- [x] #157: Create user dashboard
- [x] #160: Add email verification
```

#### Milestone Reports
```bash
# Generate milestone reports
# - Burndown charts
# - Velocity tracking  
# - Issue completion rates
# - Time to resolution metrics
```

---

## 🔗 Connecting Issues, PRs, and Projects

### Workflow Integration

#### Complete Feature Development Flow
```markdown
## Feature: Dark Mode Implementation

### 1. Planning Phase
- Create milestone: "v2.1.0 - UI Improvements"
- Create epic issue: #200 "Add dark mode support"
- Break down into smaller issues:
  - #201: Design dark mode color scheme
  - #202: Implement theme toggle component  
  - #203: Add dark mode CSS variables
  - #204: Update all components for dark mode
  - #205: Add theme persistence
  - #206: Test across all browsers

### 2. Development Phase  
- Create project board: "Dark Mode Implementation"
- Add all issues to board
- Assign team members
- Create feature branch: `feature/dark-mode`

### 3. Implementation
- Move #201 to "In Progress"
- Create PR #210 for color scheme
- Link PR to issue: "Addresses #201"
- Move card to "In Review"

### 4. Completion
- Merge PR #210
- Issue #201 automatically closes
- Card moves to "Done" 
- Update milestone progress

### 5. Release
- All milestone issues complete
- Create release branch
- Deploy to production  
- Close milestone
```

### Advanced Linking Strategies

#### Cross-Repository Projects
```markdown
# For organizations with multiple repos
Repository A (Frontend): Issues #100-110
Repository B (Backend): Issues #200-210  
Repository C (Documentation): Issues #300-310

# Link related issues across repos:
"Frontend implementation in orgname/frontend#105"
"Depends on API changes in orgname/backend#203"
"Documentation needed in orgname/docs#307"
```

#### Epic Issue Management
```markdown
## Epic: User Authentication System

### Overview
Complete user authentication including login, registration, 
password reset, and user profile management.

### Related Issues
- [ ] #101: Design authentication flow
- [ ] #102: Create user database schema  
- [x] #103: Implement registration API
- [x] #104: Create login form component
- [ ] #105: Add password reset functionality
- [ ] #106: Build user profile page
- [ ] #107: Add email verification
- [ ] #108: Implement 2FA support

### Progress: 25% (2 of 8 issues completed)
### Target Milestone: v2.0.0
### Estimated Completion: March 1, 2024
```

---

## 🎯 Team Management Best Practices

### 1. Issue Assignment Strategy

#### Individual Assignment
```bash
# Assign specific issues to team members
@johndoe - Frontend development issues
@janesmith - Backend API issues  
@bobwilson - DevOps and deployment issues
@alicechen - Testing and QA issues
```

#### Team Assignment
```bash
# Use team mentions for broader discussions
@orgname/frontend-team - UI/UX related issues
@orgname/backend-team - API and database issues
@orgname/qa-team - Testing and quality issues
```

### 2. Communication Protocols

#### Issue Updates
```markdown
## Required Updates for In-Progress Issues

### Daily Standups
- Comment with progress updates
- Mention any blockers
- Update time estimates

### Weekly Reviews
- Update issue descriptions if scope changes
- Add new acceptance criteria if needed
- Link related PRs and issues

### Milestone Planning
- Estimate effort for new issues
- Break down large issues into smaller ones
- Assign issues to appropriate milestones
```

### 3. Quality Standards

#### Issue Quality Checklist
```markdown
- [ ] Clear, descriptive title
- [ ] Detailed description with context
- [ ] Acceptance criteria defined
- [ ] Appropriate labels applied
- [ ] Milestone assigned (if planned)
- [ ] Team member assigned (if ready)
- [ ] Related issues linked
- [ ] Screenshots/mockups included (if applicable)
```

---

## 🎓 Knowledge Check

**Question 1**: What's the difference between Issues and Projects?
<details>
<summary>Click to see answer</summary>
- **Issues**: Individual work items (bugs, features, tasks)
- **Projects**: Organizational boards that group and track multiple issues
- Issues are the "what", Projects are the "how we organize and track"
</details>

**Question 2**: When should you create a Milestone?
<details>
<summary>Click to see answer</summary>
Create milestones for:
- Software releases (v1.0, v2.0)
- Sprint planning (2-week development cycles)
- Project phases (Design, Development, Testing)
- Time-bound objectives (quarterly goals)
</details>

**Question 3**: How do you automatically close an issue with a PR?
<details>
<summary>Click to see answer</summary>
Use keywords in the PR description:
- "Closes #123"
- "Fixes #123"  
- "Resolves #123"
When the PR is merged, the issue automatically closes.
</details>

---

## 🎯 Key Takeaways

1. **Issues are the foundation** of GitHub project management
2. **Use templates** to ensure consistent issue quality
3. **Projects provide visual organization** with kanban boards
4. **Milestones group work** by release or time period
5. **Link everything together** for complete traceability
6. **Automate where possible** to reduce manual overhead
7. **Regular maintenance** keeps projects organized and useful

---

## ➡️ What's Next?

Now let's explore GitHub's features for creating templates, managing releases, and using labels effectively.

**Next lesson**: [Templates, Labels & Releases](03-templates-labels-releases.md)

---

## 🎮 Practice Exercises

### Exercise 1: Project Setup
1. Create a new repository for a sample project
2. Set up issue templates
3. Create a project board with custom columns
4. Add 10 sample issues with proper labels
5. Practice moving issues through the workflow

### Exercise 2: Release Planning
1. Create a milestone for your next release
2. Group related issues into the milestone
3. Practice tracking progress
4. Create an epic issue linking multiple smaller issues

### Exercise 3: Team Collaboration
1. Work with a teammate on the same project
2. Assign issues to each other
3. Practice communication through issue comments
4. Use project board to coordinate work

---

## 📚 Additional Resources

- [GitHub Issues Documentation](https://docs.github.com/en/issues)
- [GitHub Projects Guide](https://docs.github.com/en/issues/planning-and-tracking-with-projects)
- [Milestones Documentation](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work/about-milestones)
- [Issue Templates Guide](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests)