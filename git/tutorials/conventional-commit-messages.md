# Writing Effective Commit Messages with Conventional Commits

Clear and structured commit messages are essential for project maintenance and collaboration. **Conventional Commits** help teams write consistent, informative commit messages.

## Anatomy of a Conventional Commit

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Elements:
1. **type**: The kind of change.
   - `feat`: A new feature
   - `fix`: A bug fix
   - `docs`: Documentation changes
   - `style`: Code style (formatting, etc.)
   - `refactor`: Code restructuring without functional changes
   - `test`: Adding or updating tests
   - `chore`: Maintenance tasks
   - `perf`: Performance improvements
   - `build`: Changes to build system or dependencies
   - `ci`: Continuous integration changes
   - `revert`: Reverting previous commit

2. **scope (optional)**: The part of the code affected (e.g., `auth`, `ui`).
3. **description**: Brief summary of the change (max 50 characters, imperative tone).
4. **body (optional)**: Additional details or context.
5. **footer (optional)**: Links to issues or breaking changes (`Fixes #123`).

## Best Practices
- **Imperative tone**: “Add feature” not “Added feature.”
- **Keep it focused**: One change per commit.
- **Use English**: Standard across software projects.
- **Be concise**: Short, clear messages.

## Example Commit Messages

### Feature Addition
```
feat(ui): add dark mode toggle in settings

Users can now switch between light and dark mode from the settings page.
```

### Bug Fix
```
fix(cart): resolve incorrect item count after removal

Fixed issue where the cart's item count was not updating correctly after items were removed.
Fixes #102
```

### Documentation Update
```
docs(README): update installation instructions

Added setup guide for local environment and updated broken links.
```

### Refactoring
```
refactor(auth): optimize token validation logic

Improved readability of token validation without changing functionality.
```

### Performance Improvement
```
perf(api): improve response time by optimizing query

Reduced database query time, improving API response time by 30%.
```

### Tests
```
test(user): add unit tests for authentication flow

Added tests for login and registration functionality.
```

### Build Process
```
build(deps): update lodash to 4.17.21

Upgraded lodash dependency to address security vulnerability.
```

### Continuous Integration
```
ci(travis): add lint checks to the CI pipeline

Integrated eslint checks into the Travis CI pipeline.
```

## Conclusion

By adopting **Conventional Commits**, you can create a clean, consistent commit history that improves project collaboration, automation, and maintenance. Clear commit messages are an investment in your project’s future.

### Read More
---

Open [Conventional Commits](https://conventionalcommits.org/en/v1.0.0/) website.
