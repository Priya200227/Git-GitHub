# Pull Requests

> Learn how pull requests propose changes, support code review, and help teams integrate work into a target branch on GitHub.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this section, you should be able to:

- Explain what a pull request (PR) is and how it differs from a Git command.
- Identify the source branch and target branch of a pull request.
- Push a feature branch to GitHub and open a PR.
- Understand the role of descriptions, changed files, comments, reviews, and automated checks.
- Describe what happens when a PR is merged.

## Contents

- [What Is a Pull Request?](#what-is-a-pull-request)
- [Typical Pull Request Workflow](#typical-pull-request-workflow)
- [Source Branch and Target Branch](#source-branch-and-target-branch)
- [Creating a Pull Request](#creating-a-pull-request)
- [What a Pull Request Contains](#what-a-pull-request-contains)
- [Code Review](#code-review)
- [Checks and Merge](#checks-and-merge)
- [Why Pull Requests Matter](#why-pull-requests-matter)
- [Commands Used Before a Pull Request](#commands-used-before-a-pull-request)
- [Common Mistakes](#common-mistakes)
- [Quick Reference](#quick-reference)
- [Practice Exercise](#practice-exercise)
- [Next Steps](#next-steps)
- [Official References](#official-references)

---

## What Is a Pull Request?

A **pull request (PR)** is a GitHub feature used to propose changes from one branch into another branch. It provides a place for contributors to review, discuss, and approve proposed changes before they are merged.

A PR is not a Git command. Git manages operations such as creating branches, committing changes, and merging histories. GitHub provides the pull-request interface and collaboration workflow around those Git operations.

## Typical Pull Request Workflow

```text
main
  |
  └── feature branch
          |
        commits
          |
        git push
          |
          v
     Pull Request
          |
        Review
          |
     Checks / feedback
          |
        Merge
          |
          v
         main
```

This is a simplified example. In a real project, the target branch might not be named `main`, and teams may require different reviews or checks before merging.

## Source Branch and Target Branch

A pull request compares a **source branch** with a **target branch**:

```text
feature-dashboard  ──────►  main
     source                 target
```

- **Source branch:** contains the proposed commits and changes.
- **Target branch:** the branch into which the proposed changes are intended to be integrated.

Check the source and target carefully before opening a PR. Selecting the wrong target can propose changes against an unintended branch.

## Creating a Pull Request

### 1. Commit your changes

Create a feature branch and commit the intended work. For example:

```bash
git switch -c feature-dashboard

# Make your changes, then inspect them
git status
git diff

# Stage the intended file(s)
git add path/to/file
git diff --staged

# Commit the changes
git commit -m "Add dashboard"
```

`path/to/file` is a placeholder; replace it with the actual path. Stage only the files you intend to include. If you are working in an existing repository, create your branch from the team's intended base branch.

### 2. Push the branch to GitHub

```bash
git push -u origin feature-dashboard
```

This publishes the branch to the remote named `origin` and sets its upstream tracking branch. `origin` is a common default remote name, not a special Git keyword.

### 3. Open the pull request on GitHub

On GitHub, open the repository and use the pull-request workflow to compare your source branch with the intended target branch. Review the comparison, select the correct base/target branch, and create the PR.

Include a clear title and description. Explain what changed, why it was needed, and how you tested it. Link related issues when appropriate and follow any contribution guidelines in the repository.

## What a Pull Request Contains

Depending on the repository and hosting interface, a PR can show:

- **Title:** a concise summary of the proposed change.
- **Description:** context, purpose, implementation details, and testing notes.
- **Changed files:** the diff showing what changed.
- **Commits:** the commits included in the proposal.
- **Comments and review feedback:** discussion about the changes.
- **Review status:** approvals or requests for changes.
- **Automated checks:** results from configured tests, builds, linting, or other workflows.

The exact interface and available fields can vary with repository settings and GitHub features.

## Code Review

Reviewers inspect the proposed changes and may:

- Approve the pull request.
- Request changes.
- Leave general or line-specific comments.
- Ask questions or suggest alternatives.

As the author, read feedback carefully, make appropriate updates, and push new commits to the same source branch. Those commits normally update the existing pull request automatically.

As a reviewer, focus on correctness, clarity, maintainability, tests, security, and whether the change meets the task's requirements. Be specific and respectful when leaving feedback.

An approval does not necessarily mean the PR can be merged immediately: repository rules may require additional approvals, successful checks, or other conditions.

## Checks and Merge

A team may require conditions such as:

- One or more approving reviews.
- Passing automated checks.
- Resolution of requested changes or review conversations.
- Compliance with branch-protection or repository rules.

The exact requirements depend on the repository's configuration and team policy.

When the PR is merged, its changes are integrated into the target branch. GitHub supports different merge methods, such as creating a merge commit, squashing commits, or rebasing and merging, depending on repository settings. These methods can produce different commit histories.

After merging, contributors commonly switch back to the target branch and update it:

```bash
git switch main
git pull
```

Use the actual target branch name if it is not `main`. Delete a feature branch only when it is no longer needed and doing so follows the team's workflow.

## Why Pull Requests Matter

Pull requests provide a structured way to:

- Review proposed changes before integration.
- Discuss implementation choices.
- Catch defects and missing tests.
- Maintain project quality and consistency.
- Record the context and rationale behind a change.
- Coordinate work without everyone directly modifying the shared branch.

A PR supports collaboration, but it does not automatically guarantee quality. Meaningful review, appropriate testing, and clear communication are still necessary.

## Commands Used Before a Pull Request

```bash
# Create and switch to a feature branch
git switch -c feature-dashboard

# Inspect the working tree and unstaged changes
git status
git diff

# Stage the intended file(s), not necessarily everything
git add path/to/file

# Review the staged snapshot
git diff --staged

# Create a commit
git commit -m "Add dashboard"

# Push the branch to GitHub and set its upstream
git push -u origin feature-dashboard
```

After pushing, create and manage the pull request through GitHub. There is no built-in `git pull-request` command in standard Git.

## Common Mistakes

| Mistake | Why it matters | Better practice |
|---|---|---|
| Choosing the wrong target branch | The PR may propose changes against the wrong branch | Verify source and target before creating it |
| Pushing without reviewing the diff | Unintended changes may be included | Inspect `git status` and `git diff --staged` |
| Staging everything automatically | Unrelated or sensitive files may be included | Stage specific paths when practical |
| Writing an unclear PR description | Reviewers lack context | Explain purpose, changes, and testing |
| Assuming approval means all requirements are met | Required checks or other approvals may remain | Check the PR's status and repository rules |
| Treating a PR as a Git command | PRs are part of the hosting platform's workflow | Push with Git, then open the PR on GitHub |
| Assuming every merge uses the same strategy | Repositories can configure different merge methods | Follow the project's contribution guidance |

## Quick Reference

| Goal | Command or action |
|---|---|
| Create a feature branch | `git switch -c feature-dashboard` |
| Check working-tree status | `git status` |
| Inspect unstaged changes | `git diff` |
| Stage a specific file | `git add path/to/file` |
| Inspect staged changes | `git diff --staged` |
| Commit changes | `git commit -m "Add dashboard"` |
| Push and set upstream | `git push -u origin feature-dashboard` |
| Open a PR | Use the pull-request interface on GitHub |
| Review a PR | Inspect the diff, discussion, reviews, and checks |
| Update local `main` after merge | `git switch main` followed by `git pull` |

## Practice Exercise

Use a repository where you are allowed to make changes. You can use a test repository if you do not want to change an existing project.

1. Create a branch named `docs/pr-practice` from the appropriate base branch.
2. Make a small documentation change.
3. Inspect the changes with `git status` and `git diff`.
4. Stage only the file you changed and inspect it with `git diff --staged`.
5. Commit the change with a descriptive message.
6. Push the branch with `git push -u origin docs/pr-practice`.
7. On GitHub, create a pull request into the intended target branch.
8. Write a title and description that explain the change and how you checked it.
9. Inspect the changed-files view and review the PR's status.
10. If you have permission and the repository's rules allow it, merge the PR. Otherwise, leave it open for review.

If no remote is configured, you can practise branch, staging, and commit steps locally, but pushing and opening a PR require a remote repository hosted on a platform such as GitHub.

## Next Steps

Continue to [Forking](../12_forking/README.md) to learn how contributors work with their own copies of repositories when they do not have direct write access.

Related topics:

- [Collaboration](../10_collaboration/README.md)
- [Remote Repositories](../09_remote_repositories/README.md)
- [GitHub Basics](../08_github_basics/README.md)

## Official References

- [GitHub Docs: About pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)
- [GitHub Docs: Creating a pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request)
- [GitHub Docs: About pull request reviews](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews)
- [Git documentation: `git push`](https://git-scm.com/docs/git-push)
- [Git documentation: `git diff`](https://git-scm.com/docs/git-diff)
