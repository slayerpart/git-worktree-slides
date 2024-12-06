---
title: "Stash No More"
subtitle: "Streamlining Multi-Branch Development with Git Worktrees"
theme: academic
highlighter: shiki
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-up
mdc: true
layout: cover
---

# Stash No More

Streamlining Multi-Branch Development with Git Worktrees

```sh
$ git stash clear
```

---
layout: default
---

# Agenda
<br>

- Identify challenges in multi-branch development workflows.
- Discuss pros and cons of existing solutions.
- Understand Git Worktrees to create your own efficient workflows.

---

# Multi-Branch Development Pain
<br>

**Daily tasks require branch switching:**

- Tackling multiple tasks in parallel
- Bug fixes
- Code reviews
- Release preparations

**Problem:** Context switching isn't seamless due to uncommitted changes.


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
$ git worktree add -b [new-branch] [path] [branch] # Create a new worktree called [new-branch] based on [branch].
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
<Note>Explore additional commands like locking, moving, and repairing worktrees in the docs.
</Note>

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

# What Makes Worktrees Superior

- Seamless context switching.
- Saves disk space compared to cloning.
- Boosts productivity by simplifying branch management.

<!-- Visual: Infographic comparing Worktrees vs. other methods. -->

---

# Challenges to Consider

- Increased complexity for new users.
- Disk space usage for large projects.
- Setup overhead for projects with complex environments.

<!-- Visual: A balance scale showing pros vs. cons. -->

---

# Gotta rule them all!
<br>

**Best Practices:**

- **Organized Directory Structure:** Keep all worktrees in a central folder or have clear rules where to put them.
- **Consistent Naming:** Name directories based on branch purpose (e.g., `feature-branch`, `bugfix`).
- **Regular Maintenance:** Prune unused worktrees to avoid clutter.


---

# Why Worktrees?

- A powerful solution for multi-branch challenges.
- Enables efficient and structured workflows.
- Perfect for frequent branch switchers (ditch `git stash` and premature commits).

---
layout: center
---

# Personal Workflow Demo

<!-- 
# Real-Life Example: My Setup

- Git bare repo with three linked worktrees:
  - `master`: Production-ready.
  - `feature`: Development branch.
  - `stale`: Temporary work.
- **TMUX** for easy terminal multiplexing.

-->

<!-- Visual: Screenshot of TMUX with multiple worktrees open. -->

---

# Learn More and Experiment

**Additional Resources:**

- [Git Worktree Documentation](https://git-scm.com/docs/git-worktree)

**Call to Action:**

- Try Git Worktrees in your next project.
- Explore ways to tailor Worktrees to your workflow.


---

# Let's Discuss

- Open the floor for audience questions.
- Invite feedback and suggestions for using Git Worktrees.

---