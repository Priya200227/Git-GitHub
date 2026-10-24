# `.gitignore` and Clean Repositories

> Keep generated files, local configuration, and other intentionally excluded files out of Git—without confusing ignore rules with secret protection.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this guide, you should be able to:

- Explain what `.gitignore` does and which files it affects.
- Create practical ignore rules for Python and data-analysis projects.
- Understand common pattern rules and exceptions.
- Explain the role of `.gitkeep` in keeping an otherwise empty directory visible in a repository.
- Recognize why `.gitignore` is not a security mechanism.

## Contents

- [What Is `.gitignore`?](#what-is-gitignore)
- [Why a Clean Repository Matters](#why-a-clean-repository-matters)
- [Create a `.gitignore` File](#create-a-gitignore-file)
- [Common Ignore Patterns](#common-ignore-patterns)
- [Python Project Example](#python-project-example)
- [Data Project Example](#data-project-example)
- [What `.gitignore` Does Not Do](#what-gitignore-does-not-do)
- [If a File Is Already Tracked](#if-a-file-is-already-tracked)
- [What Is `.gitkeep`?](#what-is-gitkeep)
- [Safe Habits for Secrets and Data](#safe-habits-for-secrets-and-data)
- [Practice Exercise](#practice-exercise)
- [Quick Reference](#quick-reference)
- [Next Steps](#next-steps)
- [Official References](#official-references)

## What Is `.gitignore`?

A `.gitignore` file tells Git which **untracked files** and paths to ignore when looking for files to add to the repository. It helps keep files that do not belong in version control out of your commits.

For example:

```gitignore
.venv/
__pycache__/
.env
*.csv
```

These patterns ignore virtual-environment directories, Python cache directories, a file named `.env`, and CSV files matched by the pattern. Be careful with broad rules such as `*.csv`: they may also match a CSV file that the project actually needs to version-control.

## Why a Clean Repository Matters

A repository should contain the files needed to understand, reproduce, review, or maintain the project—not every local or generated file.

Common candidates for exclusion include:

- API keys, passwords, and local environment configuration.
- Virtual environments and package caches.
- Python bytecode and temporary files.
- Notebook checkpoints and generated outputs.
- Large raw datasets or locally generated processed data, when those files are not intended to be version-controlled.
- Build outputs, logs, and editor-specific files.

Ignoring a file is a repository-management choice, not a universal rule. For example, a small, legally shareable sample dataset may be essential to a project and worth tracking, while a large private dataset may belong in approved storage outside Git.

## Create a `.gitignore` File

Create a file named `.gitignore` in the repository's root directory. Add one pattern per line.

Example project layout:

```text
my-project/
├── .gitignore
├── README.md
├── analysis.py
├── .env
├── .venv/
└── __pycache__/
```

With suitable rules in `.gitignore`, Git can ignore `.env`, `.venv/`, and `__pycache__/` while continuing to track the README and Python source file.

You can check repository status with:

```bash
git status
```

Ignored files normally do not appear in the ordinary untracked-files list. To ask Git why a particular path is ignored, use:

```bash
git check-ignore -v .env
```

This reports the matching ignore rule, when one applies.

## Common Ignore Patterns

| Pattern | Meaning |
|---|---|
| `.env` | Matches a path named `.env` according to the pattern's location and matching rules. |
| `.venv/` | Matches a directory named `.venv`. |
| `__pycache__/` | Matches Python cache directories with this name. |
| `*.pyc` | Matches paths ending in `.pyc`. |
| `*.csv` | Matches CSV files according to Git's pattern rules; it may be broader than intended. |
| `data/raw/` | Matches that directory path relative to the `.gitignore` location. |
| `*.log` | Matches log files ending in `.log`. |
| `!data/sample.csv` | Negates an earlier matching rule for this path, subject to Git's parent-directory rules. |

A few useful pattern rules:

- A trailing `/` makes the pattern match a directory.
- A leading `/` anchors a pattern to the directory containing that `.gitignore` file.
- A pattern containing a slash is interpreted relative to the `.gitignore` file's location.
- `*` matches characters within a path component; it does not match a slash.
- A leading `!` re-includes a path that was ignored by an earlier pattern, but Git cannot re-include a file if a parent directory remains excluded.

Ignore rules can also come from `.gitignore` files in subdirectories, repository-specific `.git/info/exclude`, and a user's global excludes file. For team projects, put project-wide rules in the repository's `.gitignore`.

## Python Project Example

A useful starting point for many Python projects is:

```gitignore
# Virtual environments
.venv/
venv/

# Python cache and compiled files
__pycache__/
*.py[cod]

# Local environment variables and secrets
.env
.env.*

# Jupyter Notebook checkpoints
.ipynb_checkpoints/

# Logs
*.log

# Common editor/OS files
.DS_Store
Thumbs.db
```

Adapt the rules to the project. For example, if the project intentionally includes a safe example environment file, keep a clearly non-secret template such as `.env.example` tracked. If you use `.env.*`, add an exception for the template:

```gitignore
.env
.env.*
!.env.example
```

Only include placeholder values in `.env.example`; never put real credentials in it.

## Data Project Example

Data projects often need more deliberate rules because datasets may be large, private, licensed, or reproducible from another source.

For example:

```gitignore
# Optional: ignore local datasets if they should not be version-controlled
data/raw/
data/processed/

# Optional: ignore common spreadsheet and CSV files
*.csv
*.xlsx
```

**Do not copy these broad data rules blindly.** They would also ignore useful project files such as a small sample CSV or an explicitly required spreadsheet. Choose rules based on the project's data policy, licensing, privacy requirements, file size, and reproducibility needs.

A practical alternative is to ignore a particular data directory rather than every CSV file:

```gitignore
data/raw/
data/processed/
```

Then keep documentation or a small, approved sample dataset in a separate location that is not ignored.

## What `.gitignore` Does Not Do

`.gitignore` is **not a security mechanism**.

- It does not encrypt files.
- It does not remove a file that is already tracked.
- It does not erase content from existing commits.
- It does not stop someone from explicitly staging an ignored file with `git add -f`.
- It does not replace access controls, secret management, or careful review.

If a secret has already been committed and pushed, adding its filename to `.gitignore` does not make the exposed credential safe. Revoke or rotate the credential immediately, assess where it may have been exposed, and follow the relevant incident-response process. Removing the secret from Git history may also be necessary, but rewriting history does not undo exposure and requires care when collaborating.

## If a File Is Already Tracked

Ignore rules generally affect untracked files. If Git is already tracking a file, adding it to `.gitignore` will not stop Git from tracking its changes.

For example, if `data.csv` is tracked and you decide it should no longer be tracked in future commits, you can remove it from the index while keeping your local copy:

```bash
git rm --cached data.csv
```

Then add an appropriate ignore rule to `.gitignore` and commit the change:

```bash
git add .gitignore
git commit -m "Stop tracking local data file"
```

Use this only when you intend to remove that file from the repository going forward. Other collaborators may see the file removed from their working tree when they update to the commit, so communicate the change when necessary. This command does not remove the file from previous commits.

## What Is `.gitkeep`?

Git does not normally record an empty directory by itself. A common convention is to place a placeholder file named `.gitkeep` inside a directory that should exist in the repository even when it contains no other files.

Example:

```text
data/
└── raw/
    └── .gitkeep
```

`.gitkeep` is **not a special Git command or built-in feature**. It is an ordinary filename used by convention. Teams can use any placeholder filename they agree on, and the file can contain a short note explaining the directory's purpose.

## Safe Habits for Secrets and Data

- Keep real credentials in a secure secret manager or local environment configuration that is not committed.
- Add relevant secret/configuration patterns to `.gitignore` before creating or staging those files.
- Review `git status` and staged changes before each commit.
- Avoid broad patterns unless you understand which files they will match.
- Keep data only when you have permission to store and share it in the repository.
- Track a small sample dataset only when it is safe to share and useful for reproducing the project.
- If a secret is exposed, rotate or revoke it; do not rely on ignoring or deleting the file alone.

## Practice Exercise

In a disposable test repository:

1. Create a `.gitignore` file.
2. Add these rules:

   ```gitignore
   .venv/
   __pycache__/
   .env
   *.log
   ```

3. Create a file named `.env`, a file named `debug.log`, and a regular file named `README.md`.
4. Run `git status`. Identify which files appear as untracked and which are ignored.
5. Run `git check-ignore -v .env` and `git check-ignore -v debug.log`.
6. Track and commit a harmless test file, then add a matching ignore rule for it. Observe that the already-tracked file remains tracked.
7. In the disposable repository only, use `git rm --cached` to remove that file from the index, then confirm the effect with `git status`.

Do not put real API keys, passwords, or private data in this exercise.

Questions to answer:
- Why does `.gitignore` not remove an already-tracked file?
- Why might `*.csv` be too broad for a data-analysis project?
- What does `.gitkeep` do—and what does it not do?
- If a real API key is pushed to GitHub, why is adding `.env` to `.gitignore` not enough?

## Quick Reference

| Goal | What to use |
|---|---|
| Ignore a local virtual environment | `.venv/` in `.gitignore` |
| Ignore Python cache files | `__pycache__/` and `*.py[cod]` |
| Ignore local environment configuration | `.env` and suitable related patterns |
| Check why a path is ignored | `git check-ignore -v <path>` |
| Inspect repository state | `git status` |
| Inspect staged content | `git diff --staged` |
| Stop tracking a file but keep the local copy | `git rm --cached <file>` |
| Keep an otherwise empty directory represented | Add a conventional placeholder such as `.gitkeep` |
| Respond to an exposed credential | Revoke/rotate it and follow incident-response steps |

## Next Steps

Continue with:

- [Undoing Changes](../05_undoing_changes/README.md)
- [Git Workflow](../02_git_workflow/README.md)
- [Remote Repositories](../09_remote_repositories/README.md)

## Official References

- [Git documentation](https://git-scm.com/doc)
- [Git reference: `gitignore`](https://git-scm.com/docs/gitignore)
- [Git reference: `git check-ignore`](https://git-scm.com/docs/git-check-ignore)
- [Git reference: `git rm`](https://git-scm.com/docs/git-rm)
