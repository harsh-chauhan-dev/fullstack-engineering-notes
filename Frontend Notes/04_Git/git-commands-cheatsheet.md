# 🚀 Getting Started

| Command | Description |
| :--- | :--- |
| `git init` | Initialize a new local Git repository. |
| `git clone <url>` | Clone (download) a project from GitHub. |
| `git config --global user.name "Name"` | Set your commit name. |
| `git config --global user.email "email"` | Set your commit email. |

## The Daily Workflow (Add & Commit)

| Command | Description |
| :--- | :--- |
| `git status` | Show the status of your files. |
| `git add .` | Stage all changes. |
| `git commit -m "message"` | Commit changes with a message. |
| `git push origin main` | Push changes to GitHub. |
| `git pull origin main` | Pull changes from GitHub. |

## Branching & Merging

| Command | Description |
| :--- | :--- |
| `git branch` | List all local branches. |
| `git branch <name>` | Create a new branch. |
| `git checkout <name>` | Switch to a specific branch. |
| `git checkout -b <name>` | Create a new branch and switch to it instantly. |
| `git merge <name>` | Merge the specified branch into your current branch. |
| `git branch -d <name>` | Delete a branch (only if merged). |

## Working with GitHub (Remotes)

| Command | Description |
| :--- | :--- |
| `git remote add origin <url>` | Connect your local repo to a GitHub repo. |
| `git push -u origin <branch>` | Push local commits to GitHub (sets default). |
| `git pull` | Fetch and merge changes from GitHub to local. |
| `git fetch` | Download changes from GitHub but don't merge yet. |
| `git remote -v` | List all remote connections. |

## Undoing & Fixing Mistakes

| Command | Description |
| :--- | :--- |
| `git checkout -- <file>` | Discard changes in a specific file (revert to last commit). |
| `git reset HEAD <file>` | Unstage a file but keep the changes in your editor. |
| `git reset --soft HEAD~1` | Undo the last commit but keep your work staged. |
| `git reset --hard HEAD~1` | Undo the last commit and delete all changes. |
| `git revert <commit-id>` | Create a new commit that "undoes" a previous commit. |

## Advanced & Utilities

| Command | Description |
| :--- | :--- |
| `git log --oneline` | View a condensed history of commits. |
| `git log --graph --all` | View a visual tree of all branches and commits. |
| `git diff` | Show changes between working directory and staging. |
| `git stash` | Temporarily "save" uncommitted changes to clean the workspace. |
| `git stash pop` | Bring back the changes you just stashed. |
| `git rm <file>` | Remove a file from the working directory and stage the deletion. |

## 💡 Pro-Tips 

| Tip | Description |
| :--- | :--- |
| **Commit Often** | Small, frequent commits are easier to debug than one giant commit at the end of the day. |
| **Ignore Secrets** | Always create a `.gitignore` file and add `.env` or `node_modules` to it so you don't accidentally push sensitive API keys to GitHub. |
| **Meaningful Messages** | Instead of `git commit -m "update"`, use `git commit -m "feat: integrate JWT auth middleware"`. |
