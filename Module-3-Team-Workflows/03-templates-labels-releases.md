# Templates, Labels & Releases

## 🎯 Learning Goals
By the end of this lesson, you'll master:
- Creating and managing issue and PR templates
- Designing effective label systems
- Managing software releases on GitHub
- Automating release processes
- Best practices for project standardization

---

## 📝 GitHub Templates

Templates ensure consistency and quality in issues and pull requests across your team.

### Issue Templates

#### Creating Issue Templates

Create `.github/ISSUE_TEMPLATE/bug_report.yml`:
```yaml
name: 🐛 Bug Report
description: Report a bug to help us improve
title: "[BUG] "
labels: ["bug", "needs-triage"]
body:
  - type: markdown
    attributes:
      value: |
        Thanks for taking the time to report this bug! 🐛
        Please fill out the information below to help us fix it.

  - type: textarea
    id: description
    attributes:
      label: Bug Description
      description: A clear and concise description of the bug
      placeholder: Describe what happened...
    validations:
      required: true

  - type: textarea
    id: reproduction
    attributes:
      label: Steps to Reproduce
      description: Steps to reproduce the behavior
      placeholder: |
        1. Go to '...'
        2. Click on '...'
        3. See error
    validations:
      required: true

  - type: textarea
    id: expected
    attributes:
      label: Expected Behavior
      description: What you expected to happen
    validations:
      required: true

  - type: dropdown
    id: browser
    attributes:
      label: Browser
      description: Which browser are you using?
      options:
        - Chrome
        - Firefox
        - Safari
        - Edge
        - Other
    validations:
      required: true

  - type: input
    id: version
    attributes:
      label: Version
      description: What version of the app are you using?
      placeholder: "1.0.0"
    validations:
      required: true
```

#### Feature Request Template

Create `.github/ISSUE_TEMPLATE/feature_request.yml`:
```yaml
name: ✨ Feature Request
description: Suggest a new feature or enhancement
title: "[FEATURE] "
labels: ["enhancement", "needs-discussion"]
body:
  - type: textarea
    id: problem
    attributes:
      label: Problem Statement
      description: What problem does this feature solve?
      placeholder: "I'm frustrated when..."
    validations:
      required: true

  - type: textarea
    id: solution
    attributes:
      label: Proposed Solution
      description: Describe your ideal solution
      placeholder: "I would like..."
    validations:
      required: true

  - type: textarea
    id: alternatives
    attributes:
      label: Alternative Solutions
      description: Any alternative solutions you've considered
      placeholder: "Alternative approaches..."

  - type: checkboxes
    id: checklist
    attributes:
      label: Prerequisites
      description: Please confirm the following
      options:
        - label: I have searched existing issues
          required: true
        - label: This feature would benefit other users
          required: true
```

### Pull Request Templates

#### Basic PR Template

Create `.github/pull_request_template.md`:
```markdown
## 🎯 What This PR Does

Brief description of changes

## 📋 Changes Made

- [ ] Feature/bug fix implemented
- [ ] Tests added/updated
- [ ] Documentation updated
- [ ] Code reviewed

## 🧪 Testing

Describe how you tested these changes:

- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed

## 📱 Screenshots/Videos

<!-- Add screenshots for UI changes -->

## 🔗 Related Issues

Closes #
Related to #

## 📝 Additional Notes

Any additional information for reviewers

---

### Review Checklist

- [ ] Code follows project style guidelines
- [ ] Self-review completed
- [ ] Tests added for new functionality
- [ ] Breaking changes documented
- [ ] Security considerations addressed
```

#### Advanced PR Template with Multiple Options

Create `.github/PULL_REQUEST_TEMPLATE/feature.md`:
```markdown
## 🚀 Feature Pull Request

### Feature Description
What new functionality does this add?

### User Story
As a [user type], I want [functionality] so that [benefit].

### Implementation Details
- **Frontend Changes**: 
- **Backend Changes**: 
- **Database Changes**: 
- **API Changes**: 

### Testing Strategy
- [ ] Unit tests added
- [ ] Integration tests added
- [ ] E2E tests updated
- [ ] Performance testing done

### Documentation
- [ ] README updated
- [ ] API documentation updated
- [ ] Code comments added
- [ ] Changelog updated

### Deployment Notes
Any special deployment considerations?

### Screenshots
[Add screenshots of new functionality]
```

---

## 🏷️ Label Management

Labels help categorize and prioritize issues and pull requests effectively.

### Essential Label Categories

#### 1. Type Labels
```bash
# What kind of work is this?
type:bug          # Something isn't working
type:feature      # New functionality  
type:enhancement  # Improve existing feature
type:docs         # Documentation changes
type:refactor     # Code cleanup/restructure
type:test         # Testing related
type:chore        # Maintenance tasks
```

#### 2. Priority Labels
```bash
priority:critical  # Drop everything and fix
priority:high      # Important, should be next
priority:medium    # Normal priority
priority:low       # Nice to have
```

#### 3. Status Labels
```bash
status:blocked       # Cannot proceed
status:in-progress   # Currently being worked on
status:needs-review  # Ready for code review
status:needs-info    # Waiting for more information
status:duplicate     # Duplicate of another issue
status:wontfix       # Won't be addressed
```

#### 4. Area Labels
```bash
area:frontend    # User interface
area:backend     # Server-side code
area:database    # Data storage
area:api         # API related
area:security    # Security concerns
area:performance # Speed/efficiency
area:accessibility # A11y improvements
```

#### 5. Effort Labels
```bash
effort:xs    # < 1 hour
effort:s     # 1-4 hours  
effort:m     # 1-2 days
effort:l     # 3-5 days
effort:xl    # 1+ weeks
```

### Creating Label Templates

#### Label Configuration File
Create `.github/labels.yml`:
```yaml
# Type labels
- name: "type:bug"
  color: "d73a4a"
  description: "Something isn't working"

- name: "type:feature" 
  color: "a2eeef"
  description: "New feature or request"

- name: "type:enhancement"
  color: "84b6eb"
  description: "Enhancement to existing feature"

# Priority labels
- name: "priority:critical"
  color: "b60205"
  description: "Critical priority"

- name: "priority:high"
  color: "d93f0b"
  description: "High priority"

- name: "priority:medium"
  color: "fbca04"
  description: "Medium priority"

- name: "priority:low"
  color: "0e8a16"
  description: "Low priority"

# Status labels  
- name: "status:blocked"
  color: "000000"
  description: "Blocked and cannot proceed"

- name: "status:in-progress"
  color: "yellow"
  description: "Currently being worked on"

# Area labels
- name: "area:frontend"
  color: "5319e7"
  description: "Frontend related"

- name: "area:backend"
  color: "0052cc"
  description: "Backend related"
```

### Label Management Scripts

#### Bulk Label Creation
```bash
# Using GitHub CLI to create labels
gh label create "type:bug" --color "d73a4a" --description "Something isn't working"
gh label create "type:feature" --color "a2eeef" --description "New feature"
gh label create "priority:high" --color "d93f0b" --description "High priority"

# Batch create from file
# Create labels.txt with format: name|color|description
while IFS='|' read -r name color desc; do
    gh label create "$name" --color "$color" --description "$desc"
done < labels.txt
```

### Label Best Practices

#### 1. Consistent Naming Convention
```bash
# ✅ Good: Consistent prefix pattern
type:bug, type:feature, type:docs
priority:high, priority:medium, priority:low
area:frontend, area:backend, area:mobile

# ❌ Bad: Inconsistent naming
bug, new-feature, documentation  
urgent, normal-priority, low
ui, server, mobile-app
```

#### 2. Color Coding Strategy
```bash
# Use color patterns for recognition
Red tones: Critical issues, bugs
Blue tones: Features, enhancements
Green tones: Low priority, completed
Yellow: Warnings, medium priority
Purple: Areas/components
Gray: Status labels
```

#### 3. Label Automation
```yaml
# .github/workflows/label-automation.yml
name: Auto-label Issues and PRs

on:
  issues:
    types: [opened]
  pull_request:
    types: [opened]

jobs:
  label:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/labeler@v4
        with:
          configuration-path: .github/labeler.yml
          repo-token: ${{ secrets.GITHUB_TOKEN }}
```

Create `.github/labeler.yml`:
```yaml
# Add 'area:frontend' label to any changes in frontend directories
area:frontend:
  - 'src/components/**/*'
  - 'src/styles/**/*'
  - 'public/**/*'

# Add 'area:backend' label to API or server changes
area:backend:
  - 'api/**/*'
  - 'server/**/*'
  - 'database/**/*'

# Add 'type:docs' label to documentation changes
type:docs:
  - 'docs/**/*'
  - 'README.md'
  - '**/*.md'
```

---

## 🚀 GitHub Releases

Releases package your software for distribution and mark important milestones.

### Creating Releases

#### Manual Release Creation

1. **Go to repository → Releases → Create a new release**
2. **Choose or create a tag**: `v1.2.0`
3. **Release title**: `Version 1.2.0 - Dark Mode & Performance Improvements`
4. **Description**: Detailed changelog
5. **Attach binaries** (if applicable)
6. **Publish release**

#### Release Description Template
```markdown
# Version 1.2.0 - Dark Mode & Performance Improvements

## 🚀 New Features
- ✨ Added dark mode support with system theme detection
- 🎨 New customizable dashboard layout
- 📱 Improved mobile navigation experience

## 🐛 Bug Fixes  
- 🔧 Fixed login redirect issue on Safari
- 🔧 Resolved memory leak in data visualization
- 🔧 Fixed responsive layout on tablet devices

## 🔄 Changes
- ⚡ Improved page load performance by 40%
- 📦 Updated dependencies to latest versions
- 🎯 Streamlined user onboarding flow

## 🛠️ Technical Changes
- Migrated from CSS-in-JS to CSS modules
- Added automated performance testing
- Updated Docker configuration

## 📋 Full Changelog
View all changes: https://github.com/user/repo/compare/v1.1.0...v1.2.0

## 🙏 Contributors
Thanks to @johndoe, @janesmith, and @bobwilson for their contributions!

---

### Installation
```bash
npm install your-package@1.2.0
```

### Docker
```bash
docker pull yourorg/yourapp:1.2.0
```

### Download Assets
- [Linux Binary](link-to-linux-binary)
- [macOS Binary](link-to-macos-binary)  
- [Windows Binary](link-to-windows-binary)
- [Source Code](link-to-source)
```

### Automated Releases

#### Using GitHub Actions for Releases
```yaml
# .github/workflows/release.yml
name: Create Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: Generate Changelog
        id: changelog
        uses: metcalfc/changelog-generator@v4.0.1
        with:
          myToken: ${{ secrets.GITHUB_TOKEN }}

      - name: Create Release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ github.ref }}
          release_name: Release ${{ github.ref }}
          body: ${{ steps.changelog.outputs.changelog }}
          draft: false
          prerelease: false

      - name: Build and Upload Assets
        run: |
          # Build your application
          npm run build
          
          # Create zip files
          zip -r release-linux.zip dist/
          
      - name: Upload Release Assets
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: ./release-linux.zip
          asset_name: release-linux.zip
          asset_content_type: application/zip
```

#### Semantic Release
```bash
# Install semantic-release
npm install --save-dev semantic-release

# .releaserc.json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator", 
    "@semantic-release/changelog",
    "@semantic-release/github",
    "@semantic-release/git"
  ]
}
```

### Release Management Best Practices

#### 1. Semantic Versioning
```bash
# Version format: MAJOR.MINOR.PATCH
1.0.0 → First stable release
1.0.1 → Bug fix (patch)
1.1.0 → New features (minor)
2.0.0 → Breaking changes (major)

# Pre-release versions
1.0.0-alpha.1 → Alpha release
1.0.0-beta.1  → Beta release  
1.0.0-rc.1    → Release candidate
```

#### 2. Release Cadence
```bash
# Regular release schedule options:
Weekly releases  → For web applications
Monthly releases → For mobile apps
Quarterly releases → For enterprise software
Ad-hoc releases → For libraries/tools

# Include in each release:
- New features
- Bug fixes  
- Performance improvements
- Security updates
```

#### 3. Release Communication
```markdown
## Release Communication Checklist

### Before Release
- [ ] Feature freeze announced
- [ ] Beta testing completed
- [ ] Documentation updated
- [ ] Migration guides written

### During Release
- [ ] Release notes published
- [ ] Stakeholders notified
- [ ] Social media announcement
- [ ] Blog post published

### After Release  
- [ ] Monitor for issues
- [ ] Respond to user feedback
- [ ] Plan next release cycle
- [ ] Update roadmap
```

---

## 🛠️ Advanced Template and Label Strategies

### Multi-Repository Templates

#### Organization-Wide Templates
```bash
# Create .github repository in your organization
# Templates in this repo apply to all repositories

organization/.github/
├── ISSUE_TEMPLATE/
│   ├── bug_report.yml
│   ├── feature_request.yml
│   └── config.yml
├── PULL_REQUEST_TEMPLATE.md
└── workflow-templates/
    ├── ci.yml
    └── ci.properties.json
```

#### Template Inheritance
```yaml
# .github/ISSUE_TEMPLATE/config.yml
blank_issues_enabled: false
contact_links:
  - name: Security Issues
    url: https://security.example.com
    about: Report security vulnerabilities privately
  - name: Community Discussion
    url: https://discussions.example.com
    about: Ask questions and discuss ideas
```

### Dynamic Label Assignment

#### Label Automation Rules
```yaml
# .github/workflows/label-management.yml
name: Smart Label Assignment

on:
  issues:
    types: [opened, edited]
  pull_request:
    types: [opened, edited]

jobs:
  assign-labels:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Assign Priority Labels
        uses: actions/github-script@v6
        with:
          script: |
            const issue = context.payload.issue || context.payload.pull_request;
            const body = issue.body.toLowerCase();
            
            // Auto-assign priority based on keywords
            if (body.includes('urgent') || body.includes('critical')) {
              await github.rest.issues.addLabels({
                owner: context.repo.owner,
                repo: context.repo.repo,
                issue_number: issue.number,
                labels: ['priority:high']
              });
            }
            
            // Auto-assign type based on title
            if (issue.title.toLowerCase().includes('bug')) {
              await github.rest.issues.addLabels({
                owner: context.repo.owner,
                repo: context.repo.repo,
                issue_number: issue.number,
                labels: ['type:bug']
              });
            }
```

---

## 🎓 Knowledge Check

**Question 1**: What's the difference between issue templates in Markdown and YAML format?
<details>
<summary>Click to see answer</summary>
- **Markdown templates**: Simple, free-form text templates
- **YAML templates**: Structured forms with validation, dropdowns, checkboxes, and required fields
- YAML templates provide better data quality and user experience
</details>

**Question 2**: How do you automate label assignment?
<details>
<summary>Click to see answer</summary>
Several ways:
- Use `.github/labeler.yml` with GitHub Actions
- GitHub Apps like Probot
- Custom workflows with github-script action
- Branch protection rules can require certain labels
</details>

**Question 3**: What should be included in a good release?
<details>
<summary>Click to see answer</summary>
- Clear version number (semantic versioning)
- Comprehensive changelog (features, fixes, changes)
- Installation/upgrade instructions  
- Binary assets (if applicable)
- Contributors acknowledgment
- Links to documentation
</details>

---

## 🎯 Key Takeaways

1. **Templates ensure consistency** across issues and PRs
2. **YAML templates provide better UX** than markdown templates
3. **Label systems should be logical and consistent**
4. **Automate labeling** to reduce manual overhead
5. **Releases should tell a story** about what changed
6. **Semantic versioning** helps users understand impact
7. **Regular release cadence** builds user confidence

---

## ➡️ What's Next?

You now have the skills for effective team workflows and project management! Next, we'll dive into advanced Git techniques and automation.

**Next module**: [Module 4: Advanced Git & GitHub](../Module-4-Advanced/)

---

## 🎮 Practice Exercises

### Exercise 1: Template Setup
1. Create comprehensive issue and PR templates
2. Set up a label system for your project
3. Test templates with sample issues
4. Implement label automation

### Exercise 2: Release Management
1. Create a release with proper changelog
2. Set up automated release workflow
3. Practice semantic versioning
4. Create release templates

### Exercise 3: Organization Setup
1. Create organization-wide templates
2. Set up label standards across repositories
3. Implement cross-repo project management
4. Document your team's processes

---

## 📚 Additional Resources

- [GitHub Templates Documentation](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests)
- [GitHub Labels Guide](https://docs.github.com/en/issues/using-labels-and-milestones-to-track-work)
- [GitHub Releases Documentation](https://docs.github.com/en/repositories/releasing-projects-on-github)
- [Semantic Versioning](https://semver.org/)