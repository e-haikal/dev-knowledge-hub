# How to Push a New Project to a Git Repository
Pushing a new project to GitHub is a key step in source code management. Here’s how to do it:

## Steps:
1. **Initialize Local Repository:**
   Navigate to your project folder and run:
   ```bash
   git init
   ```

2. **Add Files to Staging Area:**
   Add all files:
   ```bash
   git add .
   ```

3. **Commit Changes:**
   Create a commit with a message:
   ```bash
   git commit -m "Initial commit message"
   ```

4. **Add Remote Repository:**
   Link your local repository to GitHub:
   ```bash
   git remote add origin <repository-URL>
   ```

5. **Push Changes to Remote:**
   Push your changes to GitHub:
   ```bash
   git push -u origin main
   ```

### Example
For a repository at `https://github.com/username/my-project.git`, use:
```bash
git init
git add .
git commit -m "Initial commit message"
git remote add origin https://github.com/username/my-project.git
git push -u origin main
```

### Conclusion
Follow these steps to push a new project to GitHub. Always commit with clear messages and ensure the remote repository is set correctly.