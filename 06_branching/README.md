# Git Branching

A branch allows me to work on a separate line of development without
directly changing the main branch.

Branches are commonly used for:

-   New features
-   Bug fixes
-   Experiments
-   Different versions of development

------------------------------------------------------------------------

## Why Use Branches?

Suppose my main project is stable:

``` text
main
  |
  A
  |
  B
```

I want to develop a new dashboard feature.

Instead of changing `main` directly, I can create a separate branch:

``` text
main
  |
  A
  |
  B
   \
    C
    |
    D
feature-dashboard
```

I can develop the feature separately and merge it later.

------------------------------------------------------------------------

## View Branches

To see the branches in my local repository:

``` bash
git branch
```

The current branch is marked with `*`.

Example:

``` text
* main
  feature-dashboard
```

------------------------------------------------------------------------

## Create a Branch

To create a new branch:

``` bash
git branch feature-dashboard
```

This creates the branch but does not switch to it.

------------------------------------------------------------------------

## Switch Branches

To switch to an existing branch:

``` bash
git switch feature-dashboard
```

I can switch back to `main` using:

``` bash
git switch main
```

------------------------------------------------------------------------

## Create and Switch to a Branch

A common way to start working on a new feature is:

``` bash
git switch -c feature-dashboard
```

This:

1.  Creates the branch
2.  Switches to the new branch

------------------------------------------------------------------------

## Delete a Branch

After a feature branch has been merged and is no longer needed, I can
delete it:

``` bash
git branch -d feature-dashboard
```

The `-d` option performs a safe deletion and generally prevents deleting
a branch that contains unmerged work.

If I intentionally want to delete a branch containing unmerged changes:

``` bash
git branch -D feature-dashboard
```

> ⚠️ `-D` can remove a branch even when its commits have not been
> merged. Use it carefully.

------------------------------------------------------------------------

## Example Workflow

Suppose I want to build a customer analysis feature.

First, create and switch to a new branch:

``` bash
git switch -c feature/customer-analysis
```

Make changes to the project.

Check the repository:

``` bash
git status
```

Stage the changes:

``` bash
git add .
```

Commit them:

``` bash
git commit -m "Add customer analysis"
```

When I am finished, I can switch back to `main`:

``` bash
git switch main
```

The feature branch can then be merged when the work is ready.

------------------------------------------------------------------------

## Branch Naming

I should use descriptive branch names so that the purpose of the branch
is clear.

Examples:

``` text
feature/dashboard
feature/customer-analysis
feature/sales-report
fix/missing-values
fix/date-format
experiment/new-model
```

A consistent naming convention makes repositories easier to understand.

------------------------------------------------------------------------

## Branches and Commits

A branch is not a completely separate copy of the entire project.

A useful mental model is that a branch is a movable reference to a
commit in Git history.

For example:

``` text
A---B---C  main
     \
      D---E  feature
```

Both branches share commits `A` and `B`, but the `feature` branch
continues with its own commits.

If I create another commit on `main`:

``` text
A---B---C---F  main
     \
      D---E  feature
```

The branches now point to different commits.

------------------------------------------------------------------------

## Merging a Branch

When the feature is complete, I can switch to the branch that should
receive the changes:

``` bash
git switch main
```

Then merge the feature branch:

``` bash
git merge feature/customer-analysis
```

This combines the changes from the feature branch into `main`.

The details of merging and resolving conflicts are covered in the next
topic.

------------------------------------------------------------------------

## Local Branch vs Remote Branch

A branch on my computer is a local branch.

A branch that exists on GitHub is a remote branch.

For example:

``` text
Local:
feature/customer-analysis

Remote:
origin/feature/customer-analysis
```

To publish a local branch to GitHub:

``` bash
git push -u origin feature/customer-analysis
```

The `-u` option sets the upstream relationship, making future pushes and
pulls easier.

------------------------------------------------------------------------

## Checking the Current Branch

I can use:

``` bash
git branch
```

or:

``` bash
git status
```

`git status` will show the branch I am currently working on.

------------------------------------------------------------------------

## Important Mental Model

Think of branches as separate lines of development:

``` text
                 feature
                    |
                    D
                   /
A---B---C---------E---F
         |
        main
```

The important idea is that I can work on a feature without changing the
main branch until I am ready to integrate the work.

------------------------------------------------------------------------

## Typical Feature Branch Workflow

``` text
main
 ↓
Create feature branch
 ↓
Switch to feature branch
 ↓
Make changes
 ↓
Stage changes
 ↓
Commit changes
 ↓
Push branch
 ↓
Create Pull Request
 ↓
Review
 ↓
Merge into main
```

------------------------------------------------------------------------

## Key Commands

``` bash
# View branches
git branch

# Create a branch
git branch <branch-name>

# Switch to a branch
git switch <branch-name>

# Create and switch to a new branch
git switch -c <branch-name>

# Delete a merged branch
git branch -d <branch-name>

# Force delete a branch
git branch -D <branch-name>

# Merge a branch
git switch main
git merge <branch-name>

# Push a new branch to GitHub
git push -u origin <branch-name>
```

------------------------------------------------------------------------

## Key Takeaway

Branches allow me to isolate work, experiment safely, and develop
features independently.

A typical workflow is:

``` text
Create Branch
      ↓
Work
      ↓
Commit
      ↓
Push
      ↓
Pull Request
      ↓
Merge
```

The goal is not just to memorize branch commands, but to understand
**why and when branches are used**.
