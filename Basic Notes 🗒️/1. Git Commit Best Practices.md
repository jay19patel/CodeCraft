# Git Commit Best Practices

## Why Good Commit Messages Matter
Writing clear and concise commit messages helps maintain a clean and understandable project history. It is essential when collaborating with other developers and for future reference.

---

## Commit Message Structure
A good commit message should consist of three parts:

1. **Title (Header)** - A short summary of the changes (50 characters or less).
2. **Description (Body)** - More detailed explanation of the changes (optional, but recommended).
3. **Footer** - Any issue references or breaking changes (optional).

### Example Format
```
type: short description

Optional detailed explanation, wrapped at 72 characters. Provide context for why the change was made.

Issue Reference: #1234
Breaking Change: changes behavior of X
```

---

## Common Commit Types
- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation only changes
- **style**: Changes that do not affect the meaning of the code (e.g., formatting)
- **refactor**: Code change that neither fixes a bug nor adds a feature
- **perf**: A code change that improves performance
- **test**: Adding or correcting tests
- **chore**: Changes to the build process or auxiliary tools and libraries
- **wip**: Work in progress (not ready for production)

### Example Commits
```
feat: add user login functionality
fix: correct typo in README
refactor: optimize database query performance
wip: initial setup for analytics integration
```

---

## Writing Better Commit Messages
- Use the imperative mood (e.g., "fix bug" not "fixed bug").
- Be concise but descriptive.
- Capitalize the first letter of the title.
- Use the body to explain why a change was made, not what was changed.
- Separate the title from the body with a blank line.

---

## Basic Git Commands

### Staging and Committing
- **git add .** - Stages all changes in the current directory.
- **git add [file]** - Stages a specific file.
- **git commit -m "message"** - Commits staged changes with a message.
- **git commit -am "message"** - Adds and commits tracked files in one step.

### Viewing Changes
- **git status** - Shows the status of your working directory.
- **git log** - Displays the commit history.
- **git diff** - Shows differences between your working directory and the index.

### Undoing Changes
- **git restore [file]** - Discards changes in the working directory.
- **git reset [file]** - Unstages changes but leaves them in the working directory.
- **git revert [commit]** - Reverts a specific commit by creating a new one.
- **git reset --hard [commit]** - Resets the repository to a specific commit (dangerous).

### Branching and Merging
- **git branch** - Lists all branches.
- **git checkout -b [branch]** - Creates and switches to a new branch.
- **git merge [branch]** - Merges a branch into the current one.
- **git rebase [branch]** - Reapplies commits on top of another base tip.

### Syncing Changes
- **git pull** - Fetches changes from a remote and merges them.
- **git push** - Pushes changes to a remote repository.
- **git fetch** - Downloads objects and refs from another repository.

---

## References
- [Conventional Commits](https://www.conventionalcommits.org)
- [Git Best Practices](https://git-scm.com/book/en/v2)

