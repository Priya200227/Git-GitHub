# Git Stash

> Learn how to temporarily save uncommitted changes, switch context, and safely restore work with `git stash`.

[← Back to the main learning roadmap](../README.md)

## Learning Objectives

By the end of this section, you should be able to:

- Explain when `git stash` is useful.
- Save and list stashes.
- Restore changes with `git stash pop` or `git stash apply`.
- Create named stashes and remove stash entries deliberately.
- Understand important limitations, including untracked files and possible conflicts.

## Contents

- [Why Use Stash?](#why-use-stash)
- [Stash Changes](#stash-changes)
- [View Stashes](#view-stashes)
- [Restore Stashed Changes](#restore-stashed-changes)
- [Apply Without Removing](#apply-without-removing)
- [Include Untracked Files](#include-untracked-files)
- [Name a Stash](#name-a-stash)
- [Delete Stashes](#delete-stashes)
- [Example Workflow](#example-workflow)
- [Common Mistakes](#common-mistakes)
- [Quick Reference](#quick-reference)
- [Practice Exercise](#practice-exercise)
- [Next Steps](#next-steps)
- [Official References](#official-references)

---

## Why Use Stash?

`git stash` temporarily stores selected local changes so you can switch tasks without creating a commit for work that is not ready to be recorded in the branch history.

Suppose you are working on:

```text
feature-dashboard
```

You have made changes, but they are not ready to commit. An urgent task requires you to switch branches. You can stash your changes, switch to the other branch, and return to the original work later.

```text
Working tree changes
        |
        | git stash
        v
   Stash storage
        |
        v
Working tree cleaned of stashed changes
```

By default, `git stash` saves changes to tracked files in the working tree and index. It does **not** include untracked files unless you ask it to. It also does not stash ignored files by default.

Stashing is temporary local storage, not a substitute for a durable commit or backup.

## Stash Changes

To stash tracked-file changes:

```bash
git stash
```

Git saves the changes it stashes and restores the affected tracked files to a clean state relative to the current `HEAD`. You can then switch branches, for example:

```bash
git switch main
```

Before switching, check your repository state:

```bash
git status
```

If you have new, untracked files that must be saved too, see [Include Untracked Files](#include-untracked-files).

## View Stashes

List the saved stash entries:

```bash
git stash list
```

Example output:

```text
stash@{0}: WIP on feature-dashboard: ...
stash@{1}: WIP on main: ...
```

The newest stash is normally `stash@{0}`. When you create another stash, the numbering can change, so inspect the list before applying or deleting a particular entry.

You can also inspect a stash's summary or diff:

```bash
git stash show stash@{0}
git stash show -p stash@{0}
```

The `-p` option displays the patch. Stash contents may include sensitive work, so inspect them carefully before sharing output.

## Restore Stashed Changes

Use `git stash pop` to apply the most recent stash and remove its entry if the application succeeds:

```bash
git stash pop
```

If the changes apply cleanly, they return to your working tree. If conflicts occur, Git may leave the stash entry in the stash list so you can resolve the conflict and avoid losing the saved version. Check the command output and run `git status`.

## Apply Without Removing

Use `git stash apply` to restore the most recent stash while keeping the stash entry:

```bash
git stash apply
```

To apply a particular stash, specify its reference:

```bash
git stash apply stash@{1}
```

This is useful when you want to preserve the saved entry while checking or reusing its changes. Applying the same stash more than once can produce conflicts or duplicate edits, depending on the state of your working tree.

## Include Untracked Files

By default, `git stash` does not save untracked files. To include untracked files, use:

```bash
git stash push -u -m "Dashboard work in progress"
```

The `-u` option includes untracked files but does not include ignored files. To include ignored files as well, Git provides `-a`:

```bash
git stash push -a -m "Save tracked, untracked, and ignored changes"
```

Use `-a` carefully: ignored files can include large build outputs, local configuration, or secrets. Do not stash or share sensitive data unnecessarily.

## Name a Stash

Add a message to make a stash easier to identify:

```bash
git stash push -m "Dashboard work in progress"
```

List stashes to see the message:

```bash
git stash list
```

Example:

```text
stash@{0}: On feature-dashboard: Dashboard work in progress
```

The message is a label for humans; it does not change what the stash contains.

## Delete Stashes

To remove the latest stash entry without applying it:

```bash
git stash drop
```

To remove a specific entry:

```bash
git stash drop stash@{1}
```

Dropping a stash discards that stash reference. Make sure you no longer need its contents before removing it.

To remove all stash entries:

```bash
git stash clear
```

Use `git stash clear` with particular care because it removes all stash entries from the stash list. Do not use it as routine cleanup unless you are certain nothing needs to be recovered.

## Example Workflow

```bash
# Work on a feature and make local edits
git status

# Save tracked changes with a descriptive message
git stash push -m "Dashboard work in progress"

# Switch to another branch for an urgent task
git switch main

# Complete the urgent task, then return to the feature
git switch feature-dashboard

# Restore the latest stash and remove its entry if successful
git stash pop

# Inspect the result
git status
```

If your work includes untracked files, use `git stash push -u -m "Dashboard work in progress"` instead of the basic stash command.

## `git stash pop` vs. `git stash apply`

| Command | Applies changes | Removes stash entry after successful application |
|---|---|---|
| `git stash pop` | Yes | Yes |
| `git stash apply` | Yes | No |

Both commands can encounter conflicts. If a stash cannot be applied cleanly, inspect `git status`, resolve conflicts carefully, and check whether the stash entry remains before attempting another operation.

## Common Mistakes

| Mistake | Why it matters | Better practice |
|---|---|---|
| Assuming untracked files are included by default | New files may remain in the working tree | Use `git stash -u` when untracked files should be included |
| Using `pop` without checking the result | Conflicts may leave changes unresolved | Run `git status` and inspect the command output |
| Dropping a stash too early | The saved changes may be difficult to recover | Inspect and verify the restored work before dropping entries |
| Assuming stash is a permanent backup | Stashes are local and can be removed | Commit important work or use an appropriate backup |
| Applying a stash repeatedly without checking state | Changes may conflict or be duplicated | Inspect the working tree and stash list first |
| Stashing ignored files without considering their contents | Large or sensitive local files may be included with `-a` | Use `-a` only when necessary and inspect the result |

## Quick Reference

| Goal | Command |
|---|---|
| Stash tracked-file changes | `git stash` |
| Create a named stash | `git stash push -m "Message"` |
| Include untracked files | `git stash push -u -m "Message"` |
| Include ignored files too | `git stash push -a -m "Message"` |
| List stash entries | `git stash list` |
| Show a stash summary | `git stash show stash@{0}` |
| Show the stash patch | `git stash show -p stash@{0}` |
| Apply and remove latest stash if successful | `git stash pop` |
| Apply latest stash and keep its entry | `git stash apply` |
| Apply a specific stash | `git stash apply stash@{1}` |
| Drop latest stash entry | `git stash drop` |
| Drop a specific stash entry | `git stash drop stash@{1}` |
| Remove all stash entries | `git stash clear` |

## Practice Exercise

Use a test repository or a branch where you can safely make temporary changes.

1. Create a branch named `practice/stash`.
2. Edit a tracked file and save the change without committing it.
3. Run `git status` and inspect the diff.
4. Create a named stash with `git stash push -m "Practice stash"`.
5. Confirm that the working tree is clean and run `git stash list`.
6. Restore the stash with `git stash apply` and inspect the result.
7. Stash the changes again, then restore them with `git stash pop`.
8. Create a new untracked file and verify that plain `git stash` does not include it.
9. Use `git stash push -u -m "Include untracked practice file"` to stash tracked and untracked changes, then inspect the stash list.
10. Drop only the practice stash entries after you have verified that you no longer need them.

Be careful not to drop or clear stash entries containing real work.

## Next Steps

Continue to [Git Rebase](../14_git_rebase/README.md) to learn how rebasing replays commits onto another base and how it differs from merging.

Related topics:

- [Undoing Changes](../05_undoing_changes/README.md)
- [Branching](../06_branching/README.md)
- [Remote Repositories](../09_remote_repositories/README.md)

## Official References

- [Git documentation: `git stash`](https://git-scm.com/docs/git-stash)
- [Git documentation: `git switch`](https://git-scm.com/docs/git-switch)
- [Git documentation: `git status`](https://git-scm.com/docs/git-status)
