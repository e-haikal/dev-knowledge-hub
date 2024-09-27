
# How to Remove All Uncommitted Changes in Git

If you want to reset all uncommitted changes in Git, follow these steps:

1. View Repository Status
2. Remove Changes in Unstaged Files
3. Remove Changes in Staged Files
4. Remove All Changes, Including New Files
5. Remove Untracked Files and Folders

### 1. View Repository Status

Before resetting, check the status of the repository to see what changes exist:

```bash
git status
```

### 2. Remove Changes in Unstaged Files

To discard changes in files that are not yet staged (not added to the staging area), use:

```bash
git checkout -- .
```

### 3. Remove Changes in Staged Files

To remove changes in files that are already staged, use the following command:

```bash
git reset HEAD .
```

### 4. Remove All Changes, Including New Files

To remove all changes, including newly added files that are not yet committed, run:

```bash
git reset --hard HEAD
```

> **Note**: The `git reset --hard HEAD` command will discard all local changes that are not committed, and they cannot be recovered.

### 5. Remove Untracked Files and Folders

To remove untracked files and directories, use this command:

```bash
git clean -fd
```

- `-f` (force): Confirms that you want to delete the untracked files.
- `-d`: Deletes untracked directories as well.

### Example Usage

**View Repository Status:**

```bash
git status
```

```bash
# On branch main
# Your branch is up to date with 'origin/main'.
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#        app/src/main/res/color/
#        app/src/main/res/menu/
#
# nothing added to commit but untracked files present (use "git add" to track)
```

**Remove Untracked Files and Folders:**

```bash
git clean -fd
```

**Verify the Cleanup:**

```bash
git status
```

```bash
# On branch main
# Your branch is up to date with 'origin/main'.
#
# nothing to commit, working tree clean
```

By using these commands, you can clean up all uncommitted changes and restore your Git repository to its last committed state.

--- 