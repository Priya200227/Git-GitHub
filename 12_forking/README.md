# Forking

A fork is a personal copy of another GitHub repository under my own GitHub account.

Forking is commonly used when I want to contribute to a repository where I do not have direct write access.

---

## Fork Workflow

```text
Original Repository
        |
       Fork
        |
        ↓
My GitHub Repository
        |
      Clone
        |
        ↓
My Local Repository
```

I can then make changes locally and push them to my fork.

---

## Typical Open Source Workflow

```text
Fork repository
      ↓
Clone fork
      ↓
Create branch
      ↓
Make changes
      ↓
Commit
      ↓
Push
      ↓
Create Pull Request
      ↓
Original repository
```

This allows me to contribute changes without directly modifying the original repository.

---

## Clone My Fork

After creating a fork on GitHub, I can clone my fork to my local computer:

```bash
git clone <my-fork-url>
```

Move into the project:

```bash
cd project
```

Create a feature branch:

```bash
git switch -c feature/my-change
```

Now I can make changes without working directly on the main branch.

---

## Push Changes

After making changes:

```bash
git add .
git commit -m "Add requested change"

git push -u origin feature/my-change
```

The branch is pushed to my fork on GitHub.

I can then create a Pull Request from my fork to the original repository.

---

## Fork vs Clone

These are different concepts.

### Fork

Creates a copy of a GitHub repository under my own GitHub account.

```text
Original GitHub Repository
          ↓
         Fork
          ↓
My GitHub Repository
```

### Clone

Downloads a repository from a remote location to my local computer.

```text
GitHub Repository
        ↓
       Clone
        ↓
My Local Computer
```

The simple distinction is:

```text
Fork  → GitHub to GitHub

Clone → Remote repository to local computer
```

They are often used together.

---

## `origin` and `upstream`

When I clone my fork, Git normally creates a remote named `origin`.

```text
origin → My fork
```

If I also want to track the original repository, I can add it as another remote. By convention, this remote is often named `upstream`.

```bash
git remote add upstream <original-repository-url>
```

Check the configured remotes:

```bash
git remote -v
```

Conceptually:

```text
upstream → Original repository

origin   → My fork

local    → Repository on my computer
```

---

## Keeping My Fork Updated

The original repository may continue receiving new commits after I create my fork.

I can fetch changes from the original repository:

```bash
git fetch upstream
```

Then I can integrate the appropriate upstream branch into my local work using the workflow required by the project.

The exact branch name and integration method may vary between repositories.

---

## Why Forking Is Useful

Forking is especially useful when contributing to:

- Open-source projects
- Public GitHub repositories
- Projects where I don't have write access
- Projects I want to experiment with independently

A fork gives me my own GitHub copy while preserving a connection to the original project.

---

## Key Commands

```bash
# Clone my fork
git clone <my-fork-url>

# Move into the project
cd project

# Create a feature branch
git switch -c feature/my-change

# Stage changes
git add .

# Commit changes
git commit -m "Add requested change"

# Push the branch to my fork
git push -u origin feature/my-change

# Add the original repository as upstream
git remote add upstream <original-repository-url>

# Check remotes
git remote -v

# Fetch updates from the original repository
git fetch upstream
```

---

## Mental Model

```text
Original Repository
     (upstream)
         |
         | Fork
         ↓
    My GitHub Fork
       (origin)
         |
         | Clone
         ↓
  My Local Repository
         |
         | Create branch
         ↓
     Make changes
         |
         | Commit + Push
         ↓
    My GitHub Fork
         |
         | Pull Request
         ↓
Original Repository
```

> **Key idea:** Forking creates my own GitHub copy of another repository, while cloning creates a local copy on my computer.
