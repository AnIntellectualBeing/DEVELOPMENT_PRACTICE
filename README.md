# 🔄 Complete Git Workflow + Cheat Sheet


---

## 📋 The Complete Feature Lifecycle (Visual)

```
START HERE
    │
    ▼
┌─────────────────────┐
│ 1. Update develop   │  git checkout develop
│    (get latest)     │  git pull
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ 2. Create feature   │  git checkout -b feature/xyz
│    branch           │
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ 3. Make changes     │  code, code, code
│    (multiple times) │
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ 4. Commit changes   │  git add .
│    (regularly)      │  git commit -m "type: msg"
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ 5. Push branch      │  git push -u origin feature/xyz
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ 6. Open Pull Request│  gh pr create --base develop ...
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ 7. Code Review      │  Team reviews, comments
│    (iterate)        │  Make changes → commit → push
└─────────────────────┘  (PR updates automatically)
    │
    ▼
┌─────────────────────┐
│ 8. Merge PR         │  gh pr merge <number> --merge
│    (by Team Lead)   │  (or squash/rebase)
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│ 9. Update develop   │  git checkout develop
│    (everyone)       │  git pull
└─────────────────────┘
    │
    ▼
┌─────────────────────┐
│10. Delete branch    │  git branch -d feature/xyz
│    (clean up)       │  git push origin --delete feature/xyz
└─────────────────────┘
    │
    ▼
    DONE (ready for next feature)
```

---

## 🚀 Developer's Complete Workflow (with Commands)

### PHASE 1: Starting a New Feature

```bash
# 1. Get the latest code from develop
git checkout develop
git pull origin develop

# 2. Create your feature branch
git checkout -b feature/your-feature-name

# 3. Verify you're on the right branch
git branch
# Should show: * feature/your-feature-name
```

### PHASE 2: Working on the Feature

```bash
# 4. Make changes to code, then stage them
git add .                          # Stage all changes
# OR
git add specific/file.kt           # Stage specific file

# 5. Commit with a meaningful message
git commit -m "feat: add home screen grid"

# 6. Repeat steps 4-5 as you work
# Make small, logical commits, not one huge commit
```

### PHASE 3: Pushing to GitHub

```bash
# 7. Push your branch (first time)
git push -u origin feature/your-feature-name

# 8. For subsequent pushes (after more commits)
git push
```

### PHASE 4: Opening a Pull Request

```bash
# 9. Create PR using GitHub CLI
gh pr create \
  --base develop \
  --head feature/your-feature-name \
  --title "Your Feature Title" \
  --body "Describe what you did and how to test"

# 10. View your PR in browser
gh pr view --web
```

### PHASE 5: Handling Review Feedback

```bash
# 11. If reviewer requests changes:
# Make the changes locally
git add .
git commit -m "fix: address review comments"
git push                    # PR updates automatically

# 12. If you need to rebase on latest develop (if develop moved forward)
git fetch origin develop
git rebase origin/develop
# Fix conflicts if any, then:
git push --force-with-lease
```

### PHASE 6: After PR is Merged

```bash
# 13. Switch back to develop and get latest
git checkout develop
git pull origin develop

# 14. Delete your feature branch (cleanup)
git branch -d feature/your-feature-name
git push origin --delete feature/your-feature-name
```

---

## 👑 Team Lead's Workflow (Reviewing & Merging)

### PHASE 1: Reviewing PRs

```bash
# 1. List all open PRs
gh pr list

# 2. View a specific PR details
gh pr view 5

# 3. Review and approve
gh pr review 5 --approve --body "Looks good, ship it!"

# OR request changes
gh pr review 5 --request-changes --body "Please fix the typo"
```

### PHASE 2: Merging PRs

```bash
# 4. Merge with standard merge commit
gh pr merge 5 --merge

# OR squash all commits into one (cleaner history)
gh pr merge 5 --squash

# OR rebase and merge
gh pr merge 5 --rebase

# 5. Merge and delete branch in one command
gh pr merge 5 --merge --delete-branch
```

### PHASE 3: After All PRs Merged

```bash
# 6. Tell everyone to update
# (no commands for you, just communication)
```

---

## 🔄 Keeping Your Branch Updated (Rebase Flow)

If `develop` moves forward while you're working on your feature, you need to rebase:

```bash
# While on your feature branch
git fetch origin develop
git rebase origin/develop

# If there are conflicts:
# 1. Git will tell you which files conflict
# 2. Open those files, fix conflicts (remove <<<<, ====, >>>>)
# 3. Save files
git add .
git rebase --continue

# After rebase completes, force push
git push --force-with-lease
```

---

## 🚨 Emergency Fixes

### Undo last commit (keep changes)
```bash
git reset --soft HEAD~1
```

### Undo last commit (discard changes)
```bash
git reset --hard HEAD~1
```

### Accidentally committed to develop (and pushed)
```bash
# 1. Create a branch to save your work
git checkout -b save-my-work

# 2. Reset develop back to origin's state
git checkout develop
git reset --hard origin/develop
git push --force-with-lease
```

### Merge conflict during rebase – abort
```bash
git rebase --abort
```

---

## 📝 Commit Message Format

```
<type>: <short description>

Examples:
feat: add home screen grid
fix: correct audio playback crash
docs: update README with setup steps
style: format code according to style guide
refactor: extract PhraseCard component
test: add unit tests for repository
chore: update dependencies
```

---

## ✅ Daily Checklist for Developers

- [ ] Before starting work: `git checkout develop && git pull`
- [ ] Create branch: `git checkout -b feature/xyz`
- [ ] Commit regularly with good messages
- [ ] Push branch: `git push -u origin feature/xyz`
- [ ] Open PR: `gh pr create ...`
- [ ] Respond to review feedback
- [ ] After merge: `git checkout develop && git pull`
- [ ] Delete branch: `git branch -d feature/xyz`

---

## 📌 Quick Reference Card

| Step | What | Command |
|------|------|---------|
| 1 | Update develop | `git checkout develop && git pull` |
| 2 | New branch | `git checkout -b feature/name` |
| 3 | Stage changes | `git add .` |
| 4 | Commit | `git commit -m "type: message"` |
| 5 | Push (first) | `git push -u origin feature/name` |
| 6 | Push (later) | `git push` |
| 7 | Create PR | `gh pr create --base develop --head feature/name --title "T" --body "B"` |
| 8 | View PR | `gh pr view --web` |
| 9 | Rebase | `git fetch origin develop && git rebase origin/develop && git push --force-with-lease` |
| 10 | After merge | `git checkout develop && git pull` |
| 11 | Delete branch | `git branch -d feature/name && git push origin --delete feature/name` |

---

