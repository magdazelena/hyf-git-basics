# Git Collaboration Exercises

> **Format:** Mix of pair work and solo exercises  
> **Total Time:** ~90 minutes  
> **Setup:** Each student needs a GitHub account and Git installed

---

## 📋 Pre-Exercise Setup

Before starting, make sure everyone has:

```bash
# Verify Git is installed
git --version

# Configure your identity (if not done)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

---

## Exercise 1: Merge Conflict Resolution 👥

**Format:** Pairs | **Time:** ~20 minutes

### 🎯 Learning Goal

Understand how merge conflicts occur and practice resolving them in VS Code.

### 👥 Setup

- **Partner A** and **Partner B** work together
- Partner A creates the repository, Partner B gets added as collaborator

### 📝 Instructions

#### Partner A (Repository Owner)

1. **Create a new repository on GitHub:**
   - Name: `conflict-practice`
   - Initialize with README: No
   - Public or Private: either is fine

2. **Set up locally:**

   ```bash
   mkdir conflict-practice
   cd conflict-practice
   git init
   ```

3. **Create initial file and push:**

   ```bash
   # Create fruits.txt with content:
   echo "apple" > fruits.txt
   echo "banana" >> fruits.txt
   echo "cherry" >> fruits.txt

   git add fruits.txt
   git commit -m "feat: add initial fruits list"

   git remote add origin https://github.com/YOUR_USERNAME/conflict-practice.git
   git branch -M main
   git push -u origin main
   ```

4. **Add Partner B as collaborator:**
   - Go to repo Settings → Collaborators → Add people
   - Add Partner B's GitHub username

#### Partner B (Collaborator)

5. **Clone the repository:**

   ```bash
   git clone https://github.com/PARTNER_A_USERNAME/conflict-practice.git
   cd conflict-practice
   ```

6. **Create a feature branch and make changes:**

   ```bash
   git checkout -b feature/more-fruits
   ```

7. **Edit `fruits.txt` - change line 2:**

   ```
   apple
   mango      ← changed from banana
   cherry
   kiwi       ← added new fruit
   ```

8. **Commit your changes (don't push yet!):**
   ```bash
   git add fruits.txt
   git commit -m "feat: replace banana with mango, add kiwi"
   ```

#### Partner A (Simultaneously)

9. **Edit `fruits.txt` on `main` - change line 2:**

   ```
   apple
   pineapple  ← changed from banana
   cherry
   ```

10. **Commit and push:**
    ```bash
    git add fruits.txt
    git commit -m "feat: replace banana with pineapple"
    git push origin main
    ```

#### Partner B (The Conflict!)

11. **Try to update your branch with main:**

    ```bash
    git fetch origin main
    git merge origin/main
    ```

12. **You should see:**

    ```
    CONFLICT (content): Merge conflict in fruits.txt
    Automatic merge failed; fix conflicts and then commit the result.
    ```

13. **Open `fruits.txt` in VS Code. You'll see:**

    ```
    apple
    <<<<<<< HEAD
    mango
    cherry
    kiwi
    =======
    pineapple
    cherry
    >>>>>>> origin/main
    ```

14. **Resolve the conflict:**
    - Discuss with Partner A what the final result should be
    - Edit the file to contain the agreed content (remove conflict markers)
    - Example resolution:
      ```
      apple
      mango
      pineapple
      cherry
      kiwi
      ```

15. **Complete the merge:**

    ```bash
    git add fruits.txt
    git commit -m "merge: resolve fruit conflict, keep both new fruits"
    git push origin feature/more-fruits
    ```

16. **Create a Pull Request on GitHub:**
    - Go to the repository on GitHub
    - Click "Compare & pull request"
    - Add a description and create the PR

### 🤔 Discussion Questions

- What caused the conflict?
- Could this conflict have been avoided? How?
- What would happen if you chose "Accept Current Change" vs "Accept Incoming Change"?

---

## Exercise 2: Git Stash — Emergency Bug Fix 👤

**Format:** Solo | **Time:** ~15 minutes

### 🎯 Learning Goal

Learn to use stash when you need to switch branches with uncommitted work.

### 📝 Instructions

1. **Create a new repository:**

   ```bash
   mkdir stash-practice
   cd stash-practice
   git init
   ```

2. **Create initial setup:**

   ```bash
   echo "# My Project" > README.md
   echo "console.log('Hello');" > app.js
   git add .
   git commit -m "initial: project setup"
   ```

3. **Start working on a feature:**

   ```bash
   git checkout -b feature/greeting
   ```

4. **Edit `app.js` (don't commit!):**

   ```javascript
   console.log("Hello");
   console.log("Welcome to my app!");
   // TODO: add more greetings
   ```

5. **🚨 Emergency! Need to fix a bug on main!**

   ```bash
   git checkout main
   ```

   **You'll see an error:**

   ```
   error: Your local changes to the following files would be overwritten by checkout:
       app.js
   ```

6. **Stash your work:**

   ```bash
   git stash
   ```

7. **Now you can switch:**

   ```bash
   git checkout main
   ```

8. **Fix the "bug":**

   ```bash
   git checkout -b hotfix/typo
   # Edit README.md to fix a "typo"
   echo "# My Awesome Project" > README.md
   git add README.md
   git commit -m "fix: correct project title"
   git checkout main
   git merge hotfix/typo
   ```

9. **Go back to your feature:**

   ```bash
   git checkout feature/greeting
   ```

10. **Check your stash:**

    ```bash
    git stash list
    ```

    **You should see:**

    ```
    stash@{0}: WIP on feature/greeting: abc1234 initial: project setup
    ```

11. **Recover your work:**

    ```bash
    git stash pop stash@{0}
    ```

12. **Verify your changes are back:**
    ```bash
    cat app.js
    ```

### ✅ Success Criteria

- You successfully switched branches without losing work
- Your stashed changes are restored

### 🤔 Reflection

- What's the difference between `git stash pop` and `git stash apply`?
- When would you use `apply` instead of `pop`?

---

## Exercise 3: Reset Soft vs Hard 👤

**Format:** Solo | **Time:** ~15 minutes

### 🎯 Learning Goal

Understand the difference between `--soft` and `--hard` reset.

### 📝 Instructions

1. **Create a new repository:**

   ```bash
   mkdir reset-practice
   cd reset-practice
   git init
   ```

2. **Create a series of commits:**

   ```bash
   echo "First line" > file.txt
   git add file.txt
   git commit -m "commit 1: first line"

   echo "Second line" >> file.txt
   git add file.txt
   git commit -m "commit 2: second line"

   echo "Third line" >> file.txt
   git add file.txt
   git commit -m "commit 3: third line"
   ```

3. **Check your history:**

   ```bash
   git log --oneline
   ```

   **You should see 3 commits.**

4. **Try SOFT reset (undo last commit, keep changes staged):**

   ```bash
   git reset --soft HEAD~1
   ```

5. **Check the state:**

   ```bash
   git status
   git log --oneline
   cat file.txt
   ```

   **What do you notice?**
   - The commit is gone from history
   - Your changes are still staged (green in `git status`)
   - The file still has all three lines

6. **Recommit:**

   ```bash
   git commit -m "commit 3: third line (recommitted)"
   ```

7. **Now try HARD reset (undo and DELETE changes):**

   ```bash
   # First, note the current state
   cat file.txt    # Should show 3 lines

   # Reset hard
   git reset --hard HEAD~1
   ```

8. **Check the state:**

   ```bash
   git status
   git log --oneline
   cat file.txt
   ```

   **What happened?**
   - The commit is gone
   - The file only has 2 lines now
   - The third line is **permanently deleted**

### ✅ Success Criteria

- You understand that `--soft` keeps your changes staged
- You understand that `--hard` deletes changes permanently

### ⚠️ Warning to Remember

> Never use `git reset --hard` unless you're absolutely sure you don't need those changes!

---

## Exercise 4: Revert & Cherry-Pick 👥

**Format:** Pairs | **Time:** ~20 minutes

### 🎯 Learning Goal

Practice using `revert` to undo a commit safely, and `cherry-pick` to copy commits between branches.

### 👥 Setup

- Work with a partner
- Partner A creates the repo, both partners work on it

### 📝 Instructions

#### Partner A: Setup

1. **Create a new repository on GitHub:** `revert-cherry-practice`

2. **Set up locally:**

   ```bash
   mkdir revert-cherry-practice
   cd revert-cherry-practice
   git init

   echo "# Shopping List" > shopping.txt
   echo "- milk" >> shopping.txt
   echo "- bread" >> shopping.txt
   git add shopping.txt
   git commit -m "feat: initial shopping list"

   git remote add origin https://github.com/YOUR_USERNAME/revert-cherry-practice.git
   git push -u origin main
   ```

3. **Add Partner B as collaborator**

#### Both Partners: Clone (Partner B clones)

4. **Partner B clones:**
   ```bash
   git clone https://github.com/PARTNER_A_USERNAME/revert-cherry-practice.git
   cd revert-cherry-practice
   ```

#### Partner A: Make a "bad" commit

5. **Add something you'll later regret:**

   ```bash
   echo "- poison" >> shopping.txt    # Oops!
   git add shopping.txt
   git commit -m "feat: add poison to list"
   git push origin main
   ```

6. **Add a good commit after:**
   ```bash
   echo "- eggs" >> shopping.txt
   git add shopping.txt
   git commit -m "feat: add eggs"
   git push origin main
   ```

#### Partner B: Fix the mistake with revert

7. **Pull the changes:**

   ```bash
   git pull origin main
   ```

8. **Check the log:**

   ```bash
   git log --oneline
   ```

9. **Find the hash of the "poison" commit and revert it:**

   ```bash
   git revert <poison-commit-hash>
   # This opens an editor - just save and close
   ```

10. **Check the result:**

    ```bash
    cat shopping.txt
    git log --oneline
    ```

    **Notice:** The poison commit still exists in history, but a new "revert" commit undoes it!

11. **Push the fix:**
    ```bash
    git push origin main
    ```

#### Partner A: Cherry-pick a commit

12. **Create a new branch with some work:**

    ```bash
    git pull origin main    # Get the revert
    git checkout -b feature/veggies

    echo "- carrots" >> shopping.txt
    git add shopping.txt
    git commit -m "feat: add carrots"

    echo "- broccoli" >> shopping.txt
    git add shopping.txt
    git commit -m "feat: add broccoli"

    echo "- spinach" >> shopping.txt
    git add shopping.txt
    git commit -m "feat: add spinach"
    ```

13. **Scenario: We only want the "carrots" commit on main (not all veggies):**

    ```bash
    git log --oneline
    # Note the hash of the "carrots" commit

    git checkout main
    git cherry-pick <carrots-commit-hash>
    ```

14. **Verify:**

    ```bash
    cat shopping.txt
    git log --oneline
    ```

    **The carrots commit is now on main, but broccoli and spinach stayed on the feature branch!**

### 🤔 Discussion Questions

- When would you use `revert` instead of `reset`?
- What's a real-world scenario where `cherry-pick` would be useful?

---

## Exercise 5: Interactive Rebase — Clean Up History 👥

**Format:** Pairs | **Time:** ~20 minutes

### 🎯 Learning Goal

Use interactive rebase to squash, reorder, and clean up commits before creating a PR.

### ⚠️ Important Rule

> Never rebase commits that have already been pushed to a shared branch!

### 📝 Instructions

#### Work Together on One Computer

1. **Create a new repository:**

   ```bash
   mkdir rebase-practice
   cd rebase-practice
   git init
   ```

2. **Create several messy commits (intentionally bad practice):**

   ```bash
   echo "function greet() {" > app.js
   git add app.js
   git commit -m "WIP"

   echo "  console.log('Hello');" >> app.js
   git add app.js
   git commit -m "WIP 2"

   echo "}" >> app.js
   git add app.js
   git commit -m "finished greet function"

   echo "" >> app.js
   echo "function farewell() {" >> app.js
   git add app.js
   git commit -m "start farewell"

   echo "  console.log('Goodbye');" >> app.js
   echo "}" >> app.js
   git add app.js
   git commit -m "fix farewell"

   # A typo commit
   echo "// Main app fil" >> app.js
   git add app.js
   git commit -m "add comment"

   # Fix the typo
   sed -i '' 's/fil/file/' app.js   # macOS
   # On Linux: sed -i 's/fil/file/' app.js
   git add app.js
   git commit -m "fix typo"
   ```

3. **Look at the messy history:**

   ```bash
   git log --oneline
   ```

   **You see:**

   ```
   abc7777 fix typo
   abc6666 add comment
   abc5555 fix farewell
   abc4444 start farewell
   abc3333 finished greet function
   abc2222 WIP 2
   abc1111 WIP
   ```

4. **Clean this up with interactive rebase!**

   ```bash
   git rebase -i HEAD~7
   ```

5. **An editor opens. Change it to:**

   ```
   pick abc1111 WIP
   squash abc2222 WIP 2
   squash abc3333 finished greet function
   pick abc4444 start farewell
   squash abc5555 fix farewell
   pick abc6666 add comment
   fixup abc7777 fix typo
   ```

6. **Save and close the editor.**

7. **For each squash, you'll be asked to edit the commit message:**
   - First squash: Change to `feat: add greet function`
   - Second squash: Change to `feat: add farewell function`

8. **Check the cleaned history:**

   ```bash
   git log --oneline
   ```

   **You should see:**

   ```
   def3333 add comment
   def2222 feat: add farewell function
   def1111 feat: add greet function
   ```

   **7 messy commits → 3 clean commits!**

### 🤔 Discussion Questions

- Why is clean commit history important?
- When should you NOT use rebase?
- What's the difference between `squash` and `fixup`?

---

## 🏆 Bonus Challenges

If you finish early, try these:

### Challenge 1: The Detached HEAD Rescue

1. Checkout a specific commit by hash
2. Make some changes and commit
3. Return to main
4. Notice Git's warning about orphaned commits
5. Use `git reflog` to find and recover that commit

### Challenge 2: Simulate a Full Workflow

With your partner, simulate a complete feature development:

1. Partner A creates an issue on GitHub
2. Partner B creates a branch, implements the feature
3. Partner B creates a PR, linking to the issue
4. Partner A reviews and requests changes
5. Partner B makes changes and pushes
6. Partner A approves and merges
7. Both partners pull the updated main

### Challenge 3: Rebase Instead of Merge

Repeat Exercise 1, but instead of merging main into your feature branch, use:

```bash
git rebase origin/main
```

Compare the history with what you got from merging.

---

## 📝 Reflection Questions

After completing the exercises, discuss with your partner or group:

1. **Which undo command would you use if...**
   - You committed to the wrong branch?
   - You pushed a commit with a bug to main?
   - You want to combine your last 5 commits into 1?

2. **What's your strategy for avoiding merge conflicts?**

3. **Why do teams use Pull Requests instead of pushing directly to main?**

4. **What makes a commit message "good"?**

---

## ✅ Completion Checklist

- [ ] Exercise 1: Created and resolved a merge conflict
- [ ] Exercise 2: Used stash to switch branches safely
- [ ] Exercise 3: Understand soft vs hard reset
- [ ] Exercise 4: Used revert and cherry-pick
- [ ] Exercise 5: Cleaned up history with interactive rebase
- [ ] Discussed reflection questions with partner/group
