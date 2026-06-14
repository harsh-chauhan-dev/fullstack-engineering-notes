# What is a Branch?

Think of a branch as a parallel universe.

By default, every Git repository starts with a branch called main. When you create a new branch, you are essentially making a pointer to a specific commit. From that point on, any new commits you make will stay on that branch, leaving the main branch exactly as it was.


## Why Use Branches?

In professional web development (like when you are working on a MERN stack project), you never write code directly on the main branch.

- Feature Development: Create a feature-login branch to work on the login page.

- Bug Fixes: Create a bugfix-header branch to fix a specific CSS issue.

- Experimentation: Try a new library without ruining your stable code.

- Safety: If your experiment fails, you can simply delete the branch and go back to mai

## Branching Commands

Create a Branch:

This creates the branch but keeps you on your current one.

```bash
git branch <branch-name>
```
---

## Switch to a Branch
To move into your "parallel universe"

```bash
git checkout <branch-name>
# OR (Modern Git command)
git switch <branch-name>
```
---
## The "Shortcut" (Create & Switch)

This is the command you will use 90% of the time. It creates the branch AND immediately moves you into it.

```bash
git checkout -b <branch-name>
# OR (Modern Git command)
git switch -c <branch-name>
```
---
## List All Branches

To see all the branches in your project (and which one you are currently on):

```bash
git branch
```

- The branch you are currently on will have a star (*) next to it.

---
## The "HEAD" Pointer
In Git, HEAD is a special pointer that tells Git which branch you are currently standing on. When you checkout or switch, you are essentially moving the HEAD to a different branch.

---
## Delete a Branch

Once you have merged your feature back into main, you can delete the old branch to keep your repository clean.
 
```bash
git branch -d <branch-name>
```

- Use `-D` (capital D) if you need to force delete a branch that hasn't been merged yet.

---
## Real-World Scenario
Imagine you are building E-commerce startup site:

1. You are on main branch.
2. You want to add a new feature: User Authentication.
3. You create a new branch: git checkout -b feature/user-auth
4. You work on the feature and commit changes.
5. You switch back to main: git checkout main
6. You merge the feature branch into main: git merge feature/user-auth

---
## 🧪 Practical Exercise
Try this in your terminal right now:

1. Create a folder named git-test.

2. Run git init.

3. Create a file named hello.txt.

4. Check its status using git status.

5. Add the file to staging using git add.

6. Commit it with the message "First commit" using git commit -m "First commit".

7. Create a new branch named feature/hello.

8. Switch to the feature/hello branch.

9. Modify hello.txt and commit the changes.

10. Switch back to main branch.

11. Merge the feature/hello branch into main.

12. Delete the feature/hello branch.
