## Essential Commands

## Creating a New Repository

```markdown
git init
git add-all
git commit -m "Initial commit"
git remote add origin https://github.com/<user>/<repo>.git
git push -u origin master
```

## Working with Local Changes

```markdown
git status
git add --all
git add -p <file>
git commit -m "Your message here"
```

## Viewing History

```markdown
git log
git log -p <file>
git blame <file>
```

## Branching and Merging

```markdown
git branch -av
git branch <new-branch>
git checkout <branch>
git merge <branch>
```

## Undoing Changes

```markdown
git reset --hard HEAD
git checkout HEAD <file>
git revert <commitId>
```

## Additional Essential Commands

### Cloning a Repository

```markdown
git clone https://github.com/<user>/<repo>.git
```

### Pulling Changes from Remote

```markdown
git pull origin master
```

### Pushing Changes to Remote

```markdown
git push origin <branch>
```

### Stashing Changes

```markdown
git stash           # Stash changes
git stash list      # List all stashes
git stash apply     # Apply the latest stash
git stash drop      # Drop the latest stash
```

### Tagging

```markdown
git tag <tag-name>          # Create a new tag
git tag                     # List all tags
git push origin <tag-name>  # Push tag to remote
```

### Viewing Differences

```markdown
git diff        # Show changes between working directory and index
git diff HEAD   # Show changes between working directory and last commit
```

### Deleting Branches

```markdown
git branch -d <branch>  # Delete a local branch (only if merged)
git branch -D <branch>  # Force delete a local branch (even if unmerged)
```

### Working with Remotes

```markdown
git remote -v                   # List all remote repositories
git remote add <name> <url>     # Add a new remote repository
git remote remove <name>        # Remove a remote repository
```
## See more

Open [Git-scm documentations](https://git-scm.com/docs).
