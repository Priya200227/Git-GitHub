# Remote Repositories

A remote repository is another copy of a Git repository, usually hosted on a platform such as GitHub.

The local repository and remote repository can exchange commits.

---

## Local vs Remote

```text
Local Repository
      |
      | git push
      ↓
Remote Repository
      |
      | git pull
      ↓
Local Repository
```

The local repository contains my local Git history and work.

The remote repository provides a shared location where the repository can be stored and accessed by other contributors.

---

## Check Remotes

```bash
git remote -v
```

Example:

```text
origin  https://github.com/user/project.git (fetch)
origin  https://github.com/user/project.git (push)
```

This shows the remote name and the URL used for fetching and pushing.

---

## Add a Remote

```bash
git remote add origin <repository-url>
```

`origin` is the conventional name for the primary remote repository.

Example:

```bash
git remote add origin https://github.com/user/project.git
```

---

## Push

```bash
git push origin main
```

This pushes local commits from the `main` branch to the remote `main` branch.

---

## Set Upstream Branch

```bash
git push -u origin main
```

The `-u` option sets the upstream relationship between the local `main` branch and the remote `main` branch.

After setting the upstream relationship, future pushes can often be simplified to:

```bash
git push
```

---

## Fetch

```bash
git fetch
```

`git fetch` downloads information about changes from the remote repository without automatically integrating those changes into my current branch.

This allows me to inspect the remote changes before deciding what to do with them.

---

## Pull

```bash
git pull
```

`git pull` generally performs:

```text
git fetch
+
integration of the fetched changes
```

It retrieves changes from the remote repository and integrates them into the current branch.

---

## Clone

To get an existing remote repository:

```bash
git clone <repository-url>
```

This creates a local copy of the repository.

Example:

```bash
git clone https://github.com/user/project.git
```

After cloning, I can move into the project directory:

```bash
cd project
```

---

## Important Difference

### `git fetch`

Downloads remote changes but does not automatically integrate them into my current branch.

```text
Remote Repository
       ↓
   git fetch
       ↓
Local Repository
```

I can inspect the fetched information before integrating it.

### `git pull`

Fetches remote changes and integrates them into the current branch.

```text
Remote Repository
       ↓
   git pull
       ↓
Local Branch
```

---

## Push vs Fetch vs Pull

| Command | Purpose |
|---|---|
| `git push` | Upload local commits to a remote repository |
| `git fetch` | Download remote updates without integrating them |
| `git pull` | Download remote updates and integrate them |
| `git clone` | Create a local copy of a remote repository |

---

## Common Workflow

```bash
# Get information about remote changes
git fetch

# Check repository status
git status

# Bring remote changes into the current branch
git pull

# Make changes

git add .
git commit -m "Update project"

# Upload commits to the remote repository
git push
```

---

## Remote Workflow Mental Model

```text
                Remote Repository
                 /            \
                /              \
           git fetch          git push
              ↓                  ↑
             Local Repository
                  ↓
               git pull
                  ↓
             Local Branch
```

The important idea is:

> A remote repository is a separate copy of the Git repository that can exchange commits with my local repository.
