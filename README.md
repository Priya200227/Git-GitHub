# Git & GitHub Notes

A practical learning repository covering Git, GitHub, version control, and collaborative development through structured notes, command examples, and hands-on practice.

I created this repository to understand how version control works, how developers manage changes, and how teams collaborate on software projects. Rather than memorizing commands, I focus on understanding **why each command is used, how the workflow works, and when to apply it**.

## 📚 Learning Roadmap

This repository is organized into progressive topics, from Git fundamentals to collaboration and advanced workflows.

| # | Topic | What You'll Learn |
|---|---|---|
| 01 | [Git Fundamentals](./01_git_fundamentals/) | Version control, Git vs. GitHub, repositories, commits, branches, and remotes |
| 02 | [Git Workflow](./02_git_workflow/) | Working tree, staging area, commits, and the Git lifecycle |
| 03 | [Essential Git Commands](./03_essential_git_commands/) | Common commands for inspecting, staging, committing, and reviewing changes |
| 04 | [`.gitignore`](./04_gitignore/) | Ignore rules, `.gitignore`, `.gitkeep`, and repository hygiene |
| 05 | [Undoing Changes](./05_undoing_changes/) | Unstaging files, restoring changes, `git reset`, and `git revert` |
| 06 | [Branching](./06_branching/) | Creating, switching, managing, and deleting branches |
| 07 | [Merging & Conflicts](./07_merging_and_conflicts/) | Fast-forward merges, three-way merges, and conflict resolution |
| 08 | [GitHub Basics](./08_github_basics/) | GitHub repositories and essential platform features |
| 09 | [Remote Repositories](./09_remote_repositories/) | Remote configuration, HTTPS, SSH, fetch, pull, and push |
| 10 | [Collaboration](./10_collaboration/) | Collaborative workflows and sharing changes with other contributors |
| 11 | [Pull Requests](./11_pull_requests/) | Proposing changes, reviewing code, and merging contributions |
| 12 | [Forking](./12_forking/) | Forking repositories and contributing to external projects |
| 13 | [Git Stash](./13_git_stash/) | Temporarily saving and restoring uncommitted changes |
| 14 | [Git Rebase](./14_git_rebase/) | Rewriting commit history and understanding rebase vs. merge |

### Quick References

- [Git & GitHub Cheat Sheet](./CheatSheet.md)
- [Repository Overview](./README.md)

## 🔄 Understanding the Git Workflow

Git separates the files you edit from the changes you stage and the commits you record. Understanding these distinct states is fundamental to using Git effectively.

```text
┌─────────────────────────┐
│      Working Tree       │
│   Create and edit files │
└────────────┬────────────┘
             │
          git add
             │
             ▼
┌─────────────────────────┐
│      Staging Area       │
│ Select changes to commit│
└────────────┬────────────┘
             │
         git commit
             │
             ▼
┌─────────────────────────┐
│    Local Repository     │
│ Commits and Git history │
└────────────┬────────────┘
             │
          git push
             │
             ▼
┌─────────────────────────┐
│    Remote Repository    │
│  e.g. GitHub repository │
└─────────────────────────┘
```

### Bringing changes from a remote repository

```text
Remote Repository
        │
    git fetch
        │
        ▼
Inspect remote updates
        │
   git merge / rebase
        │
        ▼
Current Local Branch
```

`git pull` combines fetching remote updates with integrating them into the current branch, usually through a merge or rebase depending on the configuration and options.

**Important distinctions:**

- `git add` stages changes locally; it does not upload files to GitHub.
- `git commit` records the staged snapshot in the local repository.
- `git push` transfers local commits to a remote repository.
- `git fetch` downloads remote updates without automatically integrating them into the current branch.
- `git pull` fetches and integrates remote updates.

These operations serve different purposes and should not be treated as interchangeable.

## 🧠 Core Concepts

Git is a **distributed version control system** that records project history and enables developers to manage changes over time.

With Git, I can:

- Track changes and review project history.
- Compare different versions of files.
- Create commits that record coherent changes.
- Develop features on separate branches.
- Merge changes and resolve conflicts.
- Recover previous versions when necessary.
- Work locally and synchronize with remote repositories.

GitHub hosts Git repositories and provides collaboration features such as pull requests, issues, code reviews, and repository management.

Git can be used locally without GitHub.

## 🎯 Why I'm Learning Git

Version control is useful beyond traditional software development. As I develop my skills in data analytics and work toward data engineering and AI engineering, Git is relevant to maintaining and collaborating on:

- **Python:** data cleaning, analysis, and automation scripts.
- **SQL:** analytical queries, database transformations, and data models.
- **Data pipelines:** transformation logic and pipeline configuration.
- **Analytics projects:** dashboards, documentation, and reproducible workflows.
- **Machine learning:** source code, experiment configurations, and supporting documentation.
- **Collaboration:** reviewing changes, maintaining project history, and working with other contributors.

The goal is to use Git as part of a reliable development workflow rather than treating it as a collection of commands to memorize.

## 🛠️ Practical Learning Approach

I use this repository as a reference while studying and practicing Git and GitHub.

My approach is to understand each concept, execute the relevant commands, inspect the results, and learn how to handle common mistakes.

A typical development workflow looks like this:

```text
Create or clone a repository
           ↓
Modify project files
           ↓
Inspect changes with git status / git diff
           ↓
Stage selected changes with git add
           ↓
Review the staging area
           ↓
Create a commit with git commit
           ↓
Develop using branches when appropriate
           ↓
Merge changes when ready
           ↓
Push commits to a remote repository
           ↓
Open a pull request when collaboration requires it
           ↓
Review and integrate contributions
```

Not every task requires a new branch or pull request. The appropriate workflow depends on the project and collaboration requirements.

## 📖 Learning Resources

These are the learning resources I used while studying Git and GitHub. I rewrote concepts into my own notes and use practical examples to reinforce my understanding.

### Learning Videos

- [Not Your College — Git & GitHub](https://youtu.be/BnEFaIfcwOU)
- [Ansh Lamba — Git & GitHub](https://youtu.be/Kr8l7rQGwNs)

### Official Documentation

- [Git Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book/en/v2)
- [GitHub Documentation](https://docs.github.com/en/get-started/using-git/about-git)

The official documentation serves as a reference for verifying command behavior and understanding concepts beyond these notes.

---

*These notes are a personal learning resource and are intended to evolve as I practice Git and GitHub. Examples and explanations are for learning purposes and should be checked against the relevant documentation when needed.*
