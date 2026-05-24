# Git Commands Reference Guide

A comprehensive guide to essential Git commands covering installation, configuration, staging, committing, and branch management.

---

## Table of Contents

1. [Installation & GUIs](#installation--guis)
2. [Setup & Configuration](#setup--configuration)
3. [Repository Initialization](#repository-initialization)
4. [Stage & Snapshot](#stage--snapshot)
5. [Branch & Merge](#branch--merge)
6. [Common Workflows](#common-workflows)
7. [Tips & Best Practices](#tips--best-practices)

---

## Installation & GUIs

### Overview

Git provides command-line tools across multiple platforms with optional graphical user interfaces (GUIs) for enhanced user experience. These tools offer both the power of the command line and the convenience of visual interfaces for day-to-day interactions, reviews, and repository synchronization.

### Installation Options

#### GitHub for Windows

**URL:** [https://windows.github.com](https://windows.github.com)

- Platform-specific installer for Windows systems
- Includes Git command-line tool
- Provides GitHub Desktop GUI application
- Integrates with Windows Explorer context menus
- Automatic updates for latest features and security patches

#### GitHub for Mac

**URL:** [https://mac.github.com](https://mac.github.com)

- Native macOS application and command-line tools
- Optimized for macOS workflows and native integrations
- Includes GitHub Desktop GUI for macOS
- Support for Touch Bar on compatible MacBooks
- Seamless integration with macOS system preferences

#### Git for All Platforms

**URL:** [http://git-scm.com](http://git-scm.com)

- Official Git distribution repository
- Supports Linux, Solaris, macOS, and Windows
- Latest command-line releases and updates
- Comprehensive documentation and community support
- Source code available for custom builds

---

## Setup & Configuration

### Overview

Git configuration sets up user identity and preferences that apply across all local repositories. This information is crucial for version history tracking, collaboration, and audit trails.

### Set User Name (Global)

```bash
git config --global user.name "[firstname lastname]"
```

**Purpose:** Establish your identity for commit attribution

**Details:**
- Sets the name displayed in commit history
- Used for version control and code review credit
- Applied to all repositories on your machine
- Can be overridden per-repository using `--local` flag instead of `--global`

**Example:**
```bash
git config --global user.name "John Developer"
```

### Set User Email (Global)

```bash
git config --global user.email "[valid-email]"
```

**Purpose:** Associate an email address with your commits

**Details:**
- Email is displayed in commit history and version logs
- Used for notifications and user identification
- Should match your GitHub email for account linking
- Apply `--local` to override for specific repositories

**Example:**
```bash
git config --global user.email "john.developer@example.com"
```

### Enable Automatic Command-Line Coloring

```bash
git config --global color.ui auto
```

**Purpose:** Enable color syntax highlighting in Git output

**Details:**
- Makes Git command output easier to read
- Uses platform-appropriate colors for different output types
- Helps distinguish between branches, status, and changes
- Improves visibility in terminal or command prompt

**Benefits:**
- Branch names appear in distinct colors
- Additions and deletions are color-coded
- Status messages are more visually organized
- Reduces eye strain during extensive terminal use

---

## Repository Initialization

### Overview

Initialize new Git repositories or clone existing ones to begin version control.

### Initialize a New Repository

```bash
git init
```

**Purpose:** Convert an existing directory into a Git repository

**Details:**
- Creates a `.git` directory with version control infrastructure
- Initializes Git configuration and tracking system
- Enables version control for files in current directory and subdirectories
- Safe to run in any existing project directory

**When to Use:**
- Starting a new project from scratch
- Converting an existing project to version control
- Setting up a local repository before pushing to GitHub

**Example:**
```bash
cd my-project
git init
```

### Clone an Existing Repository

```bash
git clone [url]
```

**Purpose:** Create a complete copy of a remote repository locally

**Details:**
- Downloads entire repository history from hosted location
- Creates new directory with repository name (unless specified)
- Automatically sets up remote tracking for the origin
- Includes all branches and complete commit history

**What Gets Copied:**
- All project files and subdirectories
- Complete version history
- All commits from repository creation
- Remote references and branch information

**Example:**
```bash
git clone https://github.com/username/repository.git
git clone https://github.com/username/repository.git my-local-copy
```

---

## Stage & Snapshot

### Overview

The staging area is where you prepare changes before committing them. This workflow helps organize and review changes before they become permanent history.

### Check Status

```bash
git status
```

**Purpose:** Display current repository state

**Information Provided:**
- Modified files not yet staged
- Staged files ready for commit
- Untracked files (not yet added to version control)
- Current branch name
- Position relative to remote tracking branch

**Output Example:**
```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to stage changes)
        modified:   file.txt
        modified:   src/main.py

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        config.json
        test.py

nothing added to commit but untracked files present (use "git add" to track)
```

### Stage Files (Add to Index)

```bash
git add [file]
```

**Purpose:** Add files to staging area for next commit

**Details:**
- Prepares file changes for inclusion in commit
- Takes current file state as it appears in working directory
- Can stage individual files or multiple files
- Staged changes appear in commit history snapshot

**Common Usage Patterns:**

```bash
# Stage a single file
git add config.json

# Stage multiple specific files
git add file1.txt file2.txt

# Stage all changes in current directory
git add .

# Stage all changes in entire repository
git add -A

# Interactively choose changes to stage (requires user confirmation)
git add -p
```

### Unstage Files (Remove from Index)

```bash
git reset [file]
```

**Purpose:** Remove files from staging area while keeping changes

**Details:**
- Reverts file to unstaged status
- Preserves modifications in working directory
- Useful for removing accidentally staged changes
- Allows selective commit of some changes while keeping others uncommitted

**Scenario:**
```bash
# Accidentally staged a configuration file
git add sensitive-config.ini
git add main.py

# Realize you shouldn't commit the config file yet
git reset sensitive-config.ini

# Now only main.py will be committed
git commit -m "Update main function"
```

### View Unstaged Changes

```bash
git diff
```

**Purpose:** Show differences between working directory and last commit

**Details:**
- Displays all modifications not yet staged
- Shows line-by-line changes for each file
- Helps review work before staging
- Uses standard diff format (+ for additions, - for deletions)

**Output Format:**
```
diff --git a/file.txt b/file.txt
index abc1234..def5678 100644
--- a/file.txt
+++ b/file.txt
@@ -10,3 +10,4 @@
 existing line
-removed line
+added line
 another line
```

### View Staged Changes

```bash
git diff --staged
```

**Purpose:** Show differences between staged changes and last commit

**Details:**
- Displays all changes currently in staging area
- Shows exactly what will be included in next commit
- Helps verify staged changes before committing
- Same diff format as `git diff`

**Workflow:**
```bash
# Make changes
git status  # See what changed

git add file.txt  # Stage file

git diff --staged  # Review what will be committed

git commit -m "message"  # Make it permanent
```

### Commit Staged Changes

```bash
git commit -m "[descriptive message]"
```

**Purpose:** Create a permanent snapshot of staged changes

**Details:**
- Saves staged changes to repository history
- Creates commit object with author, timestamp, and message
- Message should be concise but descriptive
- Commits include author identity from configuration
- Becomes part of permanent project history

**Commit Message Best Practices:**

Good commit messages:
- Start with action verb (Add, Fix, Update, Remove, Refactor)
- Keep subject line under 50 characters
- Provide context for why change was made
- Use imperative tone (e.g., "Add feature" not "Added feature")

**Examples:**

```bash
# Simple, clear message
git commit -m "Fix login button alignment"

# More detailed message with explanation
git commit -m "Add user authentication

- Implement JWT token validation
- Add password hashing with bcrypt
- Create login endpoint"

# Quick fix message
git commit -m "Update README with setup instructions"
```

---

## Branch & Merge

### Overview

Branches enable parallel development by creating isolated environments for features, fixes, and experiments. This section covers branch management and integration workflows.

### List All Branches

```bash
git branch
```

**Purpose:** Display available branches in repository

**Details:**
- Shows all local branches
- Asterisk (*) indicates currently active branch
- Helps navigate multiple feature branches
- Doesn't show remote branches by default

**Output Example:**
```
  feature/authentication
  feature/payment-integration
* main
  hotfix/security-patch
  develop
```

**Useful Variations:**

```bash
# Show remote branches too
git branch -a

# Show branches with last commit message
git branch -v

# Show merged branches
git branch --merged

# Show unmerged branches
git branch --no-merged
```

### Create a New Branch

```bash
git branch [branch-name]
```

**Purpose:** Create new branch for isolated development

**Details:**
- New branch starts at current commit
- Doesn't automatically switch to new branch
- Allows parallel feature development
- Branch name should be descriptive (feature/name or bugfix/name)

**Branch Naming Conventions:**

```bash
# Feature branches
git branch feature/user-dashboard
git branch feature/payment-gateway

# Bug fix branches
git branch bugfix/login-error
git branch bugfix/database-query

# Release branches
git branch release/v1.2.0

# Hotfix branches
git branch hotfix/critical-security
```

### Switch to a Different Branch

```bash
git checkout [branch-name]
```

**Purpose:** Change active branch and update working directory

**Details:**
- Loads files from specified branch
- Updates repository state to match branch
- Uncommitted changes must be dealt with first
- Changes working directory to reflect branch content

**Safety Considerations:**

```bash
# Check status before switching
git status

# Stash uncommitted changes if needed
git stash
git checkout feature-branch
git stash pop

# Switch and create branch in one command
git checkout -b new-branch-name
```

### Merge Branch History

```bash
git merge [branch]
```

**Purpose:** Integrate changes from one branch into current branch

**Details:**
- Combines commit history from specified branch
- Updates current branch with all changes from source branch
- Creates merge commit if combining different histories
- Useful for integrating feature work into main branches

**Common Merge Scenarios:**

```bash
# Update main with completed feature
git checkout main
git merge feature/authentication

# Integrate develop into main for release
git checkout main
git merge develop

# Merge bugfix into main and develop
git checkout main
git merge bugfix/critical-issue
git checkout develop
git merge bugfix/critical-issue
```

**Merge Conflict Resolution:**

When automatic merge fails:

```bash
# Review conflicts in files
git status

# After resolving conflicts manually, stage files
git add resolved-file.txt

# Complete the merge
git commit -m "Merge feature/branch with conflict resolution"
```

### View Commit History

```bash
git log
```

**Purpose:** Display commit history for current branch

**Details:**
- Shows all commits on current branch
- Includes author, date, and commit message
- Most recent commits appear first
- Shows complete ancestry from branch creation

**Output Example:**
```
commit 5a7b3c2d1e0f9g8h7i6j5k4l3m2n1o0p (HEAD -> main)
Author: John Developer <john@example.com>
Date:   Wed May 24 12:00:00 2024 +0000

    Merge feature/authentication into main

commit 8h7i6j5k4l3m2n1o0p9q8r7s6t5u4v3w
Author: Jane Contributor <jane@example.com>
Date:   Wed May 24 10:30:00 2024 +0000

    Add user authentication system

commit 1o0p9q8r7s6t5u4v3w2x1y0z9a8b7c6d
Author: John Developer <john@example.com>
Date:   Tue May 23 15:45:00 2024 +0000

    Initial project setup
```

**Useful Log Variations:**

```bash
# Show last 5 commits
git log -5

# Show one-line summary
git log --oneline

# Show commits with file changes
git log --stat

# Show commits with actual changes
git log -p

# Show commits from specific author
git log --author="John Developer"

# Show commits since specific date
git log --since="2 weeks ago"

# Show visual branch history
git log --graph --oneline --all
```

---

## Common Workflows

### Feature Branch Workflow

A typical workflow for developing a new feature:

```bash
# 1. Start on main branch
git checkout main

# 2. Update to latest remote version
git pull origin main

# 3. Create feature branch
git checkout -b feature/new-dashboard

# 4. Make changes and commit
echo "new feature code" > feature.js
git add feature.js
git commit -m "Add dashboard component"

# 5. Make more changes
echo "more code" >> feature.js
git add feature.js
git commit -m "Add dashboard styles"

# 6. Switch back to main
git checkout main

# 7. Merge feature when ready
git merge feature/new-dashboard

# 8. Clean up feature branch
git branch -d feature/new-dashboard
```

### Bug Fix Workflow

Managing and merging bug fixes:

```bash
# 1. Create bugfix branch from main
git checkout main
git checkout -b bugfix/login-timeout

# 2. Fix the issue
# Edit files...
git add fixed-file.js
git commit -m "Fix login session timeout issue"

# 3. Thoroughly test before merging
# Run tests...

# 4. Merge to main
git checkout main
git merge bugfix/login-timeout

# 5. Also merge to develop if applicable
git checkout develop
git merge bugfix/login-timeout
```

### Release Branch Workflow

Preparing a release version:

```bash
# 1. Create release branch from develop
git checkout develop
git checkout -b release/v1.2.0

# 2. Update version numbers
# Edit package.json, version files, etc.
git add package.json
git commit -m "Bump version to 1.2.0"

# 3. Make release-specific fixes only
# Fix bugs found during QA...
git commit -m "Fix critical bug found in release testing"

# 4. Merge to main with tag
git checkout main
git merge release/v1.2.0
git tag -a v1.2.0 -m "Release version 1.2.0"

# 5. Merge back to develop
git checkout develop
git merge release/v1.2.0

# 6. Delete release branch
git branch -d release/v1.2.0
```

---

## Tips & Best Practices

### Commit Frequently

- Make small, logical commits
- Each commit should represent one logical change
- Easier to revert or cherry-pick individual commits
- Cleaner history for code review

```bash
# Good: Multiple focused commits
git commit -m "Add user registration form"
git commit -m "Add form validation logic"
git commit -m "Add success notification"

# Avoid: One giant commit
git commit -m "Add user registration form, validation, and notifications"
```

### Write Clear Commit Messages

- Describe *what* changed and *why*
- Use present tense imperative ("Add" not "Added")
- First line under 50 characters
- Additional context in body if needed

```bash
# Good commit message
git commit -m "Fix race condition in data synchronization

- Prevent concurrent updates to same record
- Add mutex lock for critical section
- Reduces data corruption incidents by 99%"

# Avoid
git commit -m "stuff" # Too vague
git commit -m "Added and fixed various things" # Not specific
```

### Use Branches for Organization

- Create branches for features, bugs, and releases
- Keep main branch stable and deployable
- Delete merged branches to reduce clutter
- Use consistent naming conventions

```bash
# Well-organized branches
feature/user-authentication
feature/payment-integration
bugfix/database-connection
hotfix/security-vulnerability
release/v2.0.0

# Avoid
test
new-stuff
trying-this
john-branch
```

### Review Before Committing

- Always use `git status` before committing
- Review changes with `git diff` and `git diff --staged`
- Ensure you're on the correct branch
- Verify commit message is clear and accurate

```bash
# Complete workflow
git status
git add specific-file.js
git diff --staged
git commit -m "Add feature X"
```

### Regular Synchronization

- Pull changes regularly from remote repository
- Push work to remote frequently for backup
- Keep branches up-to-date with main
- Resolve conflicts early and often

```bash
# Daily synchronization
git pull origin main
# ... do work ...
git push origin feature/my-feature

# Keep feature branch updated
git checkout feature/my-branch
git pull origin main
git push origin feature/my-branch
```

### Meaningful Branch Names

- Use forward slashes for grouping
- Be descriptive about the work
- Avoid generic or cryptic names

```bash
# Good branch names
feature/oauth-integration
bugfix/mobile-responsive-layout
hotfix/payment-processing-error
release/v1.5.0

# Avoid
feature1
fix
testing
temp
work-in-progress
```

---

## Additional Resources

- **Official Git Documentation:** [https://git-scm.com/doc](https://git-scm.com/doc)
- **GitHub Help:** [https://help.github.com](https://help.github.com)
- **Git Cheat Sheet:** [https://github.github.com/training-kit/](https://github.github.com/training-kit/)
- **Understanding Git:** [https://git-scm.com/book/en/v2](https://git-scm.com/book/en/v2)

---

## Troubleshooting Common Issues

### Accidentally Committed to Wrong Branch

```bash
# 1. Create correct branch from current position
git branch correct-branch

# 2. Reset current branch to before the commit
git reset --hard HEAD~1

# 3. Switch to correct branch
git checkout correct-branch
```

### Uncommitted Changes Blocking Switch

```bash
# Option 1: Stash changes temporarily
git stash
git checkout other-branch
git checkout previous-branch
git stash pop

# Option 2: Commit changes first
git add .
git commit -m "WIP: work in progress"
git checkout other-branch
```

### Undoing the Last Commit

```bash
# Keep changes in working directory
git reset --soft HEAD~1

# Discard changes completely
git reset --hard HEAD~1

# Keep changes staged
git reset --mixed HEAD~1
```

---

**Last Updated:** May 24, 2024  
**Version:** 1.0  
**Maintained By:** Development Team

For questions or updates, please contact your project maintainer or team lead.