# Git Collaboration

Git and GitHub allow multiple people to work on the same project without directly editing the same codebase at the same time.

A common collaboration workflow uses:

- Branches
- Remote repositories
- Pull Requests
- Code reviews
- Merging

---

## Typical Team Workflow

```text
main
  |
  ├── Developer A → feature branch
  |
  ├── Developer B → feature branch
  |
  └── Developer C → feature branch
```

Each developer can work independently on their own branch and later propose their changes for review.

---

## Feature Branch Workflow

A developer can create a branch:

```bash
git switch -c feature/customer-analysis
```

Make changes:

```bash
git add .
git commit -m "Add customer analysis"
```

Push the branch:

```bash
git push -u origin feature/customer-analysis
```

Then create a Pull Request on GitHub.

The Pull Request allows the changes to be reviewed before they are merged into the target branch.

---

## Why Branches Help

Without branches, multiple people modifying the same `main` branch can create unnecessary conflicts and make it harder to review individual changes.

Branches allow work to be isolated.

For example:

```text
main
  |
  ├── feature/customer-analysis
  |
  ├── feature/dashboard
  |
  └── fix/date-conversion
```

Each branch can contain work related to a specific feature or fix.

---

## Pull Before Starting Work

Before beginning new work, I should make sure my local `main` branch is up to date.

For example:

```bash
git switch main
git pull
```

Then create a new feature branch:

```bash
git switch -c feature/new-analysis
```

This gives me a new branch based on the latest version of `main`.

---

## Good Collaboration Practices

### Use Meaningful Commit Messages

Good commit messages describe what changed:

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

Clear commit messages make the project history easier to understand.

---

### Keep Commits Focused

A commit should ideally represent one logical change.

For example:

```text
Add customer segmentation
```

is more useful than combining unrelated changes into one commit.

---

### Keep Branches Focused

A feature branch should generally contain work related to one feature, bug fix, or specific task.

Examples:

```text
feature/customer-analysis
feature/dashboard
fix/missing-values
```

---

### Do Not Commit Secrets

Never commit sensitive information such as:

```text
API keys
Passwords
Access tokens
.env files
Private credentials
```

Use `.gitignore` and environment variables to keep sensitive information out of the repository.

---

## Collaboration Flow

```text
Pull latest main
       ↓
Create feature branch
       ↓
Make changes
       ↓
Commit
       ↓
Push branch
       ↓
Create Pull Request
       ↓
Review
       ↓
Merge
       ↓
Delete branch
```

---

## Example End-to-End Workflow

```bash
# Switch to main
git switch main

# Get the latest changes
git pull

# Create a feature branch
git switch -c feature/customer-analysis

# Make changes to the project

# Check changes
git status

# Stage changes
git add .

# Create a commit
git commit -m "Add customer analysis"

# Push the feature branch
git push -u origin feature/customer-analysis
```

After pushing the branch, I can create a Pull Request on GitHub.

---

## Important Mental Model

The basic collaboration workflow is:

```text
Local main
    ↓
Pull latest changes
    ↓
Create feature branch
    ↓
Work independently
    ↓
Commit changes
    ↓
Push feature branch
    ↓
Pull Request
    ↓
Code Review
    ↓
Merge
    ↓
Updated main
```

The goal is to keep the shared branch stable while allowing different contributors to work independently.
