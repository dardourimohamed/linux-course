# Chapter 12 Solutions: Git Version Control

## Exercise Solutions

### Exercise 1: Your First Repository

**Task:** Create a Git repository and make commits.

**Solution:**

```bash
# Create a directory
mkdir git-practice
cd git-practice

# Initialize Git repository
git init
```

**Expected Output:**
```
Initialized empty Git repository in /home/user/git-practice/.git/
```

```bash
# Create notes.md with content
cat > notes.md << 'EOF'
# Linux Course Notes

## Chapter 1: Introduction
- Linux is an open-source operating system
- Created by Linus Torvalds in 1991
- Used on servers, desktops, and embedded devices

## Chapter 2: Installation
- Can be installed alongside Windows (dual-boot)
- Also works in virtual machines
- Important to verify ISO checksums
EOF
```

```bash
# Check status
git status
```

**Expected Output:**
```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        notes.md

nothing added to commit but untracked files present
```

```bash
# Stage and commit
git add notes.md
git commit -m "Initial commit: Add course notes"
```

**Expected Output:**
```
[main (root-commit)] Initial commit: Add course notes
 1 file changed, 11 insertions(+)
 create mode 100644 notes.md
```

```bash
# Add more content
cat >> notes.md << 'EOF'

## Chapter 3: GNOME Desktop
- Activities overview: Super key
- Workspaces: organize tasks
- Extensions: customize GNOME
EOF
```

```bash
# Stage and commit again
git add notes.md
git commit -m "Add Chapter 3 notes"
```

```bash
# View commit history
git log --oneline
```

**Expected Output:**
```
a1b2c3d Add Chapter 3 notes
7f6e5d4 Initial commit: Add course notes
```

**Explanation:**
- `git init`: Create new Git repository
- `git status`: See working tree status
- `git add`: Stage changes for commit
- `git commit`: Create commit with staged changes
- `git log`: Show commit history

---

### Exercise 2: Branching Practice

**Task:** Create, merge, and delete branches.

**Solution:**

```bash
# Ensure we're in git-practice directory
cd ~/git-practice

# Create a new branch
git branch experiment
```

```bash
# On the experiment branch, create a file
git checkout experiment
touch test.txt
echo "Experiment file" > test.txt
```

```bash
# Commit the file
git add test.txt
git commit -m "Add test file"
```

**Expected Output:**
```
[experiment c3d4e5f] Add test file
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```

```bash
# Switch back to main
git checkout main
```

**Expected Output:**
```
Switched to branch 'main'
```

```bash
# Merge experiment into main
git merge experiment
```

**Expected Output:**
```
Updating a1b2c3d..c3d4e5f
Fast-forward
 test.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```

```bash
# Delete experiment branch
git branch -d experiment
```

**Expected Output:**
```
Deleted branch experiment (was c3d4e5f).
```

```bash
# Verify file exists on main
ls -la
```

**Expected Output:**
```
total 8
drwxr-xr-x  3 user user 4096 Feb  7 10:30 .
drwxr-xr-x 20 user user 4096 Feb  7 10:00 ..
drwxr-xr-x  8 user user 4096 Feb  7 10:30 .git
-rw-r--r--  1 user user  250 Feb  7 10:20 notes.md
-rw-r--r--  1 user user   17 Feb  7 10:30 test.txt
```

```bash
# Verify branches
git branch
```

**Expected Output:**
```
* main
```

**Explanation:**
- `git branch name`: Create new branch
- `git checkout name`: Switch to branch
- `git checkout -b name`: Create and switch in one command
- `git merge name`: Merge branch into current branch
- `git branch -d name`: Delete merged branch

---

### Exercise 3: Undo Mistakes

**Task:** Practice Git's undo capabilities.

**Solution:**

```bash
# Create and commit a file
echo "Version 1" > file.txt
git add file.txt
git commit -m "Add file v1"
```

```bash
# Modify and commit again
echo "Version 2" >> file.txt
git add file.txt
git commit -m "Update to v2"
```

```bash
# Undo last commit (keep changes staged)
git reset --soft HEAD~1
```

```bash
# Check status
git status
```

**Expected Output:**
```
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   file.txt
```

```bash
# Unstage changes
git reset HEAD file.txt
```

**Expected Output:**
```
Unstaged changes after reset:
M  file.txt
```

```bash
# Discard working directory changes
git checkout -- file.txt
cat file.txt
```

**Expected Output:**
```
Version 1
```

```bash
# Verify history
git log --oneline
```

**Expected Output:**
```
c3d4e5f Add file v1
7f6e5d4 Initial commit: Add course notes
```

**Explanation of reset modes:**
- `--soft`: Undo commit, keep changes staged
- `--mixed` (default): Undo commit and unstage changes
- `--hard`: Undo commit, discard all changes (dangerous!)
- `git checkout -- file`: Discard working directory changes

---

### Exercise 4: Merge Conflict Resolution

**Task:** Create and resolve a merge conflict.

**Solution:**

```bash
# Create conflict-test branch
git checkout -b conflict-test
```

```bash
# On main, create conflict.md with Version A
echo "Version A" > conflict.md
git add conflict.md
git commit -m "Add conflict.md with Version A"
```

```bash
# Switch to conflict-test branch
git checkout conflict-test
```

```bash
# Modify same line to Version B and commit
echo "Version B" > conflict.md
git add conflict.md
git commit -m "Change to Version B"
```

```bash
# Switch back to main
git checkout main
```

```bash
# Change same line to Version C
echo "Version C" > conflict.md
git add conflict.md
git commit -m "Change to Version C"
```

```bash
# Merge conflict-test into main
git merge conflict-test
```

**Expected Output:**
```
Auto-merging conflict.md
CONFLICT (content): Merge conflict in conflict.md
Automatic merge failed; fix conflicts and then commit the result.
```

```bash
# View conflict markers
cat conflict.md
```

**Expected Output:**
```
<<<<<<< HEAD
Version C
=======
Version B
>>>>>>> conflict-test
```

```bash
# Resolve conflict (choose Version B)
echo "Version B" > conflict.md
git add conflict.md
```

```bash
# Complete the merge
git commit -m "Resolve conflict: choose Version B"
```

**Expected Output:**
```
[main abc1234] Resolve conflict: choose Version B
```

```bash
# Clean up
git branch -d conflict-test
```

**Explanation:**
- Conflicts occur when same lines are modified differently
- Edit file to choose or combine versions
- Remove conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
- `git add` marks conflict as resolved
- Complete with `git commit`

---

### Exercise 5: Remote Collaboration

**Task:** Push to remote repository (if available).

**Solution:**

```bash
# Create a repository on GitHub/GitLab first
# Then add remote

git remote add origin https://github.com/username/repo.git
```

```bash
# View remotes
git remote -v
```

**Expected Output:**
```
origin  https://github.com/username/repo.git (fetch)
origin  https://github.com/username/repo.git (push)
```

```bash
# Push local commits
git push -u origin main
```

**Expected Output:**
```
Username for 'https://github.com': username
Password:
Enumerating objects: 9, done.
...
To https://github.com/username/repo.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

```bash
# Make a change on web interface (or push from another machine)
# Then pull changes

git pull origin main
```

```bash
# Verify synchronization
git log --oneline
```

**Explanation:**
- `git remote add`: Add remote repository URL
- `git push`: Upload local commits to remote
- `git pull`: Download and merge remote changes
- `-u` flag sets upstream relationship for future pushes

---

## Common Git Workflows

### Feature Branch Workflow

```bash
# Start from main
git checkout main
git pull origin main

# Create feature branch
git checkout -b feature-new-ui

# Make changes and commit
git add .
git commit -m "Add new UI design"

# Push feature branch
git push -u origin feature-new-ui

# Create pull request on GitHub/GitLab
# After review and merge, delete branch
git checkout main
git pull origin main
git branch -d feature-new-ui
```

### Commit Message Best Practices

```bash
# Good commit messages
git commit -m "Add user authentication"
git commit -m "Fix memory leak in image parser"
git commit -m "Update dependencies to latest versions"
git commit -m "Refactor: extract validation logic to separate module"

# Bad commit messages
git commit -m "update"
git commit -m "fixes"
git commit -m "stuff"
```

---

## .gitignore Examples

```bash
# Create .gitignore for project
cat > .gitignore << 'EOF'
# Python
__pycache__/
*.py[cod]
*.so
.Python
venv/
env/

# Node
node_modules/
npm-debug.log
package-lock.json

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Project specific
*.log
.env
config.local.*
EOF
```

```bash
# Stage and commit .gitignore
git add .gitignore
git commit -m "Add .gitignore for Python and Node projects"
```

---

## Instructor Notes

### Teaching Tips

1. **Live Git demo**: Create a repo during class showing each command
2. **Visual Git tools**: Show `git log --graph` to visualize branches
3. **Conflict demo**: Intentionally create conflict and resolve together
4. **GitHub/GitLab**: Show how remote repos enhance Git
5. **.gitignore**: Explain what files should never be committed

### Common Student Issues

| Issue | Solution |
|-------|----------|
| Commits missing | Forgot to `git add` first |
| Can't push | Need to `git pull` first (diverged history) |
| Merge conflict scary | Use git merge tool or edit file directly |
| Wrong commit message | `git commit --amend` (only if not pushed) |
| Accidentally deleted branch | `git reflog` to find lost commits |

### Extension Activities

- Git flow simulation: Practice feature branch workflow
- Interactive rebase practice: Clean up commit history
- Compare Git hosting platforms (GitHub, GitLab, Bitbucket)
- Fork and contribute to an open-source project
- Set up pre-commit hooks for validation
- Create a .gitignore for a specific project type
- Research and practice Git submodules
