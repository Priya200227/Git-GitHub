Undoing Changes in Git

Git allows me to undo different types of changes depending on where
those changes currently exist.

The important thing is to understand the difference between:

Unstaged changes

Staged changes

Committed changes

Pushed changes

Different Git commands are appropriate for each situation.

1. Discard Changes in the Working Directory

If I modify a file but have not staged it yet, I can discard those
changes using:

git restore filename

Example:

git restore analysis.py

This restores the file to its previous committed or staged state.

⚠️ Any uncommitted changes in that file will be lost.

2. Unstage a File

If I already ran:

git add analysis.py

but I don't want the file included in the next commit, I can unstage it:

git restore --staged analysis.py

The changes are not deleted.

They simply move from:

Staging Area
      ↓
Working Directory

The file remains modified, but it is no longer staged for the next
commit.

3. Undo the Most Recent Commit

If I want to undo the latest commit while keeping the changes staged:

git reset --soft HEAD~1

The commit is removed from the current branch history, but the changes
remain staged.

Example:

Before:

Commit A
   ↓
Commit B   ← HEAD


After:

Commit A   ← HEAD

Changes from Commit B remain staged

This can be useful when I committed too early and want to modify or
recreate the commit.

4. git reset

git reset moves the current branch to another commit.

Common forms include:

git reset --soft HEAD~1

git reset HEAD~1

git reset --hard HEAD~1

--soft

Moves the branch pointer but keeps the changes staged.

git reset --soft HEAD~1

Mixed / Default

Unstages the changes but keeps them in the working directory.

git reset HEAD~1

This is the default behavior of git reset.

--hard

Moves the branch pointer and resets the working directory and staging
area to match the target commit.

git reset --hard HEAD~1

⚠️ --hard can permanently discard local work. Use it carefully.

5. git revert

Instead of rewriting existing history, git revert creates a new
commit that reverses the changes introduced by an earlier commit.

git revert <commit-id>

Example:

Commit A
   ↓
Commit B
   ↓
Commit C
   ↓
Revert C
   ↓
New commit that reverses the changes from C

This approach is generally safer for commits that have already been
shared with others because the existing history remains intact.

Reset vs Revert

Command                             Main Purpose

git reset                         Move the branch to another commit
and potentially rewrite local
history

Important Mental Model

Before undoing anything, ask:

Where is the change?

        ↓

Working Directory?
        ↓
Staging Area?
        ↓
Local Commit?
        ↓
Already Pushed?

The location of the change helps determine which Git command is
appropriate.

Quick Reference

Situation                           Command

Discard unstaged changes            git restore filename

Unstage a file                      git restore --staged filename

Undo latest commit and keep changes git reset --soft HEAD~1
staged

Undo latest commit and unstage      git reset HEAD~1
changes

Discard local changes and move      git reset --hard HEAD~1
branch back

Safely undo an existing commit      git revert <commit-id>

Key Commands

# Discard unstaged changes
git restore filename

# Unstage a file
git restore --staged filename

# Undo latest commit, keep changes staged
git reset --soft HEAD~1

# Undo latest commit, keep changes but unstage them
git reset HEAD~1

# Reset everything to the previous commit
git reset --hard HEAD~1

# Create a new commit that reverses an earlier commit
git revert <commit-id>

⚠️ Important

Before using commands such as:

git reset --hard

make sure I understand exactly what will be removed.

A good habit is to check the repository state first:

git status

And inspect the commit history when necessary:

git log --oneline

Rule of thumb: Understand where the change is first, then choose
the command to undo it.
