# Pull Requests

A Pull Request (PR) is a GitHub feature used to propose changes from one branch to another.

It allows changes to be reviewed before they are merged.

---

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
          ↓
    Pull Request
          |
       Review
          |
        Merge
          |
          ↓
        main
```

A Pull Request provides a place where contributors can review, discuss, and approve proposed changes before they become part of the target branch.

---

## Creating a Pull Request

After creating commits on a feature branch, push the branch to GitHub:

```bash
git push -u origin feature-dashboard
```

GitHub can then be used to create a Pull Request from:

```text
feature-dashboard
        ↓
       main
```

The source branch contains the proposed changes, while the target branch is the branch that will receive those changes if the Pull Request is merged.

---

## What a Pull Request Contains

A PR can include:

- Title
- Description
- Changed files
- Commit history
- Comments
- Review feedback
- Approval status

The description should explain what was changed and provide any relevant context for the reviewers.

---

## Code Review

Other contributors can inspect the changes and provide feedback.

They may:

- Approve the PR
- Request changes
- Leave comments

Reviewers can inspect the changed files and discuss specific parts of the proposed changes.

---

## Merge

After the required review process is complete, the Pull Request can be merged.

The exact review and merge rules depend on the repository.

For example, a project may require:

- One or more approvals
- Passing automated checks
- All requested changes to be addressed

After merging, the changes become part of the target branch.

---

## Why Pull Requests Matter

Pull Requests provide a structured way to:

- Review code
- Discuss changes
- Catch problems
- Maintain project quality
- Document why changes were made
- Collaborate without directly modifying the shared branch

They also create a record of the proposed change and the discussion around it.

---

## Pull Request Workflow

A typical workflow looks like this:

```text
Create feature branch
        ↓
Make changes
        ↓
Create commits
        ↓
Push branch to GitHub
        ↓
Create Pull Request
        ↓
Code review
        ↓
Address feedback
        ↓
Required checks pass
        ↓
Merge
```

---

## Important Distinction

A Pull Request is a **GitHub collaboration feature**.

It is not a Git command.

Git handles operations such as:

```bash
git commit
git branch
git merge
```

GitHub provides the Pull Request workflow around those Git operations.

---

## Key Commands Used Before a Pull Request

```bash
# Create a feature branch
git switch -c feature-dashboard

# Stage changes
git add .

# Create a commit
git commit -m "Add dashboard"

# Push the branch to GitHub
git push -u origin feature-dashboard
```

After pushing the branch, the Pull Request is created and managed through GitHub.
