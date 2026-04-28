---
name: smart-commit
description: "Streamline Git commits with intelligent workflow automation. Use this skill whenever the user types 'commit this' — it automatically detects the repository type, handles merge conflicts, suggests and validates commit messages, optionally creates feature branches, generates PRs, and auto-merges when approved. Works with both GitHub and GitLab repositories."
compatibility:
  - requires: git, gh CLI (GitHub), glab CLI (GitLab)
---

# Smart Commit Workflow

Automate the entire Git workflow in one command: detect platform, resolve conflicts, craft commit messages, create feature branches, generate PRs, and auto-merge on approval.

## Trigger

User types or requests: **"commit this"** or **"commit changes"**

## Workflow Steps

### 1. Repository Detection
```bash
# Check local git status
git status

# Detect remote URL
git config --get remote.origin.url
```

Determine if it's GitHub or GitLab from the URL. If unclear, **prompt the user** to specify which platform.

### 2. Handle Conflicts
```bash
git status --porcelain
```

If there are merge conflicts:
- **Automatically detect** conflicted files
- **Parse conflict markers** (<<<<<<, ======, >>>>>>)
- **Resolve intelligently**: For simple non-overlapping changes, resolve automatically. For overlapping changes, **mark for manual review** but preserve the most recent non-deleted content
- Run: `git add .` after auto-resolution
- If conflicts remain unresolvable, ask the user to resolve them manually and come back

### 3. Fetch & Sync
```bash
git fetch origin
git rebase origin/$(current-branch) --autostash
```

If the current branch doesn't exist on remote, just fetch and proceed.

### 4. Suggest Commit Message
Analyze the staged changes:
```bash
git diff --cached
```

**Generate a concise suggestion** based on:
- Changed files (e.g., "api.py", "README.md")
- Commit scope if detectable (e.g., "feat", "fix", "docs")
- Common patterns in the codebase

**Present the suggestion to the user:**
```
Suggested commit message:
  "feat(auth): add JWT token refresh endpoint"

Press Enter to accept, or type a new message:
```

Accept their input (edited or as-is) and proceed. **Ask once and move forward.**

### 5. Feature Branch Decision
**Ask:** "Do you want to create a feature branch for this? (y/n)"

- **If yes:**
  - Current branch should be `develop` or `main`
  - **Create feature branch:** `git checkout -b feature/<branch-name>`
  - Use the user's commit message (or a normalized version) as the branch name, e.g., "jwt-token-refresh"
  - Push: `git push origin feature/<branch-name> -u`
  
- **If no:**
  - Proceed to commit on the current branch

### 6. Commit & Push
```bash
git commit -m "<message>"
git push origin <current-branch>
```

### 7. Create Pull Request
Determine the base branch:
- If on `develop` or feature branch → base is `develop`
- If on `main` → base is `main` (but warn if creating a feature on main)

**Create PR with:**
- **Title:** first line of commit message
- **Description:** full commit message + a note that this was auto-created
- **Target:** `develop` branch

**GitHub:**
```bash
gh pr create --title "<title>" --body "<description>" --base develop
```

**GitLab:**
```bash
glab mr create --title "<title>" --description "<description>" --target-branch develop
```

Capture the PR/MR URL.

### 8. Auto-Merge on Approval
After PR is created, **ask the user:**
```
Is the PR approved? (y/n)
```

**When user responds "yes":**

**GitHub:**
```bash
gh pr merge <pr-number> --squash
```

**GitLab:**
```bash
glab mr merge <mr-number>
```

**Do not delete the branch** — leave that to the user.

**When user responds "no":**
```
PR created and waiting for approval.
Check: <PR-URL>
```

## Output Summary

After successful completion, output a **short summary** (under 100 tokens):

```
✓ Committed to [branch]: "[message]"
✓ Pushed to origin
✓ PR created: [PR-URL] (targeting develop)
✓ Auto-merge enabled on approval
```

If feature branch was created:
```
✓ Feature branch created: feature/[branch-name]
```

Keep it concise — one status line per action.

## Error Handling

- **Unresolvable conflicts:** Stop and ask user to resolve manually
- **Auth failures (GitHub/GitLab):** Prompt user to authenticate via `gh auth login` or `glab auth login`
- **Rate limits:** Retry after 60 seconds, inform the user
- **Network errors:** Retry up to 3 times before failing

## Assumptions

- User has `git`, `gh` CLI (GitHub), and/or `glab` CLI (GitLab) installed
- User has valid credentials configured for their platform
- User is on `develop` or `main` when creating feature branches
- Staging area contains the changes to commit (run `git add .` if needed)
