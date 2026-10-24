# GitHub Basics

> Understand how Git and GitHub work together: Git manages version history, while GitHub hosts Git repositories and provides collaboration tools.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this guide, you should be able to:

- Explain the difference between Git and GitHub.
- Distinguish a local repository from a remote GitHub repository.
- Connect a local repository to a remote using `git remote`.
- Explain what `git push` and `git pull` do.
- Recognize common files and folders in a project repository.
- Use GitHub as a place to publish and collaborate on portfolio projects.

## Contents

- [Git vs. GitHub](#git-vs-github)
- [Local Repository vs. GitHub Repository](#local-repository-vs-github-repository)
- [Create a GitHub Repository](#create-a-github-repository)
- [Connect a Local Repository](#connect-a-local-repository)
- [Push Local Commits](#push-local-commits)
- [Pull Remote Changes](#pull-remote-changes)
- [Typical Repository Structure](#typical-repository-structure)
- [Why GitHub Matters for a Portfolio](#why-github-matters-for-a-portfolio)
- [Typical Local-to-GitHub Workflow](#typical-local-to-github-workflow)
- [Common Mistakes](#common-mistakes)
- [Quick Reference](#quick-reference)
- [Practice Exercise](#practice-exercise)
- [Next Steps](#next-steps)
- [Official References](#official-references)

## Git vs. GitHub

### Git

Git is a distributed version control system that runs on your computer and stores project history. It helps you:

- Track changes.
- Create commits.
- Create and switch branches.
- Compare versions.
- Review project history.
- Work locally without needing to publish every change.

### GitHub

GitHub is a cloud platform for hosting Git repositories and collaborating around them. It provides features such as:

- Repository hosting.
- Collaboration and access controls.
- Code review and pull requests.
- Issues and project planning.
- Project documentation and release information.
- Automation and continuous integration through supported workflows.

Git and GitHub are related, but they are not the same product. Git can be used without GitHub, and GitHub repositories are managed locally through Git or other supported tools.

## Local Repository vs. GitHub Repository

A simplified local-to-remote workflow looks like this:

```text
Your computer
     |
     | Git commands
     v
Local Git repository
     |
     | git push
     v
Remote repository on GitHub
```

Changes made locally do not automatically appear on GitHub. Usually, you stage and commit the intended changes locally, then push the resulting commits to the remote.

The reverse direction also requires an operation to retrieve remote changes. `git pull` fetches remote updates and integrates them into the current local branch according to the configured pull strategy.

## Create a GitHub Repository

A typical workflow for a new project is:

1. Create an empty repository on GitHub, or choose an existing repository you have permission to use.
2. Create or open the local project directory.
3. Initialize a local repository if needed, or clone the existing GitHub repository.
4. Add and commit the intended files locally.
5. Connect the local repository to the GitHub remote if it is not already configured.
6. Push the commits to the appropriate remote branch.

If you create a repository on GitHub with a README, license, or other initial files, its history may already contain a commit. If your local repository also has its own independent initial commit, the histories may be unrelated. For beginners, cloning the GitHub repository first is often the simplest way to avoid that situation.

## Connect a Local Repository

If you already have a local Git repository and an empty GitHub repository, add the remote URL:

```bash
git remote add origin <repository-url>
```

Replace `<repository-url>` with the actual HTTPS or SSH URL from GitHub. The angle brackets indicate a placeholder; do not type them literally.

Check the configured remotes:

```bash
git remote -v
```

`origin` is a conventional remote name, not a special keyword. A repository can have multiple remotes with different names.

If `origin` is already configured, `git remote add origin ...` will report an error rather than replace it. Inspect the existing remote before changing the configuration.

## Push Local Commits

To push the local `main` branch to the remote named `origin`:

```bash
git push origin main
```

This sends commits reachable from the local `main` branch that the remote branch does not yet have, subject to the remote's configuration and your permissions.

If the remote branch is new and you want to set it as the upstream for future pushes and pulls, you can use:

```bash
git push -u origin main
```

These examples assume your branch is named `main` and the remote is named `origin`. Your repository may use different names.

**Important:** `git push` publishes commits. It does not simply upload every modified or untracked file in your working directory. Stage and commit the changes you want to publish first.

## Pull Remote Changes

To fetch and integrate changes from the remote `main` branch into your current local branch:

```bash
git pull origin main
```

This command assumes that you intend to integrate `origin/main` into the branch currently checked out. Check your current branch and repository state first:

```bash
git status
git branch --show-current
```

`git pull` fetches remote updates and then integrates them. Depending on configuration and options, the integration may use a fast-forward, a merge, or a rebase. If local and remote work conflict, you may need to resolve conflicts.

Do not assume that `git pull origin main` automatically switches your local branch to `main`; it operates on the current branch.

## Typical Repository Structure

A general project repository might contain:

```text
project/
├── README.md
├── src/
├── data/
├── notebooks/
├── requirements.txt
└── .gitignore
```

A data analytics project might instead look like:

```text
data-analysis-project/
├── README.md
├── data/
├── notebooks/
├── sql/
├── src/
├── reports/
└── .gitignore
```

These are examples, not required structures. Organize files according to the project's needs. Avoid committing secrets, private data, generated files, or large datasets unless they are appropriate to store in the repository and you have permission to share them.

## Why GitHub Matters for a Portfolio

GitHub can help you publish and maintain:

- Python projects.
- SQL projects.
- Data analysis projects.
- Data engineering projects.
- AI and machine-learning projects.
- Documentation, examples, and learning notes.

For a portfolio, make each repository easy to understand. A useful README should explain the project's purpose, the problem it addresses, the tools used, how to reproduce or run it, and what the results mean. Include only data and credentials that are safe and authorized to share.

## Typical Local-to-GitHub Workflow

```text
Make changes
     |
     v
git status
     |
     v
git add
     |
     v
git commit
     |
     v
git push
     |
     v
GitHub remote
```

To bring remote changes into the local repository:

```text
GitHub remote
     |
     v
git pull
     |
     v
Current local branch
```

These are common workflows. In collaborative projects, you may also fetch updates, review branches, open pull requests, or resolve conflicts before changes are integrated.

## Common Mistakes

- **Treating Git and GitHub as the same thing:** Git is version control; GitHub hosts repositories and provides collaboration features.
- **Forgetting to commit before pushing:** Uncommitted changes are not included in a normal push.
- **Using the wrong remote or branch name:** Check with `git remote -v` and `git branch --show-current`.
- **Assuming `git pull` switches branches:** It integrates remote changes into the current branch.
- **Overwriting remote work without checking:** If the remote already contains commits, understand the history before trying to push.
- **Publishing secrets or private datasets:** Review the repository contents before pushing; `.gitignore` alone is not a security mechanism.
- **Assuming `origin` is mandatory:** It is only the conventional name for a remote.

## Quick Reference

| Goal | Command |
|---|---|
| Add a remote named `origin` | `git remote add origin <repository-url>` |
| List remote names and URLs | `git remote -v` |
| Push local `main` to `origin` | `git push origin main` |
| Push and set upstream | `git push -u origin main` |
| Fetch and integrate remote `main` into the current branch | `git pull origin main` |
| Check current repository state | `git status` |
| Show current branch | `git branch --show-current` |
| View recent commit history | `git log --oneline` |

## Practice Exercise

Use a test repository or a repository you are comfortable experimenting with.

1. Create an empty repository on GitHub. For the simplest first push, avoid initializing it with extra files if your local repository already has its own initial commit.
2. In your local project, run `git status` and confirm the current branch.
3. If the local repository has not been connected to GitHub, add the remote using the repository URL copied from GitHub.
4. Run `git remote -v` to confirm the URL.
5. Create a small, harmless file, stage it, and commit it.
6. Push the correct branch to GitHub. If needed, use `git push -u origin main`, replacing the branch name if necessary.
7. Refresh the GitHub repository page and confirm that the committed file is present.
8. Make a small change on GitHub only if you understand how you will bring it back locally. Then use `git pull` from the intended local branch and inspect the result.

Answer these questions:

- What is the difference between a local commit and a remote push?
- What does the name `origin` mean?
- What branch receives changes when you run `git pull origin main`?
- Why should you inspect the repository before pushing or pulling?

## Next Steps

Continue with:

- [Remote Repositories](../09_remote_repositories/README.md)
- [Collaboration](../10_collaboration/README.md)
- [Pull Requests](../11_pull_requests/README.md)
- [Forking](../12_forking/README.md)

## Official References

- [Git documentation](https://git-scm.com/doc)
- [GitHub Docs: About Git](https://docs.github.com/en/get-started/using-git/about-git)
- [GitHub Docs: Creating a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
- [Git reference: `git remote`](https://git-scm.com/docs/git-remote)
- [Git reference: `git push`](https://git-scm.com/docs/git-push)
- [Git reference: `git pull`](https://git-scm.com/docs/git-pull)
