# The Three States of Git

To master Git, you must understand that your files reside in one of three areas at any given time. This is often called the Git Workflow.

1. Working Directory: The actual files you are editing on your computer.

2. Staging Area (Index): A "preview" area where you gather changes you want to include in your next snapshot.

3. Local Repository (.git folder): Where Git permanently stores the snapshots (commits).

---

##  The Lifecycle of a File

As you work, your files move through different statuses:

1. **Untracked**: Git sees the file for the first time. It ignores it by default.
2. **Modified**: You changed the file, but haven't told Git to save it yet.
3. **Staged**: You've added the file to the "staging area" using `git add`. Git is ready to save it.
4. **Committed**: You've saved the file to the Local Repository. It is now a permanent snapshot.

---

##  Your First Local Workflow
Even without GitHub, you can use Git locally to track your progress. Here is the standard "Basics" loop:

## Step 1: Initialize
Go to your project folder in the terminal and run:

```bash
git init
```

This creates a hidden `.git` folder inside your project. This folder is the "brain" of your repository. It contains the entire history of your project.

---
## Step 2: Tracking Changes
If you create a new file, Git won't track it automatically. You must add it:

```bash
git add filename.js   # Stages a specific file
git add .             # Stages EVERYTHING in the folder
```

## Step 3: Commit
Once files are staged, you must "commit" them. A commit is a snapshot with a message describing the changes.

```bash
git commit -m "Initial commit"  # Creates the first snapshot
```

**Why the `-m`?** It stands for "message". You must describe what you did. Good commit messages are crucial for teamwork.

---
- Pro-Tip: Always write commit messages in the present tense (e.g., "Add login feature" instead of "Added login feature"). Think of it as giving an instruction to the codebase.

## Real-World Example
Imagine you are building a E-Commerce App:

1. You create the folder: `mkdir ecommerce-app`
2. You initialize Git: `cd ecommerce-app && git init`
3. You create your first file: `touch index.html`
4. You write some HTML code.
5. You check the status: `git status` (It shows "Untracked")
6. You stage it: `git add index.html`
7. You commit it: `git commit -m "Add landing page"`

Now, even if you delete the file, you can restore it using Git!
---
## 🧪 Practical Exercise
Try this in your terminal right now:

1. Create a folder named git-test.

2. Run `git init`.

3. Create a file named hello.txt.

4. Check its status using `git status`.

5. Add the file to staging using `git add`.

6. Commit it with the message "First commit" using `git commit -m "First commit"`.
---
After you run `git status` on a brand new file, what color does the terminal usually show the filename in, and what status does it say the file has?