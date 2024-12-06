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
- Existing Solutions: Pros and Cons
- Introduction to Git Worktrees
- Workflows, Commands, and Best Practices
- Workflow Demo

---

# Multi-Branch Development Pain Points
<br>

**Daily tasks require branch switching:**

- Tackling multiple tasks in parallel
- Bug fixes
- Code reviews
- Release preparations

**Problem:** Context switching isn't seamless due to **uncommitted changes**.


---
transition: view-transition
---

# Branch Switching Solutions
<br>

**Git Stash**

- **Advantages:** Easy to use; avoids unnecessary commits.
- **Drawbacks:** Requires remembering stashes; risk of losing or mismanaging them.

<br>

```sh
$ git stash         # Save current changes
$ git stash list    # View saved stashes
$ git stash apply   # Reapply a stash
```

---
transition: view-transition
---

# Branch Switching Solutions
<br>

**Premature Commits**

- **Advantages:** Prevents stash-related issues.
- **Drawbacks:** Pollutes commit history; creates confusion during reviews. Introduces mental load coming up with meaningful commit messages.

<br>

```sh
$ git add --all         # Stage all changes
$ git commit -m "WIP"   # Commit as work-in-progress
```



---

# Branch Switching Solutions
<br>

**Multiple Repository Clones**

- **Advantages:** Eliminates stash/commit drawbacks.
- **Drawbacks:** Disk-heavy, manual syncing each repository is time-consuming.

<br>

<Note>Each clone has its own .git folder and works independently.</Note>

<br>

```sh
$ git clone git@github.com:user/repo.git repo1   # Clone into first directory
$ git clone git@github.com:user/repo.git repo2   # Clone into second directory
```

---
transition: view-transition
---

# Git Worktrees to the Rescue!
<br>

**Definition:**

A **Git Worktree** is an independent working directory associated with a Git repository, allowing you to check out a specific branch or commit in a separate directory without affecting the state of other worktrees.

Each worktree shares the same `.git` repository metadata but operates as its *own isolated environment*, enabling efficient multi-branch development.

<br>

<Note>You’re already using a worktree—the main one by default!</Note>

---

# Git Worktrees to the Rescue!
<br>

**Characteristics:**

- **Shared Repository:** All worktrees share a single `.git` directory, minimizing disk usage.
- **Unified Management:** Fetching git refs in one worktree are reflected across all.
- **Independent Environments:** Each worktree has its own working directory and index.
- **Branch Isolation:** Each worktree operates on its own branch or commit, independent of others.
- **Parallel Development:** Enables simultaneous work on multiple branches without switching.

---

# Git Worktree Commands
<br>

- List Worktrees

```sh
$ git worktree list                # View all existing worktrees and their details.
```

- Add new Worktree

```sh
$ git worktree add [path] [branch] # Create a new worktree with a specified branch.
$ git worktree add  [path] [branch] # Create a new worktree called [new-branch] based on [branch].
```

- Remove existing Worktree

```sh
$ git worktree remove [path]       # Safely delete an existing worktree.
```

- Prune Worktrees

```sh
$ git worktree prune               # Clean up references to removed worktrees. (deleted working directories)
```

<br>
<Note>Explore additional commands like locking, moving, and repairing worktrees in the docs.</Note>

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
- **Isolated Environments:** Each worktree operates in its own separate directory, providing a clean and independent workspace.
- **Parallel Task Management:** Run long-running tests or builds in one worktree while continuing development in another, avoiding downtime.


---

# Git Worktree Caveats
<br>

- **Worktree-Branch Exclusivity:** A branch can only be checked out in one worktree at a time, as changes in one would desynchronize the other.
- **Untracked files are not copied:** When you create a new worktree, it is created from whatever is comitted, so gitignored or uncomitted files are not copied.
- **Setup Overhead:** For projects with expensive setup scripts, creating new Worktrees can slow down workflows and disrupt efficiency.
- **Learning Curve:** May feel complex for beginners or those new to Git Worktrees.
- **Disk Usage:** Can consume significant space for larger projects creating many Worktrees.

---

# Gotta rule them all!
<br>

**Best Practices:**

- **Organize Worktrees in a Central Location:** Keep all Worktrees in a central folder or have clear rules where to put them.
- **Name Worktrees Based on Purpose:** Name directories based on branch purpose (e.g., `feature`, `bugfix`).
- **Prune Regularly to Prevent Clutter:** Prune/Remove unused Worktrees to avoid clutter.


---

# Takeaways
<br>

- **Streamlined Workflows Save Time Daily:** Investing in tools like Git Worktrees simplifies multi-branch development and eliminates repetitive tasks.
- **Perfect for Frequent Branch Switchers:** Ideal for devs frequently switching between branches for tasks like bug fixes, code reviews, or release preparations.
- **Efficient Multi-Branching:** Worktrees enable seamless parallel work on multiple branches, improving productivity and reducing context-switching mental load.
- **Shared Git Metadata:** Worktrees share a single `.git` directory, ensuring synchronized updates across branches.

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

# Thank You!
<br>

**Explore More:**

- [Git Worktree Documentation](https://git-scm.com/docs/git-worktree)
- [Git Kraken Worktree Guide](https://www.gitkraken.com/learn/git/git-worktree)
- [Worktrees: Git's best kept secret (and why you should use them)](https://www.tomups.com/posts/git-worktrees/)

**Slides:**

- <https://worktree.gorecki.cc/>

---
