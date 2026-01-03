# Git Reference Guide
## دليلك الشامل لـ Git

Hey! This is my personal Git reference that I put together while learning. It's not perfect, but it covers everything I use daily. The Arabic parts are there to help me remember stuff faster.

---

## What is Git anyway? (إيه هو Git أصلاً؟)

Git is version control - basically a time machine for your code. You can save snapshots (commits), work on different versions (branches), and collaborate with others without messing things up.

Think of it like Google Docs version history but way more powerful.

**Key terms:**
- **Repository/Repo (مستودع)** - Your project folder that Git tracks
- **Commit (إيداع)** - A save point, like a checkpoint in a game
- **Branch (فرع)** - A separate timeline where you can work on stuff
- **Remote (البعيد)** - The version on GitHub/GitLab
- **HEAD** - Where you are right now in the project history

---

## Getting Started

### First time setup
You only do this once:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

**** بتقول لـ Git مين أنت عشان يعرف مين عمل كل commit

### Starting a new project

```bash
# In your project folder
git init
```

This creates a hidden .git folder where all the magic happens.

### Getting an existing project

```bash
git clone https://github.com/username/repo-name.git
```

**متى تستخدمه؟** لما تيجي تشتغل على مشروع موجود أو تنزل كود من GitHub

---

## The Basic Workflow (الأساسيات اللي هتستخدمها كل يوم)

### 1. Check what's happening

```bash
git status
```

I run this like 50 times a day. It shows you what files changed, what's staged, etc.

### 2. Add files (staging)

```bash
# Add one file
git add filename.js

# Add everything
git add .

# Add all JS files
git add *.js
```

**** بتجهز الملفات اللي عايز تحفظها. مش كل حاجة اتعدلت لازم تتحفظ مرة واحدة

### 3. Commit (save your work)

```bash
git commit -m "Add login feature"
```

**Important:** Write clear commit messages! Future you will thank present you.

Good messages:
- "Fix navigation bug on mobile"
- "Add user profile page"
- "Update README with new instructions"

Bad messages:
- "update"
- "fix stuff"
- "asdf"

### Quick add + commit

```bash
git commit -am "Your message"
```

Only works for files Git already knows about (not new files).

---

## Branches (التفريع)

Branches are awesome. They let you work on new stuff without breaking what's already working.

### See all branches

```bash
git branch
```

The one with * is where you are now.

### Create a new branch

```bash
# Old way
git checkout -b feature-name

# New way (cleaner)
git switch -c feature-name
```

**متى؟** لما تبدأ تشتغل على حاجة جديدة (feature أو bug fix)

### Switch between branches

```bash
git switch main
git switch feature-name

# Go back to previous branch
git switch -
```

### Delete a branch

```bash
# Safe delete (only if merged)
git branch -d feature-name

# Force delete
git branch -D feature-name
```

**** لما تخلص من فرع ومش محتاجه تاني، احذفه عشان متتلخبطش

---

## Working with Remote (GitHub/GitLab)

### Connect to GitHub

```bash
git remote add origin https://github.com/username/repo.git
```

Do this once when you first connect your local repo to GitHub.

### Push your work

```bash
# First time
git push -u origin main

# After that, just
git push
```

**** ترفع شغلك من جهازك لـ GitHub عشان يتحفظ على النت

### Get latest changes

```bash
git pull
```

Run this before you start working to get what others did.

**Pro tip:** Make it a habit to pull first thing in the morning!

### See your remotes

```bash
git remote -v
```

---

## Merging (الدمج)

When you finish working on a feature branch, you merge it back to main.

```bash
# Switch to main first
git switch main

# Make sure it's updated
git pull

# Merge your feature
git merge feature-name
```

### When conflicts happen (وقت التعارضات)

Sometimes Git can't auto-merge and you get conflicts. Don't panic!

You'll see stuff like this in your files:
```
<<<<<<< HEAD
your current code
=======
incoming code
>>>>>>> feature-name
```

Just:
1. Open the file
2. Decide what to keep
3. Delete the markers (<<<, ===, >>>)
4. Save the file
5. Add and commit:

```bash
git add .
git commit -m "Resolve merge conflict"
```

### Cancel a merge

```bash
git merge --abort
```

**متى؟** لو حصلت مشكلة أثناء الدمج وعايز تلغي كل حاجة

---

## Viewing History

### See commits

```bash
# Full log
git log

# Compact view (my favorite)
git log --oneline

# Last 5 commits
git log -n 5

# With a graph
git log --graph --oneline --all
```

### See what changed

```bash
# Unstaged changes
git diff

# Staged changes
git diff --staged

# Compare branches
git diff main feature-name
```

---

## Undoing Stuff (التراجع - الجزء المهم!)

### Oops, I want to discard my changes

```bash
# One file
git restore filename.js

# Everything
git restore .
```

**** لما تعدل حاجة وتكتشف إنها غلط وعايز ترجعها زي ما كانت

### I added files by mistake

```bash
git restore --staged filename.js
```

This unstages the file but keeps your changes.

### I want to undo my last commit

```bash
# Keep the changes
git reset --soft HEAD~1

# Keep changes but unstage them
git reset HEAD~1

# Delete everything (careful!)
git reset --hard HEAD~1
```

**متى تستخدم أيهم؟**
- `--soft`: عايز تعدل الـ commit (تغير الملفات أو الرسالة)
- بدون flag: عايز تشيل الـ commit بس تحتفظ بالتعديلات
- `--hard`: عايز تلغي كل حاجة **خطر!**

### Fix the last commit

```bash
# Change the commit message
git commit --amend -m "Better message"

# Add forgotten files
git add forgotten-file.js
git commit --amend --no-edit
```

---

## Stash (الحفظ المؤقت)

Super useful when you're in the middle of something and need to switch branches.

```bash
# Save your work temporarily
git stash

# See what you stashed
git stash list

# Get it back
git stash pop

# Get it back but keep the stash
git stash apply
```

**** لما تكون شغال على حاجة ولسه مخلصتش بس محتاج تبدل فرع بسرعة

---

## Common Scenarios (مواقف بتحصل كتير)

### Starting a new feature

```bash
git switch main
git pull
git switch -c feature/cool-new-thing
# ... do your work ...
git add .
git commit -m "Add cool new thing"
git push -u origin feature/cool-new-thing
```

### Finishing a feature

```bash
git switch main
git pull
git merge feature/cool-new-thing
git push
git branch -d feature/cool-new-thing
```

### Oh no, I committed to the wrong branch!

```bash
# 1. Note the commit hash
git log --oneline

# 2. Switch to the right branch
git switch correct-branch

# 3. Bring the commit over
git cherry-pick <commit-hash>

# 4. Go back and remove it
git switch wrong-branch
git reset --hard HEAD~1
```

### I need to update my branch with main

```bash
# While on your feature branch
git pull origin main
```

Or if you want a cleaner history:

```bash
git rebase main
```

**Note:** Don't rebase if others are using your branch!

---

## Things That Saved Me

### Check before you commit

```bash
git diff --staged
```

Shows exactly what you're about to commit. Has saved me many times!

### Create useful aliases

Add these to make life easier:

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.lg "log --oneline --graph --all"
```

Now you can type `git st` instead of `git status`!

### The .gitignore file

Create a `.gitignore` file in your project root to ignore files you don't want to track:

```
node_modules/
.env
*.log
.DS_Store
dist/
```

### If you lose something

```bash
git reflog
```

This shows EVERYTHING you did. You can usually recover "lost" commits from here.

---

## Troubleshooting (المشاكل الشائعة)

### "I can't push!"

Usually means someone else pushed before you:

```bash
git pull
# Fix any conflicts if needed
git push
```

### "Permission denied" or "Repository not found"

Check if you're using the right URL:

```bash
git remote -v
```

If it's SSH and causing problems, switch to HTTPS:

```bash
git remote set-url origin https://github.com/username/repo.git
```

### "Merge conflict - help!"

Don't stress. Open the files with conflicts, look for the markers, decide what to keep, and commit:

```bash
# If you want to keep your version
git checkout --ours filename.js

# If you want their version  
git checkout --theirs filename.js

# Then
git add .
git commit
```

### Something is really broken

```bash
# See what happened
git reflog

# Go back to a working state
git reset --hard <commit-hash>
```

---

## Quick Commands I Use Daily

```bash
git status                    # What's up?
git add .                     # Stage everything
git commit -m "message"       # Save
git push                      # Upload
git pull                      # Download
git switch main               # Go to main
git switch -c feature-name    # New branch
git log --oneline            # History
git stash                     # Quick save
git stash pop                 # Get it back
```

---

## Some Advice

**Commit often:** Small commits are better than huge ones. You can always squash them later.

**Pull before you push:** Saves you from merge conflicts.

**Branch names matter:** Use clear names like `feature/user-auth` or `bugfix/login-error`, not `test` or `my-branch`.

**Write good commit messages:** Your team (and future you) will appreciate it.

**Don't force push:** Unless you really know what you're doing and you're sure no one else is using that branch.

**Backup important work:** Push to GitHub regularly. Your laptop could die anytime!

---

## Resources

If you get stuck:
- GitHub's Git handbook (really good)
- Oh Shit, Git!?! website (for when things go wrong)
- Just Google the error message (seriously, someone else had the same problem)

---

That's basically everything I use! Git seems complicated at first but once you get the hang of the basic workflow (add, commit, push, pull, branch, merge), you're 90% there.

The rest you can Google when you need it.

Good luck! 🚀
