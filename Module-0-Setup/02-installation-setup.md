# Installation & Setup Guide

## 🎯 Learning Goals
By the end of this lesson, you'll have:
- Git installed on your computer
- Git configured with your personal information
- A GitHub account set up
- SSH keys configured for secure communication
- Verified everything is working correctly

---

## 📋 Pre-Installation Checklist

Before we begin, make sure you have:
- [ ] Administrator access to your computer
- [ ] A stable internet connection
- [ ] An email address you regularly check
- [ ] 15-20 minutes for the complete setup

---

## 💻 Installing Git

### Windows Installation

#### Option 1: Git for Windows (Recommended)
1. **Download Git**:
   - Go to [git-scm.com](https://git-scm.com)
   - Click "Download for Windows"
   - Run the downloaded `.exe` file

2. **Installation Settings** (Recommended options):
   - ✅ Use Git from Git Bash only
   - ✅ Use the OpenSSL library
   - ✅ Checkout Windows-style, commit Unix-style line endings
   - ✅ Use Windows' default console window

3. **Verify Installation**:
   ```bash
   # Open Git Bash and type:
   git --version
   # Should show something like: git version 2.40.0
   ```

#### Option 2: Using Package Managers
```bash
# Using Chocolatey:
choco install git

# Using Scoop:
scoop install git
```

### macOS Installation

#### Option 1: Using Homebrew (Recommended)
```bash
# Install Homebrew first (if not already installed):
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Then install Git:
brew install git
```

#### Option 2: Download from Website
1. Go to [git-scm.com](https://git-scm.com)
2. Click "Download for Mac"
3. Run the downloaded `.pkg` file
4. Follow the installation wizard

#### Option 3: Using Xcode Command Line Tools
```bash
# This also installs Git:
xcode-select --install
```

### Linux Installation

#### Ubuntu/Debian:
```bash
sudo apt update
sudo apt install git
```

#### CentOS/RHEL/Fedora:
```bash
# CentOS/RHEL:
sudo yum install git

# Fedora:
sudo dnf install git
```

#### Arch Linux:
```bash
sudo pacman -S git
```

#### Verify Installation (All Linux):
```bash
git --version
```

---

## ⚙️ Git Configuration

Now let's configure Git with your personal information:

### 1. Set Your Identity
```bash
# Set your name (use your real name):
git config --global user.name "Your Full Name"

# Set your email (use the same email you'll use for GitHub):
git config --global user.email "your.email@example.com"
```

### 2. Set Default Branch Name
```bash
# Set 'main' as the default branch name:
git config --global init.defaultBranch main
```

### 3. Configure Line Endings
```bash
# Windows:
git config --global core.autocrlf true

# macOS/Linux:
git config --global core.autocrlf input
```

### 4. Set Default Editor (Optional)
```bash
# Use VS Code (if installed):
git config --global core.editor "code --wait"

# Use nano (simple terminal editor):
git config --global core.editor nano

# Use vim (advanced terminal editor):
git config --global core.editor vim
```

### 5. Verify Your Configuration
```bash
# Check all settings:
git config --list

# Check specific settings:
git config user.name
git config user.email
```

---

## 🐙 Creating Your GitHub Account

### 1. Sign Up for GitHub
1. Go to [github.com](https://github.com)
2. Click "Sign up"
3. Enter your details:
   - **Username**: Choose carefully! This will be your GitHub URL
   - **Email**: Use the same email you configured in Git
   - **Password**: Use a strong password

4. **Verify your email address** (check your inbox)

### 2. Choose Your Plan
- **Free plan**: Perfect for learning and most projects
- **Pro plan**: Advanced features for professional development

### 3. Personalize Your Profile
1. Click your profile picture → "Settings"
2. Add a profile picture
3. Add a bio describing yourself
4. Add your location and website (optional)

---

## 🔐 Setting Up SSH Keys

SSH keys provide secure authentication between your computer and GitHub without typing passwords.

### 1. Check for Existing SSH Keys
```bash
# Check if you already have SSH keys:
ls -la ~/.ssh

# Look for files named:
# id_rsa.pub (or id_ed25519.pub)
# id_rsa (or id_ed25519)
```

### 2. Generate New SSH Key (if needed)
```bash
# Generate SSH key (replace with your GitHub email):
ssh-keygen -t ed25519 -C "your.email@example.com"

# When prompted:
# - Press Enter to accept default file location
# - Enter a secure passphrase (optional but recommended)
```

### 3. Add SSH Key to SSH Agent
```bash
# Start the SSH agent:
eval "$(ssh-agent -s)"

# Add your SSH private key:
ssh-add ~/.ssh/id_ed25519
```

### 4. Copy SSH Public Key
```bash
# Copy the public key to clipboard:

# macOS:
pbcopy < ~/.ssh/id_ed25519.pub

# Linux (with xclip):
xclip -selection clipboard < ~/.ssh/id_ed25519.pub

# Windows (Git Bash):
clip < ~/.ssh/id_ed25519.pub

# If above don't work, display and copy manually:
cat ~/.ssh/id_ed25519.pub
```

### 5. Add SSH Key to GitHub
1. Go to GitHub → Settings → SSH and GPG keys
2. Click "New SSH key"
3. Add a descriptive title (e.g., "My Laptop")
4. Paste your public key
5. Click "Add SSH key"

### 6. Test SSH Connection
```bash
# Test connection to GitHub:
ssh -T git@github.com

# You should see a message like:
# "Hi username! You've successfully authenticated, but GitHub does not provide shell access."
```

---

## ✅ Verification Tests

Let's make sure everything is working correctly:

### 1. Git Installation Test
```bash
git --version
```
**Expected**: Version number (e.g., `git version 2.40.0`)

### 2. Git Configuration Test
```bash
git config --global --list
```
**Expected**: Your name, email, and other settings

### 3. GitHub SSH Test
```bash
ssh -T git@github.com
```
**Expected**: Success message with your GitHub username

### 4. Complete Workflow Test
Let's create a test repository to verify everything works:

```bash
# 1. Create a test directory:
mkdir git-test
cd git-test

# 2. Initialize Git repository:
git init

# 3. Create a test file:
echo "Hello Git!" > test.txt

# 4. Add and commit the file:
git add test.txt
git commit -m "Initial commit"

# 5. Check the log:
git log --oneline
```

**Expected**: You should see your commit in the log

---

## 🔧 Troubleshooting Common Issues

### Git Command Not Found
```bash
# Error: git: command not found

# Solutions:
# 1. Restart your terminal/command prompt
# 2. Check if Git is in your PATH
echo $PATH

# 3. Reinstall Git
```

### SSH Key Issues
```bash
# Error: Permission denied (publickey)

# Solutions:
# 1. Make sure you copied the PUBLIC key (.pub file)
# 2. Verify SSH agent is running:
ssh-add -l

# 3. Re-add your key:
ssh-add ~/.ssh/id_ed25519
```

### Configuration Issues
```bash
# Check current configuration:
git config --global --list

# Fix common issues:
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

### Line Ending Issues (Windows)
```bash
# Warning: LF will be replaced by CRLF
git config --global core.autocrlf true
```

---

## 🎯 Quick Setup Script

For experienced users, here's a quick setup script:

```bash
#!/bin/bash
# Quick Git setup script

echo "Setting up Git configuration..."

# Set user information
read -p "Enter your full name: " fullname
read -p "Enter your email: " email

git config --global user.name "$fullname"
git config --global user.email "$email"
git config --global init.defaultBranch main
git config --global core.autocrlf input  # Use 'true' for Windows

echo "Git configuration complete!"
git config --global --list
```

---

## 📱 Optional: GUI Tools

While this guide focuses on command-line Git, you might also want these visual tools:

### GitHub Desktop
- **Purpose**: Official GitHub GUI application
- **Download**: [desktop.github.com](https://desktop.github.com)
- **Best for**: Beginners who prefer visual interfaces

### VS Code Git Integration
- **Purpose**: Built-in Git features in Visual Studio Code
- **Setup**: Install VS Code, Git integration is built-in
- **Best for**: Developers already using VS Code

### GitKraken
- **Purpose**: Professional Git GUI with visual commit graph
- **Download**: [gitkraken.com](https://gitkraken.com)
- **Best for**: Complex branching workflows

---

## 🎓 Knowledge Check

Test your setup by answering these questions:

**Question 1**: What command shows your current Git configuration?
<details>
<summary>Click to see answer</summary>
`git config --global --list`
</details>

**Question 2**: What's the difference between your SSH public and private key?
<details>
<summary>Click to see answer</summary>
- **Public key** (.pub file): Safe to share, goes on GitHub
- **Private key**: Keep secret, stays on your computer
</details>

**Question 3**: How do you test if your SSH connection to GitHub works?
<details>
<summary>Click to see answer</summary>
`ssh -T git@github.com`
</details>

---

## 🎯 Key Takeaways

1. **Git is now installed** and configured with your personal information
2. **GitHub account is ready** for hosting your repositories
3. **SSH keys provide secure** password-free authentication
4. **Everything is tested** and working correctly
5. **You're ready** to start using Git and GitHub!

---

## ➡️ What's Next?

Now that Git and GitHub are set up, let's create your first repository and learn the basic workflow!

**Next lesson**: [Your First Repository Workflow](03-first-repo-workflow.md)

---

## 📚 Additional Resources

- [Git Official Documentation](https://git-scm.com/doc)
- [GitHub SSH Key Guide](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)
- [Git Configuration Guide](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)
- [GitHub Desktop Tutorial](https://docs.github.com/en/desktop)