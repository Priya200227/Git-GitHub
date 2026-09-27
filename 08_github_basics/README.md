# GitHub Basics

Git and GitHub are related but different.

---

## Git

Git is a version control system that runs locally on my computer.

It helps me:

- Track changes
- Create commits
- Create branches
- Compare versions
- Manage project history

---

## GitHub

GitHub is a cloud platform built around Git.

It allows me to:

- Host repositories
- Share projects
- Collaborate with others
- Review code
- Create Pull Requests
- Track issues
- Manage project documentation

---

## Local Repository vs GitHub Repository

```text
My Computer
    |
    | Git
    |
Local Repository
    |
    | git push
    ↓
GitHub Repository
```

Changes made locally do not automatically appear on GitHub.

I need to push them to the remote repository.

---

## Creating a GitHub Repository

A typical workflow is:

```text
Create repository on GitHub
        ↓
Connect local repository
        ↓
Commit local changes
        ↓
Push to GitHub
```

---

## Connecting a Local Repository

If I already have a local Git repository, I can connect it to a GitHub repository using:

```bash
git remote add origin <repository-url>
```

Check the configured remote:

```bash
git remote -v
```

The conventional name `origin` usually refers to the main remote repository.

---

## Push

```bash
git push origin main
```

This uploads commits from my local `main` branch to the remote `main` branch on GitHub.

---

## Pull

```bash
git pull origin main
```

This retrieves changes from the remote `main` branch and integrates them into my current local branch.

---

## GitHub Repository Structure

A typical project repository may contain:

```text
project/
│
├── README.md
├── src/
├── data/
├── notebooks/
├── requirements.txt
└── .gitignore
```

The exact structure depends on the type of project.

For example, a data analytics project might contain:

```text
data-analysis-project/
│
├── README.md
├── data/
├── notebooks/
├── sql/
├── src/
├── reports/
└── .gitignore
```

---

## Why GitHub Matters for Me

GitHub is useful for maintaining:

- Python projects
- SQL projects
- Data analysis projects
- Data engineering projects
- AI projects
- Documentation
- Portfolio projects

It also provides a way to demonstrate practical project work and collaborate with other developers.

---

## GitHub and Git Workflow

A simple local-to-GitHub workflow looks like this:

```text
Make changes
     ↓
git status
     ↓
git add
     ↓
git commit
     ↓
git push
     ↓
GitHub
```

To bring changes from GitHub back to my local repository:

```text
GitHub
   ↓
git pull
   ↓
Local Repository
```

---

## Key Commands

```bash
# Connect local repository to GitHub
git remote add origin <repository-url>

# Check remote repository
git remote -v

# Push local commits to GitHub
git push origin main

# Pull changes from GitHub
git pull origin main
```

---

## Important Distinction

Git and GitHub should not be treated as the same thing.

```text
Git
↓
Version control system
↓
Runs locally

GitHub
↓
Cloud platform built around Git
↓
Hosting + collaboration
```

Git handles version control.

GitHub provides a platform for hosting Git repositories and collaborating around them.
