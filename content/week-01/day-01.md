+++
title = "Day 01 - 15/06/2026"
weight = 1
+++

## Topics Learned

### Git

#### Common Commands

| Command            | Meaning                                        |
| ------------------ | ---------------------------------------------- |
| git init           | Create a new Git repository                    |
| git remote         | Manage connections to remote repositories      |
| git clone          | Copy a remote repository to the local machine  |
| git fetch          | Download remote changes without merging them   |
| git pull           | Download and merge remote changes              |
| git status         | Show the current repository state              |
| git branch         | List, create, or delete branches               |
| git switch         | Move to another branch                         |
| git checkout       | Switch branches or restore files               |
| git add            | Stage changes for the next commit              |
| git commit         | Save staged changes to the repository history  |
| git commit --amend | Update the latest commit                       |
| git push           | Upload local commits to a remote repository    |
| git reset          | Unstage changes or move commit history         |
| git rebase         | Reapply commits on top of another branch       |
| git rebase -i      | Edit, squash, or reorder commits interactively |
| git stash          | Temporarily save uncommitted work              |
| git stash pop      | Restore the latest stashed work                |
| git merge          | Combine changes from another branch            |
| git cherry-pick    | Apply a specific commit to the current branch  |

#### Merge Conflict Handling

| Situation                        | Solution in Code Source Control                                     |
| -------------------------------- | ------------------------------------------------------------------- |
| Keep changes from both branches  | Open the file, edit the conflict manually, then mark it as resolved |
| Keep the current branch version  | Use `Accept Current Change` in the conflict editor                  |
| Keep the incoming branch version | Use `Accept Incoming Change` in the conflict editor                 |
| Cancel the merge                 | Open Source Control, use the `...` menu, then choose `Abort Merge`  |
| Resolve conflicts manually       | Review the marked conflict blocks and keep the correct final code   |

### TypeScript

#### Interface vs Type

- Use `interface` when the main goal is to describe object structure and support inheritance.
- Use `type` when the shape is more complex, such as a union, tuple, primitive alias, or function type.
- Both are valid for object modeling, so choose the one that fits the use case and team convention.

#### Union Type

- A union type allows a value to have more than one possible type.
- It uses the `|` operator.

#### Omit Utility Type

- `Omit` creates a new type by removing one or more properties from an existing type.
- It is useful when reusing a model but hiding fields that are not needed.

#### Extends

- `extends` lets an interface inherit properties from another interface.
- It reduces duplication and keeps related types consistent.

---

### ESLint

#### Purpose of ESLint

- ESLint is a static analysis tool for JavaScript and TypeScript.
- It helps detect errors and warnings before runtime.
- It keeps code aligned with project conventions.

#### Common Errors and Warnings

- `no-unused-vars`: a variable is declared but not used.
- `no-undef`: a variable is used before it is defined.
- `react-hooks/rules-of-hooks`: React Hooks are used in the wrong place.
- `react-hooks/exhaustive-deps`: a `useEffect` dependency is missing.
- `no-magic-numbers`: a hard-coded number is used without clear meaning.

## Lessons Learned

- Avoid **"magic numbers"**.
- Do not commit `node_modules`.
- Understand the difference between merge and rebase.
- Use `git add <file>` instead of `git add .` when possible.

### Key Principles

- Organize `src/` by feature or by file type.
- Keep configuration files at the project root.
- Always add `node_modules/` and `dist/` to `.gitignore`.
- Use clear and descriptive folder names.
- Group related files together for easier navigation.


