# Git & GitHub Notes

Personal notes and practical reference for Git and GitHub.

I created this repository while learning Git and GitHub to understand how version control works, how developers manage changes, and how teams collaborate on software projects.

The notes focus on **understanding the workflow and using the commands in practice**, rather than memorizing commands.

---

## 📚 Topics Covered

### Git Fundamentals

* What is Version Control?
* What is Git?
* Git vs GitHub
* Repository
* Commit
* Branch
* Remote
* Clone

### Git Workflow

* Working Directory
* Staging Area
* Git Repository
* Understanding how changes move through Git

### Essential Commands

* `git init`
* `git status`
* `git add`
* `git commit`
* `git log`
* `git diff`

### Repository Management

* `.gitignore`
* `.gitkeep`
* Keeping sensitive and unnecessary files out of repositories

### Undoing Changes

* Unstaging files
* Discarding local changes
* `git reset`
* `git revert`
* Understanding `--soft`, default/mixed, and `--hard`

### Branching

* Why branches are used
* Creating branches
* Switching branches
* Deleting branches
* Working with feature branches

### Merging

* Fast-forward merge
* Three-way merge
* Merge conflicts
* Conflict resolution

### GitHub

* GitHub vs Git
* Creating repositories
* Connecting local and remote repositories
* HTTPS and SSH
* Push and pull

### Collaboration

* Remote branches
* Pull Requests
* Forking repositories
* Contributing to projects

### Advanced Git

* `git stash`
* `git rebase`
* Understanding when to use rebase vs merge

---

## 🔄 Core Git Workflow

```text
              Git
              │
              ▼
       Working Directory
              │
          git add
              │
              ▼
        Staging Area
              │
        git commit
              │
              ▼
       Local Repository
              │
         git push
              │
              ▼
           GitHub
```

To bring changes from GitHub back to the local repository:

```text
GitHub
   │
git pull
   │
   ▼
Local Repository
```

---

## 🧠 Key Idea

Git is a **version control system**.

It allows me to:

* Track changes
* Create snapshots of my work
* Compare versions
* Work on features independently
* Recover previous versions
* Collaborate with other developers

GitHub is a platform that hosts Git repositories online and provides collaboration features such as Pull Requests, Issues, code review, and repository management.

---

## 🛠️ Why I'm Learning Git

Git is not only useful for software developers.

As I move toward **Data Analytics, Data Engineering, and AI Engineering**, I need version control for:

* Python projects
* SQL projects
* Data pipelines
* Analytics projects
* Machine learning projects
* AI applications
* Configuration files
* Documentation
* Collaboration

---

## 📂 Practical Goal

I am using these notes as a reference while building and maintaining my own projects.

Instead of only memorizing commands, I want to be comfortable with the complete workflow:

```text
Create
  ↓
Modify
  ↓
Check changes
  ↓
Stage
  ↓
Commit
  ↓
Branch
  ↓
Merge
  ↓
Push
  ↓
Pull Request
  ↓
Collaborate
```

---

## 📖 Learning Source

I used learning material from the **Not Your College — Git & GitHub** learning resource and rewrote the concepts into my own notes while studying and practicing.

Original resource:

https://41chaitanya.github.io/git_github_notes/

YouTube channel:

https://www.youtube.com/@notyourcollege

---

## 🚧 Status

**Learning + Practicing**

This repository will be updated as I learn more Git, GitHub, and software development workflows.
