# Git Collaboration

> Learn how teams use branches, remote repositories, commits, and pull requests to collaborate safely on a shared Git project.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this section, you should be able to:

- Explain why teams use branches when collaborating.
- Create a feature branch from an up-to-date local `main` branch.
- Stage, commit, and push work to a remote feature branch.
- Describe how pull requests and code reviews fit into a team workflow.
- Apply practical habits for focused commits, focused branches, and secret protection.

## Contents

- [How Git Collaboration Works](#how-git-collaboration-works)
- [Typical Team Workflow](#typical-team-workflow)
- [Feature Branch Workflow](#feature-branch-workflow)
- [Why Branches Help](#why-branches-help)
- [Update Your Local Main Branch](#update-your-local-main-branch)
- [Good Collaboration Practices](#good-collaboration-practices)
- [End-to-End Example](#end-to-end-example)
- [Common Mistakes](#common-mistakes)
- [Quick Reference](#quick-reference)
- [Practice Exercise](#practice-exercise)
- [Next Steps](#next-steps)
- [Official References](#official-references)

---

## How Git Collaboration Works

Git lets contributors work in separate local branches and record changes as commits. A remote repository, such as one hosted on GitHub, provides a shared place to publish branches and exchange work.

A common collaboration workflow uses:

- **Branches** to isolate a feature or fix.
- **Remote repositories** to share commits.
- **Pull requests (PRs)** to propose changes and discuss them.
- **Code reviews** to inspect proposed changes.
- **Merging** to integrate approved changes into a target branch.

A pull request is a review and integration workflow provided by a hosting platform such as GitHub; it is not itself a Git command.

## Typical Team Workflow

Several contributors can create separate branches from a shared base branch:

```text
main
  |
  ├── feature/customer-analysis
  ├── feature/dashboard
  └── fix/date-conversion
```

Each contributor can work on a task independently, commit changes, and push their branch. The team can then review proposed changes before integrating them into the target branch.

Branches reduce unnecessary overlap, but they do **not** guarantee that conflicts will never occur. Conflicts can still happen when different branches change the same or related lines.

## Feature Branch Workflow

### 1. Create a feature branch

Start from the intended base branch, commonly `main`:

```bash
git switch main
git pull
git switch -c feature/customer-analysis
```

This assumes `main` tracks the correct remote branch and that the project's team workflow uses `main` as the base. If your team specifies another base branch or a different update process, follow that guidance.

### 2. Make and inspect changes

Edit the relevant project files, then inspect the working tree:

```bash
git status
git diff
```

`git status` summarizes the working tree and staging area. `git diff` shows unstaged changes; use `git diff --staged` to inspect changes already staged for the next commit.

### 3. Stage and commit the intended changes

```bash
git add path/to/file
git diff --staged
git commit -m "Add customer analysis"
```

Prefer adding the specific files you intend to include rather than automatically staging everything with `git add .`, especially when unrelated or sensitive files may be present.

### 4. Push the feature branch

```bash
git push -u origin feature/customer-analysis
```

Here, `origin` is a conventional remote name, not a reserved keyword. The `-u` option sets the upstream tracking relationship for this branch, making later `git push` and `git pull` commands simpler in common workflows.

### 5. Open a pull request

On GitHub, open a pull request from your feature branch into the intended target branch. Explain what changed, why it changed, and how it was tested. Respond to review feedback and update the branch as needed.

After approval and any required checks, the team can merge the pull request according to its repository rules.

## Why Branches Help

Working directly on a shared `main` branch can make changes harder to isolate, review, and coordinate. Feature branches provide a separate line of work for a particular task.

Examples:

```text
feature/customer-analysis
feature/dashboard
fix/date-conversion
```

Useful branch names are descriptive and consistent with the team's naming convention. Prefixes such as `feature/`, `fix/`, and `docs/` are common conventions, not Git requirements.

A branch is a movable reference to a commit, not a separate copy of the entire project in the way beginners sometimes imagine. Branches are lightweight, and Git records their history through commits.

## Update Your Local Main Branch

Before creating a new feature branch, make sure your local base branch includes the latest changes from its configured upstream:

```bash
git switch main
git pull
git switch -c feature/new-analysis
```

In the usual configuration, `git pull` fetches from the upstream and then integrates the fetched changes into the current branch. The exact integration behavior depends on Git configuration and the command options used.

You can inspect the configured remotes and branch tracking information with:

```bash
git remote -v
git branch -vv
```

If `git pull` reports conflicts, resolve them before starting unrelated work. Do not assume every repository uses `main` as its default branch; follow the team's instructions if the base branch has another name.

## Good Collaboration Practices

### Use meaningful commit messages

Good commit messages describe the change:

```text
Add customer segmentation
Fix date conversion
Update sales dashboard
```

Avoid vague messages:

```text
update
changes
final
test
```

A clear message helps teammates understand the project history. Follow any commit-message convention the repository requires.

### Keep commits focused

A commit should ideally represent one logical change. Focused commits are easier to review, test, understand, and revert.

For example, keep a customer-segmentation change separate from an unrelated dashboard styling change when practical.

### Keep branches focused

A branch should generally represent one feature, bug fix, or task. This makes the pull request easier to understand and reduces unrelated review work.

### Review before pushing

Before committing or pushing, inspect the files and changes you intend to share:

```bash
git status
git diff
git diff --staged
```

Run relevant tests or checks for the project. Do not rely on the fact that a command completed successfully as proof that the change is correct.

### Do not commit secrets

Never commit API keys, passwords, access tokens, private credentials, or other secrets. Keep local secret files such as `.env` out of version control when appropriate, using `.gitignore` and environment variables.

**Important:** `.gitignore` does not stop Git from tracking a file that is already tracked. If a secret has been committed, removing the file in a later commit does not erase it from repository history. Revoke or rotate the exposed credential promptly, then follow the team's incident-response process.

### Communicate and follow repository rules

Agree on branch naming, review requirements, test expectations, and merge strategy. Check the repository's contribution guide and pull-request template if available. Avoid force-pushing to shared branches unless the team explicitly permits it and you understand the impact.

## End-to-End Example

This example creates a feature branch, records a focused change, and publishes it for review. It assumes the remote is named `origin`, the base branch is `main`, and the working tree is ready for the task.

```bash
# Switch to the base branch
git switch main

# Fetch and integrate the latest changes from its upstream
git pull

# Create and switch to a feature branch
git switch -c feature/customer-analysis

# Edit the project files, then inspect the changes
git status
git diff

# Stage only the intended file(s)
git add path/to/file

# Review the staged snapshot
git diff --staged

# Create a commit
git commit -m "Add customer analysis"

# Publish the branch and set its upstream
git push -u origin feature/customer-analysis
```

Then open a pull request on GitHub, select the correct target branch, describe and test the changes, and request review. Once the team's approval and checks are complete, merge the pull request according to project policy.

The example uses `path/to/file` as a placeholder. Replace it with the actual path of a file you changed.

## Common Mistakes

| Mistake | Why it matters | Better practice |
|---|---|---|
| Creating a feature branch from stale `main` | The branch may miss recent team changes | Update the intended base branch first |
| Running `git add .` without checking status | Unrelated or sensitive files may be staged | Stage specific paths and inspect the staged diff |
| Pushing without reviewing changes | Unintended edits may be shared | Check `git status` and `git diff --staged` |
| Treating a pull request as a Git command | PRs are a hosting-platform workflow | Use Git commands to publish branches, then open the PR on the host |
| Assuming branches eliminate conflicts | Overlapping edits can still conflict | Coordinate changes and resolve conflicts carefully |
| Committing a secret and only deleting the file later | The secret may remain in repository history | Revoke or rotate the secret and follow incident-response guidance |
| Force-pushing to a shared branch without coordination | Other contributors' history may be disrupted | Follow team policy and understand the consequences first |

## Quick Reference

| Goal | Command or action |
|---|---|
| Switch to `main` | `git switch main` |
| Update current branch from its upstream | `git pull` |
| Create and switch to a branch | `git switch -c feature/name` |
| Check working-tree state | `git status` |
| Inspect unstaged changes | `git diff` |
| Stage a specific file | `git add path/to/file` |
| Inspect staged changes | `git diff --staged` |
| Commit changes | `git commit -m "Describe the change"` |
| Push a branch and set upstream | `git push -u origin feature/name` |
| Inspect remotes | `git remote -v` |
| Inspect branch tracking | `git branch -vv` |
| Propose changes for review | Open a pull request on GitHub or your hosting platform |

## Practice Exercise

Practice this workflow in a repository where you are allowed to make changes. If you want to avoid changing an existing project, create a separate test repository.

1. Switch to the intended base branch and update it from its upstream.
2. Create a branch named `docs/collaboration-practice`.
3. Make a small documentation change.
4. Use `git status` and `git diff` to inspect it.
5. Stage only the file you changed and inspect it with `git diff --staged`.
6. Commit with a message that describes the change.
7. Push the branch with `git push -u origin docs/collaboration-practice`.
8. If using GitHub, open a pull request and write a short description of the change.
9. After the pull request is merged, switch back to the base branch and update it. Delete the local or remote feature branch only when it is no longer needed and doing so follows the team's workflow.

If you do not have a remote configured, you can still practice the local branch, staging, and commit steps; pushing and opening a pull request require a hosted remote repository.

## Next Steps

Continue to [Pull Requests](../11_pull_requests/README.md) to learn more about proposing, reviewing, and merging changes on GitHub.

Related topics:

- [Remote Repositories](../09_remote_repositories/README.md)
- [GitHub Basics](../08_github_basics/README.md)
- [Git Ignore](../04_gitignore/README.md)

## Official References

- [Git documentation: Branches in a Nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
- [Git documentation: Distributed Workflows](https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows)
- [Git documentation: `git pull`](https://git-scm.com/docs/git-pull)
- [Git documentation: `git push`](https://git-scm.com/docs/git-push)
- [GitHub Docs: About pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [GitHub Docs: About protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches)

