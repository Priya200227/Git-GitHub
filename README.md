# Git & GitHub Notes

A practical, hands-on learning repository covering Git, GitHub, version control, and collaborative development. It is organized into 14 topic folders, with command examples, workflow diagrams, exercises, and a quick-reference cheat sheet.

The goal is to understand **why** a command is used, **what** it changes, and **when** it belongs in a real development workflow—not simply to memorize syntax.

## Learning Roadmap

Work through the topics in order, or use the links below to revisit a specific concept.

| # | Topic | What you'll learn |
|---:|---|---|
| 01 | [Git Fundamentals](01_git_fundamentals/README.md) | Version control, Git vs. GitHub, repositories, commits, branches, and remotes |
| 02 | [Git Workflow](02_git_workflow/README.md) | Working tree, staging area, commits, and the Git lifecycle |
| 03 | [Essential Git Commands](03_essential_git_commands/README.md) | Commands for inspecting, staging, committing, and reviewing changes |
| 04 | [`.gitignore`](04_gitignore/README.md) | Ignore rules, `.gitignore`, `.gitkeep`, and repository hygiene |
| 05 | [Undoing Changes](05_undoing_changes/README.md) | Unstaging files, restoring changes, `git reset`, and `git revert` |
| 06 | [Branching](06_branching/README.md) | Creating, switching, managing, and deleting branches |
| 07 | [Merging and Conflicts](07_merging_and_conflicts/README.md) | Fast-forward merges, three-way merges, and conflict resolution |
| 08 | [GitHub Basics](08_github_basics/README.md) | GitHub repositories and essential platform features |
| 09 | [Remote Repositories](09_remote_repositories/README.md) | Remote configuration, fetch, pull, push, and remote-tracking branches |
| 10 | [Collaboration](10_collaboration/README.md) | Feature-branch workflows and collaborating with other contributors |
| 11 | [Pull Requests](11_pull_requests/README.md) | Proposing changes, reviewing code, checks, and merging contributions |
| 12 | [Forking](12_forking/README.md) | Forks, clones, `origin`, `upstream`, and contributing to external projects |
| 13 | [Git Stash](13_git_stash/README.md) | Temporarily saving and restoring uncommitted changes |
| 14 | [Git Rebase](14_git_rebase/README.md) | Replaying commits onto a new base and understanding rebase vs. merge |

## Quick References

- [Git & GitHub Cheat Sheet](CheatSheet.md) — common commands, short explanations, and safety reminders.

## Understanding the Git Workflow

Git separates editing files, choosing which changes to include in a commit, and recording that snapshot in the repository.

```text
Working Tree
    │  Edit files
    │
    │  git add
    ▼
Staging Area
    │  Review selected changes
    │
    │  git commit
    ▼
Local Repository
    │  Local commit history
    │
    │  git push
    ▼
Remote Repository
    e.g. GitHub
```

### Bringing changes from a remote

```text
Remote Repository
       │
       │  git fetch
       ▼
Remote-tracking references
       │
       │  Inspect changes, then merge or rebase as appropriate
       ▼
Current Local Branch
```

`git pull` fetches remote updates and then integrates them into the current branch. The integration behavior depends on Git configuration and command options.

Keep these distinctions clear:

- `git add` stages selected changes locally; it does not upload them to GitHub.
- `git commit` records the staged snapshot in local Git history.
- `git push` transfers local commits to a remote repository.
- `git fetch` downloads remote updates without automatically integrating them into the current branch.
- `git pull` fetches and integrates remote updates.

## Core Concepts

Git is a distributed version control system. It records project history and helps developers manage changes over time.

With Git, you can:

- Track changes and inspect project history.
- Compare file versions.
- Record coherent changes as commits.
- Develop features on separate branches.
- Merge changes and resolve conflicts.
- Recover from some mistakes using appropriate undo operations.
- Work locally and synchronize with remote repositories.

GitHub hosts Git repositories and adds collaboration features such as pull requests, issues, code reviews, and repository management. Git itself can be used locally without GitHub.

## Why Git Is Useful

Version control is valuable across software development, data analytics, data engineering, and AI/ML projects. It can help manage:

- **Python:** data cleaning, analysis, and automation scripts.
- **SQL:** analytical queries, transformations, and data models.
- **Data pipelines:** transformation logic and pipeline configuration.
- **Analytics projects:** dashboards, documentation, and reproducible workflows.
- **Machine learning:** source code, experiment configuration, and supporting documentation.
- **Collaboration:** reviewing changes, maintaining history, and coordinating contributions.

Avoid committing datasets, generated outputs, credentials, or other sensitive or bulky files unless the project specifically requires them and they are appropriate to store in version control.

## A Practical Learning Approach

For each topic:

1. Read the concept and understand the purpose of the commands.
2. Practise in a test repository or a branch where you can safely experiment.
3. Inspect the result with commands such as `git status`, `git diff`, and `git log`.
4. Learn what can go wrong and how to recover safely.
5. Apply the workflow to a small project of your own.

A typical feature-branch workflow looks like this:

```text
Create or clone a repository
             ↓
Create a feature branch
             ↓
Modify project files
             ↓
Inspect with git status / git diff
             ↓
Stage selected changes with git add
             ↓
Review with git diff --staged
             ↓
Record a commit with git commit
             ↓
Push the branch to a remote
             ↓
Open a pull request when collaboration calls for it
             ↓
Review, check, and integrate the changes
```

Not every task requires a new branch or a pull request. Follow the project's contribution guidelines and collaboration requirements.

## Learning Resources

These resources complement the notes. Use official documentation to verify command behavior and explore concepts in more depth.

### Official documentation

- [Git documentation](https://git-scm.com/docs)
- [Pro Git book](https://git-scm.com/book/en/v2)
- [GitHub documentation](https://docs.github.com/)

### Learning videos

- [Not Your College — Git & GitHub](https://www.youtube.com/results?search_query=Not+Your+College+Git+GitHub)
- [Ansh Lamba — Git & GitHub](https://www.youtube.com/results?search_query=Ansh+Lamba+Git+GitHub)

## Repository Structure

```text
Git-GitHub/
├── 01_git_fundamentals/
├── 02_git_workflow/
├── 03_essential_git_commands/
├── 04_gitignore/
├── 05_undoing_changes/
├── 06_branching/
├── 07_merging_and_conflicts/
├── 08_github_basics/
├── 09_remote_repositories/
├── 10_collaboration/
├── 11_pull_requests/
├── 12_forking/
├── 13_git_stash/
├── 14_git_rebase/
├── CheatSheet.md
└── README.md
```

---

These notes are a personal learning resource intended to evolve with continued practice. Use the examples as a starting point, inspect the effects of commands, and consult the official documentation when you need details beyond this guide.
