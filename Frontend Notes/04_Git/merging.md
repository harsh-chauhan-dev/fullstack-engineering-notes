# What is Merging?

Merging is the act of taking the independent lines of development created by git branch and integrating them into a single branch. Usually, you merge a feature branch into the main branch.

## Types of Merges

### A. Fast-Forward Merge

This happens if the main branch hasn't changed since you created your feature branch. Git simply moves the main pointer forward to your latest commit. It’s clean and linear.

### B. Three-Way Merge (Recursive)
This happens if main has moved forward (perhaps a teammate pushed a hotfix) while you were working on your branch. Git looks at:

1.The tip of main.

2.The tip of your feature branch.

3.The "Common Ancestor" (where they first split).
Git then creates a new "Merge Commit" to tie them together.

---

## How to Merge (Step-by-Step)
To merge changes, you must first be on the branch you want to merge into (usually main).

1.Switch to the target branch:

```bash
git checkout main
```

2.Pull latest changes (Best Practice):

```bash
git pull origin main
```

3.Merge the feature branch:

```bash
git merge feature-branch-name
```

4.Delete the old branch (Optional):

```bash
git branch -d feature-branch-name
```

# Dealing with Merge Conflicts ⚠️
A Merge Conflict happens when Git doesn't know which code to keep. This usually occurs when two people (or two branches) edit the same line of the same file.

## How Git Marks Conflicts
When a conflict occurs, Git pauses the merge and marks the conflicting code in your file like this:

```
<<<<<<< HEAD
This is the code from the current branch (main).
=======
This is the code from the branch you are merging in (feature).
>>>>>>> feature-branch-name
```

## Resolving Conflicts (Step-by-Step)
1.Open the conflicted file.
2.Edit the code to keep what you want. You can:
Keep only your changes.
Keep only the incoming changes.
Keep both (combine them).
Remove the conflict markers (<<<<<<<, =======, >>>>>>>).
3.Stage the file:

```bash
git add conflicted-file-name.txt
```
4.Commit the resolution:

```bash
git commit -m "Resolved merge conflict in file.txt"
```