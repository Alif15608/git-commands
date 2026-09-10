# Git & GitHub Complete Guide for Full-Stack Developers and DevOps Engineers

> A practical command reference for daily development, GitHub repository management, CI/CD, access control, releases, troubleshooting, and DevOps workflows.
>
> **Important:** Git is the version-control system. GitHub is the hosting/collaboration platform. `gh` is the GitHub CLI. Many GitHub organization/admin operations are easier through the GitHub web UI or `gh api`.

---

## 1. The mental model you should understand first

```text
Working directory
      |
      | git add
      v
Staging area / Index
      |
      | git commit
      v
Local repository
      |
      | git push
      v
GitHub remote repository
```

And when getting changes:

```text
GitHub remote
      |
      | git fetch
      v
Remote-tracking branches
      |
      | git merge / git rebase / git pull
      v
Local branch
```

### The five concepts you must know

| Concept | Meaning | Typical example |
|---|---|---|
| Working tree | Files currently on your computer | You edit `app.py` |
| Staging area | Changes selected for the next commit | `git add app.py` |
| Commit | Permanent snapshot in local Git history | `git commit -m "Fix login"` |
| Branch | Movable pointer to a line of development | `feature/login` |
| Remote | A named Git server/repository | `origin` |

---

# 2. Git installation and first-time configuration

| Command | Why / what it does | When you use it |
|---|---|---|
| `git --version` | Shows installed Git version | Verify Git is installed |
| `git help` | Opens Git help | When learning Git |
| `git help <command>` | Shows detailed help for a command | When unsure about options |
| `git <command> -h` | Short command help | Quick terminal reference |
| `git config --list` | Shows Git configuration | Diagnose configuration |
| `git config --global user.name "Your Name"` | Sets commit author name globally | First-time setup |
| `git config --global user.email "you@example.com"` | Sets commit email globally | First-time setup |
| `git config --global init.defaultBranch main` | Makes new repositories use `main` | Recommended setup |
| `git config --global core.editor "code --wait"` | Sets default Git editor | If Git opens an unwanted editor |
| `git config --global pull.rebase true` | Makes pull use rebase by default | If your team follows a rebase workflow |
| `git config --global fetch.prune true` | Removes stale remote-tracking branches after fetch | Useful for long-lived repos |
| `git config --show-origin --list` | Shows where configuration values came from | Debug configuration conflicts |

### Local repository configuration

| Command | Why / what it does | When you use it |
|---|---|---|
| `git config user.name "Name"` | Sets author name for only the current repo | Different work/personal identity |
| `git config user.email "work@example.com"` | Sets email for only the current repo | Work repository |
| `git config --local --list` | Shows current repository config | Inspect repo settings |

---

# 3. Create or obtain a repository

| Command | Why / what it does | When you use it |
|---|---|---|
| `git init` | Creates a new Git repository | Starting version control in an existing project |
| `git init -b main` | Creates repo with `main` as initial branch | New projects |
| `git clone <url>` | Downloads a repository and its history | Joining an existing project |
| `git clone <url> <folder>` | Clones into a specific directory | Organizing projects |
| `git clone --branch <branch> <url>` | Clones and checks out a specific branch | Working directly on a target branch |
| `git clone --depth 1 <url>` | Shallow clone with limited history | CI/CD or very large repos |
| `git clone --recurse-submodules <url>` | Clones repo plus submodules | Projects using Git submodules |

---

# 4. Check repository state

| Command | Why / what it does | When you use it |
|---|---|---|
| `git status` | Shows changed, staged, and untracked files | Use constantly |
| `git status -sb` | Compact branch/status view | Fast daily check |
| `git branch` | Lists local branches | See your branches |
| `git branch -a` | Lists local and remote branches | Understand available branches |
| `git branch -vv` | Shows tracking relationship and ahead/behind state | Diagnose branch synchronization |
| `git remote -v` | Shows remote URLs | Verify where push/pull go |
| `git remote show origin` | Shows detailed remote information | Diagnose remote tracking |
| `git log` | Shows commit history | Understand history |
| `git log --oneline` | Compact history | Daily history review |
| `git log --oneline --graph --decorate --all` | Visual branch/history graph | Very useful for complex repositories |
| `git show <commit>` | Shows a commit and its changes | Investigate a specific commit |
| `git rev-parse --show-toplevel` | Shows repository root directory | Scripts and troubleshooting |

---

# 5. Stage and commit changes

| Command | Why / what it does | When you use it |
|---|---|---|
| `git add <file>` | Stages one file | Commit selected changes |
| `git add .` | Stages changes under current directory | Quick staging; review first |
| `git add -A` | Stages all additions, modifications, and deletions | Full working-tree staging |
| `git add -p` | Interactively stages parts of files | Best when one file contains unrelated changes |
| `git commit -m "message"` | Creates a commit | Save a logical unit of work |
| `git commit` | Opens editor for commit message | Detailed commit message |
| `git commit -am "message"` | Stages tracked modifications/deletions and commits | Quick commit; does NOT include new untracked files |
| `git commit --amend` | Replaces the latest commit | Fix latest commit before sharing |
| `git commit --amend --no-edit` | Adds current staged changes to previous commit without changing message | Forgot to include a file |
| `git commit --no-verify` | Skips local hooks | Emergency/special case; avoid unless necessary |

### Recommended daily pattern

```bash
git status
git diff
git add -p
git diff --cached
git commit -m "feat: add customer export"
```

---

# 6. Inspect differences

| Command | Why / what it does | When you use it |
|---|---|---|
| `git diff` | Changes not staged | Before `git add` |
| `git diff --cached` | Changes staged for commit | Before committing |
| `git diff HEAD` | All changes since last commit | Review everything |
| `git diff <branch1>..<branch2>` | Compares two branch tips | Compare branches |
| `git diff <commit1> <commit2>` | Compares two commits | Investigate history |
| `git diff --stat` | Shows summary of changed files | Quick review |
| `git diff --name-only` | Lists changed file names | Scripts/review |
| `git diff --word-diff` | Shows word-level changes | Documentation/config files |

---

# 7. Undo changes safely

This is one of the most important sections.

## Working-tree changes

| Command | Why / what it does | When you use it |
|---|---|---|
| `git restore <file>` | Discards unstaged changes in a file | You want to return file to HEAD |
| `git restore .` | Discards all unstaged changes | Only when certain you don't need them |
| `git restore --staged <file>` | Removes file from staging but keeps modifications | Accidentally staged a file |
| `git restore --staged .` | Unstages everything | Reset staging area |

## Reset

| Command | Why / what it does | When you use it |
|---|---|---|
| `git reset --soft HEAD~1` | Removes last commit but keeps changes staged | Redo previous commit |
| `git reset --mixed HEAD~1` | Removes last commit and unstages changes | Redo commit and review files |
| `git reset --hard HEAD~1` | Removes last commit and working changes | Dangerous; only for disposable local work |
| `git reset <file>` | Unstages a file | Older equivalent of `git restore --staged` |

### Important rule

Avoid `git reset --hard` on shared branches.

---

# 8. Revert a published commit

| Command | Why / what it does | When you use it |
|---|---|---|
| `git revert <commit>` | Creates a new commit that reverses an earlier commit | Safely undo a published change |
| `git revert HEAD` | Reverts latest commit | Undo latest shared commit |
| `git revert <old>..<new>` | Reverts a range of commits | Undo multiple published commits |

### Reset vs revert

```text
reset  = move branch history backward
revert = create a new commit that reverses history
```

For shared branches, prefer **revert**.

---

# 9. Branch management

| Command | Why / what it does | When you use it |
|---|---|---|
| `git branch` | List local branches | See branches |
| `git branch -a` | List all local/remote branches | See complete branch picture |
| `git branch <name>` | Create branch | Create feature branch |
| `git switch <name>` | Switch branches | Move to existing branch |
| `git switch -c <name>` | Create and switch to branch | Start feature work |
| `git checkout <name>` | Older branch switching command | Legacy repositories/workflows |
| `git checkout -b <name>` | Create and switch branch | Older equivalent of `switch -c` |
| `git branch -d <name>` | Delete merged local branch | Cleanup |
| `git branch -D <name>` | Force-delete local branch | Delete unmerged branch; dangerous |
| `git branch -m <new>` | Rename current branch | Rename branch |
| `git branch -vv` | Show upstream tracking | Diagnose synchronization |
| `git branch --merged` | Show merged branches | Cleanup |
| `git branch --no-merged` | Show unmerged branches | Check before deletion |

### Recommended modern syntax

Prefer:

```bash
git switch -c feature/payment-api
git switch main
```

instead of using `checkout` for ordinary branch switching.

---

# 10. Remote repositories

| Command | Why / what it does | When you use it |
|---|---|---|
| `git remote` | Lists remotes | Quick check |
| `git remote -v` | Shows fetch/push URLs | Verify remote |
| `git remote add origin <url>` | Adds a remote named origin | New local repository |
| `git remote set-url origin <url>` | Changes remote URL | Switch HTTPS ↔ SSH or repository |
| `git remote rename origin upstream` | Renames remote | Reorganizing remotes |
| `git remote remove origin` | Removes remote | No longer needed |
| `git remote show origin` | Detailed remote information | Troubleshooting |

### Origin vs upstream

Common fork workflow:

```text
upstream = company's original repository
origin   = your fork
```

---

# 11. Fetch, pull, and synchronization

| Command | Why / what it does | When you use it |
|---|---|---|
| `git fetch` | Downloads remote updates without changing your working branch | Safest way to inspect remote updates |
| `git fetch origin` | Fetches from origin | Daily synchronization |
| `git fetch --all` | Fetches from all remotes | Multi-remote projects |
| `git fetch --prune` | Removes stale remote-tracking branches | Branch cleanup |
| `git pull` | Fetches and integrates changes | Simple team workflow |
| `git pull --rebase` | Fetches then rebases local commits | Keeps linear history |
| `git pull --ff-only` | Only updates if fast-forward is possible | Safer automation |
| `git pull origin main` | Pulls main from origin | Explicit synchronization |

### Understand this

```bash
git pull
```

is approximately:

```bash
git fetch
git merge
```

or, with rebase configuration:

```bash
git fetch
git rebase
```

For professional development, learn `fetch` separately from `pull`.

---

# 12. Push changes

| Command | Why / what it does | When you use it |
|---|---|---|
| `git push` | Push current branch to configured upstream | Normal daily push |
| `git push -u origin feature/login` | Pushes and establishes upstream | First push of a new branch |
| `git push origin main` | Pushes main | Explicit branch push |
| `git push --all origin` | Pushes all local branches | Special administrative use |
| `git push --tags` | Pushes all tags | Release workflows |
| `git push origin <tag>` | Pushes one tag | Publish release tag |
| `git push --delete origin <branch>` | Deletes remote branch | Branch cleanup |
| `git push --force-with-lease` | Safely force-updates remote when your expectation matches | Rebased branch already pushed |
| `git push --force` | Force-updates remote regardless of remote changes | Dangerous; avoid on shared branches |

### Prefer

```bash
git push --force-with-lease
```

over:

```bash
git push --force
```

when rewriting your own remote feature branch.

---

# 13. Merge

| Command | Why / what it does | When you use it |
|---|---|---|
| `git merge <branch>` | Combines another branch into current branch | Integrate feature branch |
| `git merge --no-ff <branch>` | Forces a merge commit | Preserve explicit feature branch history |
| `git merge --ff-only <branch>` | Refuses merge commit | Enforce linear history |
| `git merge --abort` | Cancels conflicted merge | Merge went wrong |

Typical workflow:

```bash
git switch main
git pull --ff-only
git merge feature/login
git push
```

---

# 14. Rebase

| Command | Why / what it does | When you use it |
|---|---|---|
| `git rebase main` | Replays your branch commits on latest main | Update feature branch |
| `git rebase origin/main` | Rebase on latest fetched main | Common before PR |
| `git rebase -i HEAD~3` | Interactively edit last 3 commits | Squash/reorder/edit commits |
| `git rebase --continue` | Continue after resolving conflict | During rebase |
| `git rebase --skip` | Skip current commit | When commit is unnecessary |
| `git rebase --abort` | Cancel rebase | Recover from difficult rebase |

### Rebase rule

Do not casually rebase commits that other developers are already building on.

---

# 15. Merge conflicts

When Git says:

```text
CONFLICT (content): Merge conflict in file.py
```

Use:

```bash
git status
```

Then open the file and resolve:

```text
<<<<<<< HEAD
your version
=======
incoming version
>>>>>>> branch-name
```

Then:

```bash
git add file.py
git commit
```

For rebase:

```bash
git add file.py
git rebase --continue
```

To cancel:

```bash
git merge --abort
```

or:

```bash
git rebase --abort
```

Useful commands:

| Command | Why / what it does | When you use it |
|---|---|---|
| `git checkout --ours <file>` | Select current branch version during conflict | Merge conflict |
| `git checkout --theirs <file>` | Select incoming version | Merge conflict |
| `git restore --ours <file>` | Modern restore equivalent | Conflict resolution |
| `git restore --theirs <file>` | Modern restore equivalent | Conflict resolution |
| `git mergetool` | Opens configured merge tool | Complex conflicts |

---

# 16. Stash

Stash temporarily stores uncommitted work.

| Command | Why / what it does | When you use it |
|---|---|---|
| `git stash` | Saves tracked modifications | Need clean working tree temporarily |
| `git stash push -m "message"` | Saves with description | Multiple stashes |
| `git stash -u` | Also includes untracked files | Temporary incomplete work |
| `git stash -a` | Includes ignored files too | Rare; use carefully |
| `git stash list` | Lists stashes | Find saved work |
| `git stash show` | Shows stash summary | Inspect stash |
| `git stash show -p` | Shows stash diff | Inspect exact changes |
| `git stash pop` | Applies and removes latest stash | Resume work |
| `git stash apply` | Applies without deleting stash | Safer when uncertain |
| `git stash drop stash@{0}` | Deletes one stash | Cleanup |
| `git stash clear` | Deletes all stashes | Dangerous |
| `git stash branch <name>` | Creates branch from stash | Recover complicated work |

### Important

Stash is not a replacement for commits. For valuable work, make a temporary commit instead.

---

# 17. Tags and releases

| Command | Why / what it does | When you use it |
|---|---|---|
| `git tag` | Lists tags | Inspect releases |
| `git tag v1.0.0` | Creates lightweight tag | Simple marking |
| `git tag -a v1.0.0 -m "Release 1.0.0"` | Creates annotated tag | Recommended for releases |
| `git show v1.0.0` | Shows tag details | Inspect release |
| `git tag -d v1.0.0` | Deletes local tag | Correct mistaken tag |
| `git push origin v1.0.0` | Pushes tag | Publish release tag |
| `git push origin --delete v1.0.0` | Deletes remote tag | Correct release mistake |

Recommended versioning:

```text
v1.0.0
v1.0.1
v1.1.0
v2.0.0
```

---

# 18. Git log and history investigation

| Command | Why / what it does | When you use it |
|---|---|---|
| `git log --oneline` | Compact commit history | Daily |
| `git log --graph --oneline --all` | Visual history | Branch troubleshooting |
| `git log --decorate` | Shows refs/tags | Understand branch positions |
| `git log -p` | Shows patches | Investigate changes |
| `git log --stat` | Shows file statistics | Review history |
| `git log --author="Name"` | Filter commits by author | Audit history |
| `git log --since="2 weeks ago"` | Filter by date | Investigate recent work |
| `git log -- <file>` | History of one file | Debug changes |
| `git log -S "text"` | Finds commits that added/removed text | Find when code appeared |
| `git log -G "regex"` | Finds commits matching regex changes | Advanced investigation |
| `git show <commit>` | Inspect commit | Debug |
| `git show --stat <commit>` | Commit summary | Quick inspection |
| `git blame <file>` | Shows who last changed each line | Find historical context |
| `git blame -L 20,40 <file>` | Blame specific lines | Investigate code |
| `git reflog` | Shows movement of local refs | Recover lost commits/branches |

### `git reflog` is extremely important

If you accidentally reset/rebase something:

```bash
git reflog
```

Find the previous commit and recover it:

```bash
git reset --hard <commit>
```

---

# 19. Finding bugs

| Command | Why / what it does | When you use it |
|---|---|---|
| `git bisect start` | Starts binary-search debugging | Find bad commit |
| `git bisect bad` | Marks current commit as bad | Current version has bug |
| `git bisect good <commit>` | Marks known-good commit | Establish search boundary |
| `git bisect reset` | Ends bisect | Finished |
| `git blame <file>` | Identifies last commit touching lines | Locate origin |
| `git log -S "functionName"` | Finds changes involving text | Find introduction/removal |

### Typical bisect workflow

```bash
git bisect start
git bisect bad
git bisect good v1.2.0
```

Git checks out candidate commits. Test each one and run:

```bash
git bisect good
```

or:

```bash
git bisect bad
```

until Git identifies the likely offending commit.

---

# 20. Clean untracked files

| Command | Why / what it does | When you use it |
|---|---|---|
| `git clean -n` | Dry-run cleanup | ALWAYS inspect first |
| `git clean -f` | Deletes untracked files | Cleanup generated files |
| `git clean -fd` | Deletes untracked files/directories | Full untracked cleanup |
| `git clean -fdx` | Also deletes ignored files | Very dangerous; clean build environment |

Never use `git clean -fdx` casually.

---

# 21. Rename and delete files through Git

| Command | Why / what it does | When you use it |
|---|---|---|
| `git mv old new` | Renames/moves file | Track rename |
| `git rm file` | Deletes file and stages deletion | Remove tracked file |
| `git rm --cached file` | Stops tracking file but keeps it locally | Accidentally committed `.env` or generated file |
| `git rm -r directory` | Removes directory from Git | Remove tracked folder |

---

# 22. `.gitignore`

Typical entries:

```gitignore
# Python
__pycache__/
*.pyc
.venv/
venv/

# Node
node_modules/
.next/
dist/

# Environment/secrets
.env
.env.*
!.env.example

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db

# Logs
*.log
```

Useful command:

```bash
git check-ignore -v path/to/file
```

This tells you which `.gitignore` rule is causing a file to be ignored.

---

# 23. Submodules

| Command | Why / what it does | When you use it |
|---|---|---|
| `git submodule add <url> path` | Adds another Git repository as submodule | Monorepo-like dependency |
| `git submodule init` | Initializes submodule config | Existing submodule |
| `git submodule update` | Checks out recorded submodule commit | Clone/update project |
| `git submodule update --init --recursive` | Initializes nested submodules too | Common after clone |
| `git submodule update --remote` | Updates submodule from configured remote | Upgrade dependency |
| `git submodule status` | Shows submodule states | Troubleshooting |

---

# 24. Worktrees

Worktrees let you have multiple branches checked out simultaneously.

| Command | Why / what it does | When you use it |
|---|---|---|
| `git worktree add ../hotfix hotfix` | Creates another working directory | Work on hotfix while feature remains open |
| `git worktree list` | Lists worktrees | Inspect |
| `git worktree remove ../hotfix` | Removes worktree | Cleanup |
| `git worktree prune` | Removes stale worktree metadata | Troubleshooting |

This is extremely useful for developers who frequently switch between feature work and urgent production fixes.

---

# 25. Cherry-pick

| Command | Why / what it does | When you use it |
|---|---|---|
| `git cherry-pick <commit>` | Applies one commit to current branch | Move a specific fix |
| `git cherry-pick A B` | Applies multiple commits | Selected changes |
| `git cherry-pick A..B` | Applies a commit range | Transfer a series |
| `git cherry-pick --no-commit <commit>` | Applies changes without committing | Review/edit before commit |
| `git cherry-pick --continue` | Continue after conflict | Conflict resolution |
| `git cherry-pick --abort` | Cancel operation | Failed cherry-pick |

Common DevOps scenario:

```text
production
   ^
   | urgent bug fix
   |
development

Cherry-pick the production fix into development.
```

---

# 26. Advanced Git objects and references

| Command | Why / what it does | When you use it |
|---|---|---|
| `git rev-parse HEAD` | Prints current commit SHA | Scripts/debugging |
| `git rev-parse --abbrev-ref HEAD` | Prints current branch | Scripts |
| `git rev-parse origin/main` | Resolves remote ref | Automation |
| `git cat-file -p <sha>` | Inspects Git object | Advanced debugging |
| `git fsck` | Finds dangling/corrupt objects | Repository recovery |
| `git count-objects -vH` | Shows object storage usage | Repository maintenance |
| `git gc` | Cleans/optimizes repository | Maintenance |
| `git maintenance run` | Runs Git maintenance tasks | Large repositories |

---

# 27. Signed commits and tags

For security-sensitive organizations, signed commits/tags can prove authorship.

| Command | Why / what it does | When you use it |
|---|---|---|
| `git log --show-signature` | Shows signature information | Verify commits |
| `git verify-commit <commit>` | Verifies commit signature | Security/audit |
| `git verify-tag <tag>` | Verifies tag signature | Release verification |
| `git commit -S -m "message"` | Creates signed commit | Required by team policy |
| `git tag -s v1.0.0 -m "Release"` | Creates signed tag | Secure releases |

SSH signing is also supported by modern Git/GitHub configurations.

---

# 28. Authentication: HTTPS vs SSH

GitHub supports HTTPS and SSH for Git operations. Password authentication for Git over HTTPS has been removed; use a personal access token/credential manager or SSH instead.

## Test SSH

```bash
ssh -T git@github.com
```

## Typical SSH remote

```text
git@github.com:COMPANY/REPOSITORY.git
```

## Typical HTTPS remote

```text
https://github.com/COMPANY/REPOSITORY.git
```

### Recommended developer setup

For a long-term development machine, SSH is usually convenient:

```bash
ssh-keygen -t ed25519 -C "work-email@example.com"
```

Then add the public key to GitHub and test:

```bash
ssh -T git@github.com
```

Never share your private key.

---

# 29. SSH key commands

| Command | Why / what it does | When you use it |
|---|---|---|
| `ssh-keygen -t ed25519 -C "email"` | Creates SSH key pair | First-time GitHub SSH setup |
| `ssh-add ~/.ssh/id_ed25519` | Adds key to SSH agent | Use key without repeatedly entering passphrase |
| `ssh-add -l` | Lists loaded keys | Diagnose SSH |
| `ssh -T git@github.com` | Tests GitHub SSH authentication | Verify setup |
| `ssh -vT git@github.com` | Verbose SSH troubleshooting | SSH authentication problems |

For multiple GitHub accounts, use `~/.ssh/config` with separate host aliases.

Example:

```sshconfig
Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work

Host github-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
```

Then:

```bash
git clone git@github-work:COMPANY/repository.git
```

---

# 30. Git Credential Manager / HTTPS

On Windows, Git Credential Manager can securely store GitHub credentials.

Useful commands:

```bash
git config --global credential.helper manager
```

Do not put tokens directly into repository URLs such as:

```text
https://TOKEN@github.com/company/repo.git
```

Tokens can leak into shell history, logs, process listings, or configuration.

---

# 31. GitHub CLI (`gh`)

Install GitHub CLI separately from Git.

Check:

```bash
gh --version
```

Authenticate:

```bash
gh auth login
```

Check authentication:

```bash
gh auth status
```

Logout:

```bash
gh auth logout
```

Refresh permissions:

```bash
gh auth refresh
```

The GitHub CLI provides commands for repositories, pull requests, issues, releases, Actions, secrets, variables, organizations, rulesets, and API access.

---

# 32. GitHub CLI repository commands

| Command | Why / what it does | When you use it |
|---|---|---|
| `gh repo list` | Lists repositories you can access | Discover company repos |
| `gh repo view` | Shows current repo information | Inspect repo |
| `gh repo view --web` | Opens repository in browser | Quick navigation |
| `gh repo clone OWNER/REPO` | Clones repo | Developer setup |
| `gh repo create` | Creates repository | New project |
| `gh repo fork OWNER/REPO` | Forks repository | Contribute without direct write access |
| `gh repo edit` | Changes repository settings | Repository administration |
| `gh repo archive` | Archives repository | Retire project |
| `gh repo delete` | Deletes repository | Administrative/destructive action |

Be extremely careful with delete/archive operations.

---

# 33. Pull requests with `gh`

| Command | Why / what it does | When you use it |
|---|---|---|
| `gh pr list` | Lists PRs | Daily project management |
| `gh pr status` | Shows your PR status | Daily |
| `gh pr create` | Creates PR | Submit work |
| `gh pr create --fill` | Creates PR using commit information | Fast PR creation |
| `gh pr view <number>` | Views PR | Review |
| `gh pr view --web` | Opens PR in browser | Detailed review |
| `gh pr checkout <number>` | Checks out PR locally | Test/review someone else's PR |
| `gh pr diff <number>` | Shows PR diff | Review |
| `gh pr checks <number>` | Shows CI checks | Debug CI |
| `gh pr review <number> --approve` | Approves PR | Code review |
| `gh pr review <number> --request-changes --body "..."` | Requests changes | Review |
| `gh pr merge <number>` | Merges PR | Merge approved work |
| `gh pr close <number>` | Closes PR without merge | Reject/cancel |
| `gh pr reopen <number>` | Reopens closed PR | Continue work |
| `gh pr update-branch` | Updates PR branch | Bring branch up to date |

---

# 34. Issues with `gh`

| Command | Why / what it does | When you use it |
|---|---|---|
| `gh issue list` | Lists issues | Project management |
| `gh issue view <number>` | Views issue | Investigate task |
| `gh issue create` | Creates issue | Report/request work |
| `gh issue close <number>` | Closes issue | Complete issue |
| `gh issue reopen <number>` | Reopens issue | Continue issue |
| `gh issue comment <number>` | Adds comment | Communicate |
| `gh issue edit <number>` | Edits issue | Update metadata |
| `gh issue status` | Shows issue status | Personal/project tracking |

---

# 35. GitHub Actions / CI/CD with `gh`

| Command | Why / what it does | When you use it |
|---|---|---|
| `gh workflow list` | Lists workflows | Discover CI/CD pipelines |
| `gh workflow view <workflow>` | Shows workflow | Understand pipeline |
| `gh workflow run <workflow>` | Manually triggers workflow | Deployment/manual CI |
| `gh workflow enable <workflow>` | Enables workflow | Activate CI |
| `gh workflow disable <workflow>` | Disables workflow | Temporarily stop automation |
| `gh run list` | Lists workflow runs | CI/CD monitoring |
| `gh run view <run-id>` | Shows run details | Debug pipeline |
| `gh run watch <run-id>` | Watches running workflow | Live deployment monitoring |
| `gh run cancel <run-id>` | Cancels workflow | Stop bad deployment |
| `gh run rerun <run-id>` | Reruns workflow | Retry failed CI/CD |
| `gh run download <run-id>` | Downloads artifacts | Inspect build output |

---

# 36. GitHub Actions secrets and variables

| Command | Why / what it does | When you use it |
|---|---|---|
| `gh secret list` | Lists secrets you can manage | Audit configuration |
| `gh secret set NAME` | Creates/updates secret | Add API key/deployment credential |
| `gh secret delete NAME` | Deletes secret | Rotate/remove secret |
| `gh variable list` | Lists Actions variables | Inspect non-sensitive config |
| `gh variable set NAME --body "value"` | Sets variable | CI/CD configuration |
| `gh variable delete NAME` | Deletes variable | Cleanup |

Never put passwords, API keys, cloud credentials, or tokens in Git source code.

---

# 37. GitHub releases

| Command | Why / what it does | When you use it |
|---|---|---|
| `gh release list` | Lists releases | Release management |
| `gh release view <tag>` | Shows release | Inspect release |
| `gh release create v1.0.0` | Creates release | Publish version |
| `gh release create v1.0.0 ./build.zip` | Creates release and uploads asset | Distribute artifacts |
| `gh release upload v1.0.0 file.zip` | Adds asset | Publish build |
| `gh release download v1.0.0` | Downloads release assets | Deployment/testing |
| `gh release edit v1.0.0` | Edits release | Correct release metadata |
| `gh release delete v1.0.0` | Deletes release | Administrative correction |

---

# 38. GitHub organization management

Git itself does not manage GitHub organization permissions. For company administration, understand:

```text
Organization
 ├── Teams
 │    ├── Backend
 │    ├── Frontend
 │    ├── DevOps
 │    └── QA
 │
 └── Repositories
      ├── backend
      ├── frontend
      ├── infrastructure
      └── documentation
```

GitHub organization repository roles include:

| Role | Typical use |
|---|---|
| Read | View/discuss code |
| Triage | Manage issues/PRs without pushing code |
| Write | Active developers who push code |
| Maintain | Project maintainers without full destructive administration |
| Admin | Full repository administration |

Use the **least privilege** role appropriate for the person.

Organization owners have broad administrative access, so ownership should be limited.

---

# 39. GitHub organization commands

| Command | Why / what it does | When you use it |
|---|---|---|
| `gh org list` | Lists organizations you can access | Account audit |
| `gh org view ORG` | Views organization | Inspect org |
| `gh api orgs/ORG/members` | Lists organization members through API | Administration/audit |
| `gh api orgs/ORG/teams` | Lists teams | Team administration |
| `gh api orgs/ORG/repos` | Lists repositories | Organization inventory |

For sensitive organization operations, verify the required permissions before executing API commands.

---

# 40. GitHub API with `gh api`

`gh api` is one of the most important DevOps/admin tools because it lets you call GitHub's REST API from the terminal.

Basic:

```bash
gh api repos/OWNER/REPO
```

Get branches:

```bash
gh api repos/OWNER/REPO/branches
```

Get collaborators:

```bash
gh api repos/OWNER/REPO/collaborators
```

Get organization members:

```bash
gh api orgs/ORG/members
```

Use HTTP methods:

```bash
gh api -X POST repos/OWNER/REPO/issues \
  -f title="Bug" \
  -f body="Description"
```

Use JSON output filtering:

```bash
gh api repos/OWNER/REPO \
  --jq '.full_name'
```

This is useful for automation and DevOps scripts.

---

# 41. Repository access control checklist

For a company GitHub organization, establish:

| Area | Recommended practice |
|---|---|
| Organization owners | Keep very limited |
| Developers | Usually Write |
| Project maintainers | Maintain where appropriate |
| Repository admins | Only trusted administrators |
| Teams | Prefer teams over manually assigning many users |
| Branch protection/rulesets | Protect `main`/production branches |
| Pull requests | Require PRs for protected branches |
| Required reviews | Use appropriate reviewer requirements |
| Status checks | Require CI to pass |
| Force push | Block on important branches |
| Secrets | Store in GitHub Secrets/Environments, not source |
| 2FA | Require/enforce where appropriate |
| SSH/PAT | Individual credentials only; never share |
| Offboarding | Remove organization/repository access promptly |
| Audit | Periodically review members, teams, tokens, deploy keys, and workflows |

GitHub's current repository roles range from Read through Admin, and rulesets can enforce pull requests, status checks, signed commits, force-push restrictions, code scanning, file/path restrictions, and other controls.

---

# 42. Branch protection / rulesets

Typical production policy:

```text
main
 ├── no direct developer push
 ├── pull request required
 ├── 1–2 approvals required
 ├── CI must pass
 ├── conversation resolution required
 ├── force push blocked
 └── deployment checks required
```

Rulesets can enforce policies such as:

- Require pull requests
- Require approvals
- Require status checks
- Require branches to be up to date
- Require signed commits
- Block force pushes
- Block deletion
- Restrict who can push
- Require deployments to succeed
- Restrict file paths/extensions/sizes
- Require code scanning/code quality checks

---

# 43. GitHub Actions files you should understand

Common structure:

```text
.github/
└── workflows/
    ├── ci.yml
    ├── cd.yml
    └── security.yml
```

Typical pipeline:

```text
Push / Pull Request
        |
        v
Install dependencies
        |
        v
Lint
        |
        v
Unit tests
        |
        v
Build
        |
        v
Security scan
        |
        v
Deploy
```

Common commands:

```bash
gh workflow list
gh run list
gh run view <id>
gh run watch <id>
gh run rerun <id>
```

---

# 44. Git commands every full-stack developer should know

### Tier 1 — Daily commands

```bash
git status
git add
git commit
git pull
git push
git fetch
git switch
git branch
git diff
git log
git stash
```

### Tier 2 — Professional development

```bash
git merge
git rebase
git cherry-pick
git revert
git reset
git restore
git tag
git remote
git reflog
git blame
git bisect
```

### Tier 3 — Advanced / DevOps

```bash
git worktree
git submodule
git filter-repo
git fsck
git gc
git maintenance
git cat-file
git rev-parse
```

---

# 45. Typical professional feature workflow

```bash
git switch main
git pull --ff-only

git switch -c feature/customer-export

# edit files

git status
git diff
git add -p
git diff --cached
git commit -m "feat: add customer export"

git push -u origin feature/customer-export
```

Then create a PR:

```bash
gh pr create --fill
```

After review:

```bash
gh pr checks
gh pr view --web
```

---

# 46. Updating a feature branch from main

Option A — merge:

```bash
git fetch origin
git merge origin/main
```

Option B — rebase:

```bash
git fetch origin
git rebase origin/main
```

If your team prefers clean linear history, rebase your private feature branch before opening/updating the PR.

---

# 47. Hotfix workflow

```bash
git switch main
git pull --ff-only

git switch -c hotfix/payment-timeout

# fix
git add .
git commit -m "fix: handle payment timeout"

git push -u origin hotfix/payment-timeout
gh pr create --fill
```

After deployment, tag the release:

```bash
git switch main
git pull --ff-only
git tag -a v1.4.1 -m "Release v1.4.1"
git push origin v1.4.1
```

---

# 48. Recover from common mistakes

## Accidentally staged a file

```bash
git restore --staged file
```

## Accidentally modified a file and want to discard it

```bash
git restore file
```

## Forgot a file in your last commit

```bash
git add forgotten-file
git commit --amend --no-edit
```

## Want to undo the latest shared commit

```bash
git revert HEAD
git push
```

## Accidentally reset a branch

```bash
git reflog
```

Find the previous SHA and recover it:

```bash
git reset --hard <sha>
```

## Accidentally committed `.env`

Immediately stop and assess whether the secret was exposed.

Remove the file from tracking:

```bash
git rm --cached .env
```

Add:

```gitignore
.env
```

Then rotate the exposed credential.

**Important:** Removing the file from the latest commit does not necessarily remove the secret from Git history. For sensitive secrets, rotate/revoke the credential immediately and use history-rewriting tools only with an agreed repository-maintenance plan.

---

# 49. Git LFS

For large binary files:

```bash
git lfs install
git lfs track "*.psd"
git add .gitattributes
git add file.psd
git commit -m "chore: track large design file"
git push
```

Check:

```bash
git lfs ls-files
```

Use Git LFS instead of putting huge binaries directly into normal Git history.

---

# 50. Git hooks

Hooks automate local checks.

Examples:

```text
pre-commit
commit-msg
pre-push
post-checkout
```

Typical uses:

- Formatting
- Linting
- Unit tests
- Commit message validation
- Secret detection

Inspect:

```bash
ls .git/hooks
```

Project-managed hooks are often configured with tools such as pre-commit, Husky, or Lefthook.

---

# 51. DevOps repository structure

A typical infrastructure repository may contain:

```text
repo/
├── application/
├── docker/
├── helm/
├── k8s/
├── terraform/
├── ansible/
├── scripts/
├── .github/
│   └── workflows/
├── Dockerfile
├── docker-compose.yml
└── README.md
```

Git knowledge becomes particularly important because infrastructure changes are also production changes.

---

# 52. Git + Docker workflow

Typical pattern:

```bash
git switch main
git pull --ff-only

git switch -c feature/update-container

# modify Dockerfile

git diff
git add Dockerfile
git commit -m "build: update application image"

git push -u origin feature/update-container
```

CI then builds:

```text
GitHub
  |
  v
GitHub Actions
  |
  v
docker build
  |
  v
docker push
  |
  v
Container registry
  |
  v
Kubernetes
```

---

# 53. Git + Kubernetes workflow

Keep manifests/versioned configuration in Git:

```text
k8s/
├── deployment.yaml
├── service.yaml
├── ingress.yaml
└── configmap.yaml
```

Never casually edit production Kubernetes resources without recording the intended configuration in version control.

Typical workflow:

```bash
git switch -c feature/update-k8s-resources
# edit manifests
git diff
git add k8s/
git commit -m "ops: update application resources"
git push -u origin feature/update-k8s-resources
```

---

# 54. Git + Terraform workflow

Typical Terraform repository:

```text
terraform/
├── main.tf
├── variables.tf
├── outputs.tf
├── providers.tf
└── environments/
```

Workflow:

```bash
terraform fmt
terraform validate
terraform plan
```

Then:

```bash
git diff
git add .
git commit -m "infra: update cloud resources"
git push
```

CI should normally run formatting, validation, security checks, and plan before production apply.

---

# 55. Git + CI/CD best practices

A professional pipeline should generally:

```text
Developer
   |
   v
Feature branch
   |
   v
Pull Request
   |
   +--> lint
   +--> unit tests
   +--> integration tests
   +--> security scan
   +--> build
   |
   v
Review
   |
   v
Merge
   |
   v
Deploy
```

Avoid:

```text
Developer -> git push main -> production
```

unless the organization deliberately uses such a workflow.

---

# 56. Useful aliases

You can create shortcuts:

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.sw switch
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --decorate --all"
```

Then:

```bash
git st
git sw main
git br
git lg
```

---

# 57. Command-line troubleshooting checklist

When Git behaves unexpectedly:

```bash
git status
git branch -vv
git remote -v
git fetch --prune
git log --oneline --graph --decorate --all
```

Then inspect:

```bash
git diff
git diff --cached
```

For authentication:

```bash
gh auth status
ssh -T git@github.com
```

For branch divergence:

```bash
git fetch origin
git status
git log --oneline --left-right HEAD...origin/main
```

---

# 58. "Ahead / behind" explained

Suppose Git says:

```text
Your branch is ahead of 'origin/main' by 3 commits.
```

Your local branch has commits GitHub does not have.

Use:

```bash
git push
```

If:

```text
Your branch is behind 'origin/main' by 3 commits.
```

GitHub has commits your local branch does not have.

Use:

```bash
git fetch origin
git log --oneline HEAD..origin/main
```

Then decide whether to:

```bash
git merge origin/main
```

or:

```bash
git rebase origin/main
```

---

# 59. Dangerous commands

Understand these before using them:

| Command | Risk |
|---|---|
| `git reset --hard` | Can destroy local changes |
| `git push --force` | Can overwrite remote history |
| `git push --force-with-lease` | Safer but still rewrites history |
| `git clean -fd` | Deletes untracked files |
| `git clean -fdx` | Deletes ignored and untracked files |
| `git branch -D` | Deletes unmerged branch |
| `git stash clear` | Deletes all stashes |
| `git push --delete` | Deletes remote branch/tag |
| `git filter-repo` | Rewrites repository history |
| `git gc` | Repository maintenance; normally safe, but understand recovery implications |
| `gh repo delete` | Deletes GitHub repository |

Before destructive commands, ask:

```text
Is this branch shared?
Is this data backed up?
Do I need the current history?
Could another developer be using this branch?
```

---

# 60. Recommended company Git workflow

For a professional full-stack + DevOps team:

```text
main
  |
  +-- feature/*
  +-- fix/*
  +-- hotfix/*
  +-- release/*
```

Recommended process:

1. Pull latest `main`.
2. Create a short-lived feature/fix branch.
3. Make small logical commits.
4. Push branch.
5. Open pull request.
6. CI runs automatically.
7. Reviewer approves.
8. Merge through GitHub.
9. Deploy through CI/CD.
10. Tag releases when appropriate.
11. Delete merged feature branch.

---

# 61. Commit message convention

A useful convention is:

```text
feat: add customer export
fix: handle null invoice value
refactor: simplify billing service
docs: update deployment guide
test: add payment service tests
build: update Node version
ci: add deployment workflow
perf: optimize CDR query
security: restrict API endpoint
chore: update dependencies
```

This makes Git history easier to understand and automate.

---

# 62. Full daily cheat sheet

```bash
# Check
git status
git branch -vv
git remote -v

# Update
git fetch --prune
git pull --ff-only

# Create branch
git switch -c feature/my-feature

# Review
git diff
git diff --cached

# Stage
git add -p

# Commit
git commit -m "feat: implement feature"

# Push
git push -u origin feature/my-feature

# GitHub
gh pr create --fill
gh pr status
gh pr checks

# Update feature branch
git fetch origin
git rebase origin/main

# Conflict
git status
# resolve
git add .
git rebase --continue

# Cancel rebase
git rebase --abort

# Recover
git reflog
```

---

# 63. The commands you should master first

If you currently have limited Git experience, do **not** try to memorize everything at once.

Master these first:

```text
1.  git status
2.  git add
3.  git diff
4.  git commit
5.  git log
6.  git branch
7.  git switch
8.  git fetch
9.  git pull
10. git push
11. git remote
12. git merge
13. git rebase
14. git stash
15. git restore
16. git revert
17. git reset
18. git reflog
19. git cherry-pick
20. gh auth
21. gh pr
22. gh run
23. gh repo
24. gh secret
25. gh api
```

---

# 64. Suggested learning order for you

### Level 1 — Daily developer Git

Learn until these feel automatic:

```text
status
add
diff
commit
branch
switch
fetch
pull
push
log
remote
restore
stash
```

### Level 2 — Team collaboration

Then learn:

```text
merge
rebase
merge conflicts
revert
cherry-pick
tags
PR workflow
branch protection
```

### Level 3 — DevOps

Then:

```text
GitHub Actions
gh run
gh workflow
gh secret
gh variable
releases
tags
SSH
GitHub API
```

### Level 4 — Git administration

Finally:

```text
organization roles
teams
repository permissions
rulesets
branch protection
deploy keys
SSH keys
PATs
audit
GitHub Apps
webhooks
GitHub API automation
```

---

# 65. Golden rules for professional Git usage

1. **Never push secrets.**
2. **Never share personal SSH private keys or PATs.**
3. **Never force-push shared branches unless your team explicitly requires it.**
4. Prefer `git push --force-with-lease` over `--force` when rewriting your own remote branch.
5. Use pull requests for important changes.
6. Keep commits small and meaningful.
7. Pull/fetch before starting important work.
8. Review `git diff` before committing.
9. Review `git diff --cached` before committing.
10. Use `git revert` to undo changes already shared with others.
11. Use `git reflog` when you think you lost a commit.
12. Protect production/default branches.
13. Require CI checks before merging production code.
14. Use least-privilege GitHub roles.
15. Use teams for scalable company access management.
16. Keep organization owners limited.
17. Rotate credentials immediately if they are exposed.
18. Treat infrastructure repositories as production-sensitive.
19. Do not blindly copy AI-generated Git commands.
20. Before any destructive command, understand exactly which refs/files it will modify.

---

# 66. Quick distinction: Git vs GitHub vs GitHub CLI

| Tool | What it is | Main purpose |
|---|---|---|
| Git | Distributed version-control system | Track code/history locally and synchronize repositories |
| GitHub | Hosted Git collaboration platform | Repositories, PRs, Issues, Actions, permissions, releases |
| `gh` | GitHub command-line client | Manage GitHub resources from terminal |
| GitHub Actions | CI/CD automation platform | Build, test, scan, deploy |
| GitHub API | Programmatic GitHub interface | Automation and administration |

---

# 67. Official references

- Git command reference: https://git-scm.com/docs
- GitHub documentation: https://docs.github.com/
- GitHub CLI manual: https://cli.github.com/manual/
- GitHub authentication: https://docs.github.com/en/authentication
- GitHub repository roles: https://docs.github.com/en/organizations/managing-user-access-to-your-organizations-repositories/managing-repository-roles
- GitHub rulesets: https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets
- GitHub Actions: https://docs.github.com/en/actions

---

## Final recommendation

Because Git is now part of your daily work, your goal should not be to memorize every command.

Your goal should be to understand the **state transitions**:

```text
edit
  ↓
git diff
  ↓
git add
  ↓
git diff --cached
  ↓
git commit
  ↓
git fetch
  ↓
rebase / merge
  ↓
git push
  ↓
Pull Request
  ↓
CI/CD
  ↓
review
  ↓
merge
  ↓
deploy
  ↓
tag/release
```

Once this mental model becomes natural, the individual Git commands become much easier to remember.
