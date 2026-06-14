
## 📁 Repository Structure
```
04-Git/
│
├── README.md
├── git-basics.md
├── branching.md
├── merging.md
├── github-workflow.md
├── git-commands-cheatsheet.md

```

--- 

# 📦 Git & Version Control

Git is a distributed Version Control System (VCS) that runs locally on your machine. It tracks every change made to your files.

If your code breaks at 2:00 PM, Git allows you to revert to a working version from 1:00 PM.

---

## 🔹 Core Concepts

- **Repository (Repo):** A project folder where Git tracks all changes.

- **Working Directory:** Where you write and modify code.

- **Staging Area:** A temporary area where changes are prepared before committing.

- **Commit:** A snapshot of your code at a specific point in time.

- **Branch:** A separate line of development used to build features independently.

- **HEAD:** Pointer to the latest commit in the current branch.

---

## 🔹 What is GitHub?

GitHub is a cloud-based platform for hosting Git repositories.

While Git works locally, GitHub enables:
- Remote storage
- Collaboration
- Code sharing

---

## 🔹 Why Use GitHub?

- Backup your code
- Collaborate with teams
- Showcase projects (portfolio)
- Manage open-source contributions

---

## 🔹 Git vs GitHub

| Git | GitHub |
|-----|--------|
| Version control system | Cloud hosting platform |
| Works locally | Works remotely |
| CLI-based | GUI + collaboration tools |

---

## 🔹 Git Workflow

1. Working Directory → Write code  
2. Staging Area → `git add`  
3. Local Repository → `git commit`  
4. Remote Repository → `git push`  

---

## 🔹 Basic Git Commands

```bash
git init
git clone <repo-url>
git status
git add .
git commit -m "message"
git push origin main
git pull origin main