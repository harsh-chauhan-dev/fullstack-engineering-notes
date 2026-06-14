# 🌐 The Centralized Cloud Model
While Git is distributed (everyone has a copy), GitHub acts as the "Single Source of Truth." The workflow revolves around keeping your local machine in sync with the remote repository on GitHub.

---

## The Standard Feature-Branch Workflow
**1. Fork (Optional):** Create a copy of someone else's repo in your account.
**2. Clone:** Download the repo to your local machine.
```bash
git clone https://github.com/chauhan-harsh630/fullstack-engineering-notes.git
```
**3.Branch:** Create a new branch for your specific task.
```bash
git checkout -b feature-name
```

**4.Work & Commit:** Write code and save snapshots locally.
```bash
git add .
git commit -m "Your descriptive message"
```

**5.Push:** Upload your branch to GitHub.
```bash
git push origin feature-name
```
**6.Pull Request (PR):** Go to GitHub and ask to "merge" your branch into the main branch. This is where Code Review happens.

---

# Remote Commands Table

| Command | Action |
| --- | --- |
| git remote -v | See which GitHub URL your local repo is connected to. |
| git push origin <branch> | Sends your local commits to the GitHub branch. |
| git pull origin <branch> | Fetches changes from GitHub and merges them into your current branch. |
| git fetch | Downloads changes from GitHub but does not merge them (safer than pull). |


---

## What is a Pull Request (PR)?
A Pull Request isn't a Git command; it's a GitHub feature. It’s a formal way to say: "I've finished the security API for the_cyberpath. Please review my code before it goes live on the main site.

### The PR Benefits:

**1. Discussion:** Teammates can comment on specific lines of code.

**2. Automated Testing:** GitHub can run scripts to see if your new code breaks the app.

**3. Conflict Detection:** GitHub will tell you if your branch has "Merge Conflicts" before you try to merge.

---
## Real-World Scenario: Collaborating on a Project

1. Friend pushes a new Navbar to main.

2. You are working on a Sidebar in a branch.

3. Before you push, you run git pull origin main. This brings your friend's Navbar into your local machine so you can make sure the Sidebar looks good next to it.

4. You Push your sidebar and open a Pull Request.