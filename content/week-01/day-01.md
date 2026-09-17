
+++
title = "Day 01 - 15/09/2026 (ON-SITE) Git Report"
weight = 1
+++

### Report DAY 1

### NOTE:

Working with Git:

- Creating a repository with `git init` and connecting it to a remote with `git remote`.
- Copying an existing repository with `git clone`, and synchronizing changes with `git fetch` and `git pull`.
- Checking the working tree with `git status` and managing branches with `git branch`, `git switch`, and `git checkout`.
- Preparing changes with `git add`, saving them with `git commit`, and correcting the latest commit with `git commit --amend`.
- Uploading changes with `git push` and undoing or reorganizing commits with `git reset` and `git rebase`.
- Using interactive rebase with `git rebase -i` to review and clean up commit history.
- Temporarily saving unfinished work with `git stash` and restoring it with `git stash pop`.
- Combining work from different branches with `git merge` and applying a specific commit with `git cherry-pick`.

### IMPORTANT NOTE:

Always fetch and pull the latest code to avoid conflicts, and check the current branch before making changes.

### COMMON CONFLICT RESOLUTION:

When Git reports a conflict, first check the repository state:

```bash
git status
```

Open the affected files and resolve the sections between the conflict markers. Keep or combine the correct code, remove the markers, review the result with `git diff`, and stage the resolved files with `git add`.

#### 1. Uncommitted changes (`git stash`)

```bash
git stash
git pull
```

#### 2. Restoring stashed changes (`git stash pop`)

```bash
git stash pop
```

If `git stash pop` causes a conflict, resolve it and run `git add .`, `git commit -m "Resolve stash conflict"`, and `git push`.

#### 3. Committed but not pushed (`git pull --rebase`)

```bash
git pull --rebase
# resolve the conflict
git add .
git rebase --continue
git push
```

Abort with `git rebase --abort` if necessary.

#### 4. Merging branches (`git merge`)

```bash
git switch main
git merge <branch-name>
# resolve the conflict
git add .
git commit
git push
```

Abort with `git merge --abort`.

#### 5. Applying a commit (`git cherry-pick`)

```bash
git cherry-pick <commit-id>
# resolve the conflict
git add .
git cherry-pick --continue
```

Abort with `git cherry-pick --abort`.

#### 6. Rebasing a branch (`git rebase`)

```bash
git switch <branch-name>
git rebase main
# resolve the conflict
git add .
git rebase --continue
```

Repeat if another conflict occurs. Abort with `git rebase --abort`.

#### 7. Reverting a commit (`git revert`)

```bash
git revert <commit-id>
# resolve the conflict
git add .
git revert --continue
git push
```

Abort with `git revert --abort`.

#### 8. Applying a patch (`git apply`)

If a patch cannot be applied completely, Git may create a `.rej` file instead of conflict markers. Review the rejected hunk, apply the required changes manually, remove the `.rej` file when it is no longer needed, then run:

```bash
git add .
git commit -m "Apply patch"
```

### PRATICE

#### 1. `git init`

![git init](/images/report/day-01/git_init_remote_branch_add.png)

#### 2. `git remote`

![git remote](/images/report/day-01/git_init_remote_branch_add.png)

#### 3. `git clone`

![git fetch](/images/report/day-01/git_clone.png)

#### 4. `git fetch`

![git fetch](/images/report/day-01/git_fetch.png)

#### 5. `git pull`

![git fetch](/images/report/day-01/git_pull.png)

#### 6. `git status`

![git status](/images/report/day-01/git_status_switch.png)

#### 7. `git branch`

![git branch](/images/report/day-01/git_status_switch.png)

#### 8. `git switch`

![git switch](/images/report/day-01/git_status_switch.png)

#### 9. `git checkout`

![git checkout](/images/report/day-01/git_push_checkout.png)

#### 10. `git add`

![git add](/images/report/day-01/git_init_remote_branch_add.png)

#### 11. `git commit`

![git commit](/images/report/day-01/git_commit.png)

#### 12. `git commit --amend`

![git commit --amend](/images/report/day-01/git_commit_--amend.png)

#### 13. `git push`

![git push](/images/report/day-01/git_push_checkout.png)

#### 14. `git reset`

![git reset](/images/report/day-01/git_reset.png)

#### 15. `git rebase`

![git rebase](/images/report/day-01/git_rebase.png)

#### 16. `git rebase -i`

![git rebase](/images/report/day-01/github_rebase-i.png)

#### 17. `git stash`

![git rebase](/images/report/day-01/git_stash_stashpop.png)

#### 18. `git stash pop`

![git rebase](/images/report/day-01/git_stash_stashpop.png)

#### 19. `git merge`

![git merge](/images/report/day-01/conflic.png)

#### 20. `git cherry-pick`

![git cherry-pick](/images/report/day-01/git_cherry.png)
