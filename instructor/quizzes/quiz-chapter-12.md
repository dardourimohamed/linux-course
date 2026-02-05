# Chapter 12 Quiz: Git Version Control

## Quiz Questions

### Multiple Choice

**1. Who created Git?**
- A) Richard Stallman
- B) Linus Torvalds
- C) Bill Gates
- D) Ken Thompson

**Answer:** B

**2. What is the Git command to stage files for commit?**
- A) git add
- B) git commit
- C) git stage
- D) git save

**Answer:** A

**3. What does HEAD refer to in Git?**
- A) The first commit
- B) The current branch tip
- C) The repository root
- D) The main branch

**Answer:** B

**4. Which command creates a new branch?**
- A) git branch branchname
- B) git checkout -b branchname
- C) Both A and B
- D) git new branch branchname

**Answer:** C

**5. What does `git status` show?**
- A) Commit history
- B) Current branch and working tree status
- C) Remote repositories
- D) Branch names only

**Answer:** B

### True/False

**6. True or False: Git tracks changes to files, not just file contents.**

**Answer:** True

**7. True or False: git push sends your commits to a remote repository.**

**Answer:** True

**8. True or False: The staging area is where files go before being committed.**

**Answer:** True

**9. True or False: git pull is equivalent to git fetch followed by git merge.**

**Answer:** True

**10. True or False: Merge conflicts are always automatic and never require manual resolution.**

**Answer:** False

### Short Answer

**11. What are the three main areas in Git's workflow (working directory, staging area, repository) and what happens in each?**

**Answer:**
1. **Working Directory**: Your actual files on disk where you make changes. Git detects modifications here.

2. **Staging Area (Index)**: Files prepared for the next commit. You choose which changes to include with `git add`.

3. **Repository (Local)**: Where Git stores committed history and metadata. Committed snapshots are here.

**Workflow**: Modify files in working directory → Stage with `git add` → Commit with `git commit` → Push with `git push`

**12. Explain the difference between `git merge` and `git rebase`.**

**Answer:**
**`git merge`**:
- Creates a merge commit combining two branches
- Preserves complete history including all branches
- Non-destructive - original branches unchanged
- Can create merge commits in history
- Safer for shared branches

**`git rebase`**:
- Replays commits from one branch onto another
- Creates linear history (no merge commits)
- Rewrites history - destructive if pushed
- Cleaner history but can cause issues if shared
- Generally used for local cleanup before pushing

**Rule of thumb**: Use merge for shared branches, rebase only for local cleanup or before integrating shared work.

**13. What is a merge conflict and how do you resolve it?**

**Answer:**
A **merge conflict** occurs when Git can't automatically reconcile changes between branches - typically when two branches modified the same lines differently.

**What it looks like:**
```
<<<<<<< HEAD
print("Version A")
=======
print("Version B")
>>>>>>> feature-branch
```

**Resolution steps:**
1. Open the conflicting file(s)
2. Find the conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
3. Edit the file to keep the correct version (or combine both)
4. Remove the conflict markers
5. `git add` the resolved file
6. `git commit` to complete the merge

**Tools**: Git provides merge tools (`git mergetool`) and editors have conflict resolution helpers.

### Discussion Question

**14. Version control is one of the most important tools in modern software development. Explain why Git is considered superior to older centralized systems like SVN, and discuss how branching in Git enables workflows that would be difficult or impossible with centralized version control. Include examples of how Git improves collaboration and experimentation.**

**Sample Answer:**

**Git vs. Centralized VCS (SVN, CVS):**

**Distributed Architecture:**
- **Git**: Every developer has complete repository history. Can work offline, commit locally, no single point of failure.
- **SVN**: Single central server. Network required for most operations. Server failure stops all work.

**Branching and Merging:**
- **Git**: Branches are cheap and fast. Merging is easy and frequent. Branches are first-class objects.
- **SVN**: Branches are expensive (copy entire directory). Merging is difficult and error-prone. Avoided when possible.

**Performance:**
- **Git**: Most operations are local and instant. History browsing, commits, diffs are fast.
- **SVN**: Must contact server for most operations. Slow on large repositories.

**Workflows Enabled by Git Branching:**

**1. Feature Branch Workflow:**
```bash
git checkout -b feature/new-ui
# Make changes, commit
git checkout main
git merge feature/new-ui
```
Each feature gets its own branch. Clean history, isolated development.

**2. Experimentation Without Risk:**
```bash
git checkout -b experiment/bold-refactor
# Try something radical
# If it fails: git checkout main && git branch -D experiment/bold-refactor
# If it works: git merge experiment/bold-refactor
```
Zero-cost experiments encourage innovation.

**3. Parallel Development:**
- Multiple developers work on features simultaneously
- No blocking - merge when ready
- Test features independently before integration

**4. Code Review with Pull Requests:**
- Push feature branch to remote
- Create pull request for review
- Discuss and refine code in isolation
- Merge when approved

**5. Release Branching:**
```bash
git checkout -b release/v1.0
# Stabilize for release
git checkout main
# Continue development on main
```
Development continues while release is prepared.

**6. Hotfix Branches:**
```bash
git checkout -b hotfix/critical-bug
# Fix, deploy, merge to main and release
```
Quick fixes without disrupting main development.

**Collaboration Improvements:**

**Merge Requests/Pull Requests:**
- Code review before integration
- Discussion of implementation approach
- CI/CD testing on each branch
- Clear history of who did what and why

**Forks and Contributions:**
- Fork project, make changes, propose merge
- Maintain your own version
- Contribute back to upstream

**Conflict Resolution:**
- Git handles conflicts gracefully
- Tools for visual comparison
- Preserve both versions if needed
- Learn from conflicts

**Experimentation Culture:**
- Branches enable "try everything" approach
- No fear of breaking main
- A/B test different implementations
- Learn from failed experiments

**Conclusion**: Git's distributed nature and cheap branching fundamentally changed how software is developed. It enables modern workflows (feature branches, pull requests, forking) that would be impractical with centralized systems. The ability to branch and merge easily encourages experimentation, parallel development, and better collaboration.

## Answer Key

| Question | Answer | Points |
|----------|--------|--------|
| 1 | B | 1 |
| 2 | A | 1 |
| 3 | B | 1 |
| 4 | C | 1 |
| 5 | B | 1 |
| 6 | True | 1 |
| 7 | True | 1 |
| 8 | True | 1 |
| 9 | True | 1 |
| 10 | False | 1 |
| 11 | Three areas explained | 4 |
| 12 | Both compared | 3 |
| 13 | Conflict explained + resolution | 3 |
| 14 | Git advantages + workflows | 5 |

**Total Points:** 27

**Passing Score:** 18/27 (67%)

---

## Instructor Notes

### Common Misconceptions to Address

1. **"Git is too complex"** - Start with basic commands (add, commit, push). Advanced features can wait.

2. **"I don't need branches for personal projects"** - Branches are useful even solo: experimental features, cleanup, stable releases.

3. **"Merge conflicts are scary"** - They're normal and usually straightforward to resolve. Tools help.

4. **"git pull is always safe"** - Emphasize understanding what pull does (fetch + merge). Can create merge commits.

### Teaching Tips

- **Live Git demo**: Create a repo, make commits, branch, merge during class
- **Visual Git tools**: Show git log --graph to visualize branches
- **Conflict demo**: Intentionally create a conflict and resolve it together
- **GitHub/GitLab**: Show how remote repositories enhance Git's power
- **.gitignore**: Explain what files should never be committed

### Extension Activities

- Git flow simulation: Practice feature branch workflow
- Merge conflict challenge: Resolve increasingly complex conflicts
- Interactive rebase practice: Clean up commit history
- Create a .gitignore for a specific project type
- Fork and contribute to an open-source project
- Compare Git hosting platforms (GitHub, GitLab, Bitbucket)
