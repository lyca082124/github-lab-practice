# Git & GitHub Lab - Complete Step-by-Step Guide

## Overview
This guide covers three parts:
- **Part A**: Initialize local repository and push to GitHub
- **Part B**: Feature branch workflow (create branch, add files, open & merge PR)
- **Part C**: Peer collaboration & conflict resolution (pair work)

---

## Part A: Initialize Local Repository and Push to GitHub

### Step 1: Create and Navigate to Your Project Directory
```bash
cd c:\Users\lyca\Downloads\lyy\git-activity
```

### Step 2: Initialize Git Repository
```bash
git init
```
This creates a hidden `.git` folder to track your repository.

### Step 3: Check Repository Status
```bash
git status
```
This shows untracked files in your directory.

### Step 4: Create Initial Files (if not already created)
Create README.md:
```markdown
# This is our Collaborative Project.
```

### Step 5: Stage Files for Commit
```bash
git add README.md
```
Or stage all files:
```bash
git add .
```

### Step 6: Commit Your Changes
```bash
git commit -m "Initial commit: Add README"
```

### Step 7: Create GitHub Repository
1. Go to https://github.com
2. Click "New" or "+" icon → "New repository"
3. Repository name: `github-lab-practice`
4. **IMPORTANT**: Do NOT check "Initialize with README" (we already have one)
5. Click "Create repository"

### Step 8: Link Local Repository to GitHub
```bash
git remote add origin https://github.com/YOUR_USERNAME/github-lab-practice.git
```
Replace `YOUR_USERNAME` with your actual GitHub username.

### Step 9: Verify Remote Connection
```bash
git remote -v
```
You should see fetch and push URLs.

### Step 10: Rename Branch to Main (if needed)
```bash
git branch -M main
```

### Step 11: Push to GitHub
```bash
git push -u origin main
```
The `-u` flag sets upstream tracking for future pushes.

### Step 12: Verify on GitHub
Refresh your GitHub repository page. You should see your README.md file.

---

## Part B: Feature Branch Workflow

### Step 1: Create a New Feature Branch
```bash
git checkout -b feature-bio
```
This creates and switches to the `feature-bio` branch.

### Step 2: Verify Branch Creation
```bash
git branch
```
You should see `feature-bio` highlighted with an asterisk (*).

### Step 3: Create the Bio File
Create about.txt with your professional bio:
```
Professional Bio: I am a passionate developer learning Git and GitHub for version control and collaboration.
```

### Step 4: Check Status
```bash
git status
```
You should see about.txt as an untracked file.

### Step 5: Stage the New File
```bash
git add about.txt
```

### Step 6: Commit the Changes
```bash
git commit -m "Add professional bio file"
```

### Step 7: Push Feature Branch to GitHub
```bash
git push origin feature-bio
```

### Step 8: Create Pull Request on GitHub
1. Go to your GitHub repository
2. You'll see a yellow banner: "feature-bio had recent pushes" with "Compare & pull request" button
3. Click "Compare & pull request"
4. Verify:
   - Base: `main`
   - Compare: `feature-bio`
5. Add PR title: "Add professional bio file"
6. Add description (optional): "This PR adds about.txt with my professional bio"
7. Click "Create pull request"

### Step 9: Review and Merge Pull Request
1. Review the changes in the PR
2. Check the "Files changed" tab
3. Click "Merge pull request"
4. Confirm merge
5. Click "Delete branch" (optional but recommended)

### Step 10: Update Local Main Branch
Switch back to main:
```bash
git checkout main
```

Pull the merged changes:
```bash
git pull origin main
```

### Step 11: Verify Files
```bash
ls
```
You should see both README.md and about.txt.

---

## Part C: Peer Collaboration & Conflict Resolution

### Setup: Pair Work
- **Student A**: Repository Owner
- **Student B**: Contributor

---

### Step 1: Student A Invites Student B as Collaborator

**Student A does this:**
1. Go to your GitHub repository
2. Click **Settings** tab
3. In left sidebar, click **Collaborators** (or "Collaborators and teams")
4. Click **Add people** button
5. Enter Student B's GitHub username or email
6. Select Student B from the dropdown
7. Click **Add [username] to this repository**
8. Student B will receive an invitation email or notification

**Student B accepts invitation:**
1. Check email or GitHub notifications
2. Click the invitation link
3. Click **Accept invitation**

---

### Step 2: Student B Clones Student A's Repository

**Student B does this:**

Get the repository URL from Student A:
```
https://github.com/STUDENT_A_USERNAME/github-lab-practice.git
```

Clone the repository:
```bash
git clone https://github.com/STUDENT_A_USERNAME/github-lab-practice.git
```

Navigate to the cloned directory:
```bash
cd github-lab-practice
```

Verify remote connection:
```bash
git remote -v
```

---

### Step 3: The Conflict Setup - Divergent Edits

#### Student A's Actions (On Their Computer):

Edit README.md line 1 to say:
```
This is Student A's Project.
```

**Using PowerShell:**
```bash
(Get-Content README.md) | ForEach-Object { if ($_ -eq 0) { "This is Student A's Project." } else { $_ } } | Set-Content README.md
```

**Or manually:**
1. Open README.md in any text editor
2. Replace line 1 with: `This is Student A's Project.`
3. Save the file

Stage and commit:
```bash
git add README.md
git commit -m "Update project title - Student A"
```

Push to main:
```bash
git push origin main
```

---

#### Student B's Actions (WITHOUT Pulling First):

**IMPORTANT**: Student B should NOT pull before making their change.

Edit README.md line 1 to say:
```
This is a Collaborative Project.
```

**Using PowerShell:**
```bash
(Get-Content README.md) | ForEach-Object { if ($_ -eq 0) { "This is a Collaborative Project." } else { $_ } } | Set-Content README.md
```

**Or manually:**
1. Open README.md in any text editor
2. Replace line 1 with: `This is a Collaborative Project.`
3. Save the file

Stage and commit:
```bash
git add README.md
git commit -m "Update project title - Collaborative"
```

Try to push:
```bash
git push origin main
```

**EXPECTED ERROR:**
```
To https://github.com/STUDENT_A_USERNAME/github-lab-practice.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to '...'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
```

---

### Step 4: Student B Fixes the Conflict

#### The Problem:
Student B's local main is behind the remote. GitHub has Student A's changes that Student B doesn't have.

#### The Solution - Pull First:

Student B runs:
```bash
git pull origin main
```

**RESULT: Merge Conflict!**

Git will display:
```
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

---

### Step 5: Open README.md to See Conflict Markers

Student B opens README.md and sees:

```markdown
<<<<<<< HEAD
This is a Collaborative Project.
=======
This is Student A's Project.
>>>>>>> [commit-hash]
```

**Understanding the markers:**
- `<<<<<<< HEAD` - Start of Student B's changes (current/local version)
- `=======` - Divider between the two versions
- `>>>>>>> [commit-hash]` - End of Student A's changes (remote/incoming version)

---

### Step 6: Resolve the Conflict

Student B must manually edit the file to combine BOTH changes or choose one.

**Option 1: Keep Student A's Version**
```markdown
This is Student A's Project.
```

**Option 2: Keep Student B's Version**
```markdown
This is a Collaborative Project.
```

**Option 3: Combine Both (Recommended for Collaboration)**
```markdown
# This is Student A's Project - A Collaborative Effort
```

**Or:**
```markdown
# This is a Collaborative Project (Owner: Student A)
```

**IMPORTANT**: Remove ALL conflict markers:
- Delete `<<<<<<< HEAD`
- Delete `=======`
- Delete `>>>>>>> [commit-hash]`

---

### Step 7: Complete the Merge (Student B)

Stage the resolved file:
```bash
git add README.md
```

Commit the merge:
```bash
git commit -m "Resolve merge conflict: Combine project titles"
```

**Note**: Git may open a text editor for the merge commit message. You can:
- Accept the default message
- Or write your own: "Merge remote changes and resolve conflict"

---

### Step 8: Push the Resolution (Student B)

```bash
git push origin main
```

This should now succeed!

---

### Step 9: Student A Pulls the Resolution

Student A should now get the final resolved version:

```bash
git pull origin main
```

---

### Step 10: Verify the Resolution

**Both students check:**

1. View README.md locally:
```bash
cat README.md
```
Or in PowerShell:
```bash
Get-Content README.md
```

2. Check on GitHub:
   - Go to the repository
   - Open README.md
   - Verify it shows the resolved version

3. Check commit history:
```bash
git log --oneline --graph
```

You should see the merge commit in the history.

---

### What We Learned

✓ **Collaboration**: How to work together on the same repository  
✓ **Conflict Detection**: Git prevents overwriting others' work  
✓ **Error Handling**: The `[rejected]` error protects your code  
✓ **Pull Before Push**: Always pull latest changes before pushing  
✓ **Manual Resolution**: You control how conflicts are resolved  
✓ **Communication**: Talk to your partner when conflicts arise

---

### Common Mistakes to Avoid

❌ **Don't force push** (`git push --force`) - This overwrites others' work  
❌ **Don't ignore conflict markers** - Your code won't work with them  
❌ **Don't panic** - Merge conflicts are normal and expected  
❌ **Don't work on the same line** - Coordinate with your partner

### Best Practices

✓ Always `git pull` before starting work  
✓ Communicate with your partner about what files you're editing  
✓ Work on different files or different sections when possible  
✓ Commit frequently with clear messages  
✓ Review changes before committing

---

### Alternative: Fetch First Approach

If Student B wants to see what changed before merging:

```bash
# Fetch without merging
git fetch origin main

# View the differences
git diff HEAD origin/main

# Then pull and resolve
git pull origin main
```

---

### Verification Checklist for Part C

**Student A:**
- [ ] Invited Student B as collaborator
- [ ] Made and pushed changes to README.md
- [ ] Pulled final resolved version
- [ ] Verified conflict is resolved

**Student B:**
- [ ] Cloned Student A's repository
- [ ] Made changes without pulling first
- [ ] Got the `[rejected]` error
- [ ] Ran `git pull origin main`
- [ ] Resolved merge conflict markers
- [ ] Committed and pushed resolution

**Both Students:**
- [ ] README.md shows resolved content
- [ ] No conflict markers remain
- [ ] Can view merge commit in history
- [ ] Repository is synchronized

---

## Useful Git Commands Reference

### Branch Management
```bash
git branch                    # List branches
git branch <name>             # Create branch
git checkout <name>           # Switch to branch
git checkout -b <name>        # Create and switch
git branch -d <name>          # Delete branch
```

### Staging and Committing
```bash
git status                    # Check status
git add <file>                # Stage file
git add .                     # Stage all
git commit -m "message"       # Commit
git commit --amend            # Modify last commit
```

### Remote Operations
```bash
git remote -v                 # Show remotes
git push origin <branch>      # Push branch
git pull origin <branch>      # Pull branch
git fetch origin              # Fetch without merge
```

### Viewing History
```bash
git log                       # Show commit history
git log --oneline             # Compact view
git log --graph               # Visual graph
git diff                      # Show changes
```

---

## Common Issues and Solutions

### Issue: "Remote origin already exists"
**Solution**: 
```bash
git remote remove origin
git remote add origin <URL>
```

### Issue: "Updates were rejected because the tip of your current branch is behind"
**Solution**: 
```bash
git pull origin main
# Resolve any conflicts
git push origin main
```

### Issue: Accidentally committed to wrong branch
**Solution**:
```bash
git stash                     # Save changes
git checkout correct-branch   # Switch branch
git stash pop                 # Apply changes
```

### Issue: Want to undo last commit
**Solution**:
```bash
git reset --soft HEAD~1       # Keep changes staged
git reset HEAD~1              # Keep changes unstaged
```

---

## Deliverables to Submit

1. **Repository URL**: https://github.com/YOUR_USERNAME/github-lab-practice
2. **Network Graph**: 
   - Go to repository → Insights → Network
   - Take screenshot showing branch structure
3. **Pull Request Screenshot**:
   - Show the merged PR from feature-bio to main
4. **Final README.md**:
   - Should contain the resolved project title
5. **Collaboration Evidence**:
   - Show commit history with both students' contributions
   - Show merge commit from conflict resolution

---

## Tips for Success

1. **Always pull before pushing** to avoid unnecessary conflicts
2. **Use descriptive commit messages** that explain what and why
3. **Create feature branches** for each new feature or fix
4. **Review PRs carefully** before merging
5. **Communicate with team members** when working on same files
6. **Commit frequently** with small, logical changes
7. **Test before committing** to ensure code works
8. **Never force push** to shared branches

---

## Additional Resources

- Git Documentation: https://git-scm.com/doc
- GitHub Guides: https://guides.github.com
- Learn Git Branching: https://learngitbranching.js.org
- Pro Git Book: https://git-scm.com/book/en/v2

---

**Created**: 2026  
**Purpose**: Git & GitHub Lab Activity Guide  
**Parts**: A (Repository Setup), B (Feature Branch), C (Peer Collaboration)
