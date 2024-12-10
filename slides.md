---
title: "Stash No More"
subtitle: "Streamlining Multi-Branch Development with Git Worktrees"
theme: academic
highlighter: shiki
drawings:
  persist: false
transition: slide-up
mdc: true
layout: cover
---

# <logos-git-icon /> Stash No More

Streamlining Multi-Branch Development with **Git Worktrees**

```sh
$ git stash clear
```

---
layout: default
---

# Agenda
<br>

- Challenges in Multi-Branch Development
- Common Solutions: Pros and Cons
- Anatomy of a Git Repository
- Introduction to Git Worktrees
- Demo

---

# Multi-Branch Development Pain Points
<br>

**Daily tasks require branch switching:**

- Parallel feature development
- Bug fixes
- Code reviews
- Release preparations
- ...

**Problem:** Managing uncommitted changes during frequent switches **disrupts workflow**.

<v-click>
```sh
error: Your local changes to the following files would be overwritten by checkout
  - ...
```
</v-click>

---
layout: center
---

#### **How do you manage uncommitted changes when switching branches?**


---
transition: view-transition
---

# Common Solutions: Pros and Cons
<br>

**Git Stash**

- **Pros:** straight-forward, avoids unnecessary commits.
- **Cons:** Requires remembering stashes; risk of losing or mismanaging them.

<br>

```sh
$ git stash         # Save current changes
$ git stash list    # View saved stashes
$ git stash apply   # Reapply a stash
```

---
transition: view-transition
---

# Common Solutions: Pros and Cons
<br>

**Premature Commits**

- **Pros:** Prevents stash-related issues.
- **Cons:** Pollutes history, complicates reviews, and adds mental load for brainstorming meaningful commit messages.

<br>

```sh
$ git add --all         # Stage all changes
$ git commit -m "WIP"   # Commit as work-in-progress
```

---

# Common Solutions: Pros and Cons
<br>

**Multiple Repository Clones**

- **Pros:** Eliminates stash/commit drawbacks.
- **Cons:** Disk-heavy; manual git metadata syncing is tedious.

<br>

<Note>Each clone has its own .git folder and works independently.</Note>

<br>

```sh
$ git clone git@github.com:user/repo.git repo1   # Clone into first directory
$ git clone git@github.com:user/repo.git repo2   # Clone into second directory
```

---

# Anatomy of a Git Repository
<br>

The `.git` folder is the heart of a Git repository, storing essential metadata and configurations.

**Overview:**

- `HEAD`: Tracks the currently checked-out branch.
- `objects/`: Stores all commits, trees, and blobs (Git’s content database).
- `refs/`: Contains branch and tag references.
- `index`: Staging area for files to be committed.
- `worktrees/`: Stores all Worktree-related information

<br>
<Note>Next to the .git folder, you’ll find the contents of the main Worktree.</Note>

---
transition: view-transition
---

# Git Worktrees to the Rescue!
<br>

**Definition:**

A **Git Worktree** is an **independent working directory** linked to a Git repository, allowing you to check out a specific branch or commit in a separate directory without affecting the state of other Worktrees.

Each Worktree shares the same `.git` folder but operates as its *own isolated environment*, enabling efficient multi-branch development.

---

# Git Worktrees to the Rescue!
<br>

**Characteristics:**

- **Shared Repository:** All Worktrees share a single `.git` directory, minimizing disk usage.
- **Independent Environments:** Each Worktree has its own working directory and index, operating on their own branch or commit.
- **Parallel Development:** Enables parallel work on multiple branches without switching.

<br>
<Note>When you clone or initialize a Git repository, a main worktree is automatically created. Using the worktree API, you can add additional linked worktrees to the repository.</Note>

---
transition: view-transition
---

# Git Worktree Commands (1/2)
<br>

- Add new Worktree

````md magic-move
```sh
$ git worktree add (-b [new-branch]) [path] [branch] # Create a new Worktree with a specified branch.
```
```sh
$ git worktree add -b featureA ~/.worktrees/repo.feature main
```
```sh
$ git worktree add -b featureA ~/.worktrees/repo.feature main
Preparing worktree (new branch 'featureA')
branch 'featureA' set up to track 'main'.
HEAD is now at dc8f89d Update lockfile
```
````

<br>

<v-click>

- List Worktrees
````md magic-move
```sh
$ git worktree list                # View all existing Worktrees and their details.
```
```sh
$ git worktree list                # View all existing Worktrees and their details.
/Users/User/Code/repo                dc8f89d [main]
/Users/User/.worktrees/repo.feature  dc8f89d [featureA]
```
````

</v-click>


---

# Git Worktree Commands (2/2)
<br>

- Remove existing Worktree

````md magic-move
```sh
$ git worktree remove [path]       # Safely delete an existing Worktree.
```
```sh
$ git worktree remove ~/.worktrees/repo.feature
```
```sh
$ git worktree remove ~/.worktrees/repo.feature
$ git worktree list
```
```sh
$ git worktree remove ~/.worktrees/repo.feature
$ git worktree list
/Users/User/Code/repo                dc8f89d [main]
```
````

<br>

<v-click>

- Prune Worktrees

````md magic-move
```sh
$ git worktree prune               # Clean up references to deleted working directories.
```
```sh
$ rm -rf ~/.worktrees/repo.feature
```
```sh
$ rm -rf ~/.worktrees/repo.feature
$ git worktree list
```
```sh
$ rm -rf ~/.worktrees/repo.feature
$ git worktree list
/Users/User/Code/repo                dc8f89d [main]
/Users/User/.worktrees/repo.feature  dc8f89d [featureA] prunable
```
```sh
$ rm -rf ~/.worktrees/repo.feature
$ git worktree list
/Users/User/Code/repo                dc8f89d [main]
/Users/User/.worktrees/repo.feature  dc8f89d [featureA] prunable
$ git worktree prune
```
```sh
$ rm -rf ~/.worktrees/repo.feature
$ git worktree list
/Users/User/Code/repo                dc8f89d [main]
/Users/User/.worktrees/repo.feature  dc8f89d [featureA] prunable
$ git worktree prune
$ git worktree list
```
```sh
$ rm -rf ~/.worktrees/repo.feature
$ git worktree list
/Users/User/Code/repo                dc8f89d [main]
/Users/User/.worktrees/repo.feature  dc8f89d [featureA] prunable
$ git worktree prune
$ git worktree list
/Users/User/Code/repo                dc8f89d [main]
```
````

</v-click>

<br>

<v-click>
<Note>Explore additional commands like locking, moving, and repairing Worktrees in the docs.</Note>
</v-click>
---
layout: center
---
# Worktree Demo

<!-- 
**Walkthrough:**

1. **Add a worktree.**
2. **List existing worktrees.**
3. **Switch between worktrees.**
4. **Commit and push changes from a worktree.**
5. **Remove an unused worktree.** 
-->

---

# Git Worktree Advantages
<br>

- **Avoids Common Pain Points:** No need for stashing, premature commits, or multiple repository clones.
- **Effortless Context Switching:** Seamlessly switch between branches without disrupting your workflow, increasing productivity.
- **Isolated Environments:** Each Worktree operates in its own separate working directory, providing an independent workspace.
- **Parallel Task Management:** Run long-running tasks like tests or builds in one Worktree while continuing development in another, avoiding downtime.


---

# Git Worktree Caveats
<br>

- **Branch Exclusivity:** A branch can only be checked out in one Worktree at a time, as changes in one would desynchronize the other.
- **Untracked Files Not Copied:**  Only committed files are included in new Worktrees.
- **Setup Overhead:** For projects with expensive setup scripts, creating new Worktrees can slow down workflows and disrupt efficiency.
- **Learning Curve:** May feel complex for beginners or those new to Git Worktrees.
- **Disk Usage:** Disk impact with many Worktrees on large projects.

---

# Git Worktree Best Practices
<br>

- **Organize Worktrees:** Keep all Worktrees in a central folder or have clear rules where to put them.
- **Purposeful Naming:** Name directories based on branch purpose (e.g., `feature`, `bugfix`).
- **Prune Regularly:** Prune/Remove unused Worktrees to avoid clutter.

---
layout: center
---

# Workflow Demo

<!-- 
# Real-Life Example: My Setup

- Git bare repo with three linked worktrees:
  - `master`: Production-ready.
  - `feature`: Development branch.
  - `stale`: Temporary work.
- **TMUX** for easy terminal multiplexing.
-->

---

# Recap
<br>

- **Streamlined Workflows Save Time Daily:** Investing in tools like Git Worktrees simplifies multi-branch development and eliminates repetitive tasks.
- **Perfect for Frequent Branch Switchers:** Ideal for devs frequently switching between branches for tasks like bug fixes, code reviews, or release preparations.
- **Efficient Multi-Branching:** Worktrees enable seamless parallel work on multiple branches, improving productivity and reducing context-switching mental load.
- **Shared Git Metadata:** Worktrees share a single `.git` directory, ensuring synchronized updates across branches.

---

# Thank You!
<br>

**Explore More:**

- [Git Worktree Documentation](https://git-scm.com/docs/git-worktree)
- [Git Kraken Worktree Guide](https://www.gitkraken.com/learn/git/git-worktree)
- [Git Worktree Cheat Sheet](https://gitlab.com/-/snippets/3631442)
<br>

**Slides:** <https://worktree.gorecki.cc/>

<br>

**Questions?**