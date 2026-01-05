---
tags: 
- Other
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]

# GIT PART 1
# Porcelain and plumbing
- Git commands are split into 2 groups
- High level commands are known as Porcelain, while low level commands are plumbing
- Porcelain commands include:
	- `git status`
		- Shows you the status of your repo
		- Shows untracked, unstaged, committed files, etc..
	- `git add <files-to-stage>`
		- Stages files in preparation for committing them
		- `git add -p <path/to/file>` allows you to go through a file hunk by hunk and choose whether to stage it, pass it, split it into smaller pieces, manually edit it, or quit
		- `-p` is the `--patch` flag
		- You can also do `git add -i` to get into interactive mode
	- `git commit`
		- Committs staged files. add `-m` to leave a commit message
	- `git push`
	- `git pull`
	- `git log`
		- My favorite log command is `git log --oneline --parents --all --graph`
		- `--oneLine` summs up and shortens the commits skipping info like the author name
		- `--parents` shows the parent commits
		- `--all` prints the full log showing branches that aren't even related to the one I'm on
		- `--graph` shows a nice graph that demonstrates how branches diverged and were re-merged later
		- `--no-pager` I don't use this one but it's used to get the output into the terminal without having it being in a pager
	- `git branch`: Creates a branch but doesn't go to it
	- `git switch -c`: Switches to a branch
		- `-c` creates the branch before switching to it
		- You can specify the first 4 digits of the hash on any commit to base your branch off of it
- Plumbing commands include:
	- `git apply`
	- `git commit-tree`
	- `git hash-object`
	- `git cat-file -p [hash]`
		- Fetches the contents of a commit using its hash or ref
		- Usually, a commit will show 3-4 elements:
			- Tree
			- Parent (depends)
			- Author
			- Committer
		- `-p` is for pretty print
# Configuration
- Git configuration has 4 layers, and is done via the `git config` command. Git's default is the global config
	- System: `/etc/gitconfig`
	- Global: Supposedly `~/.gitconfig` but mine is in `~/.config/git/git`
	- Local: In a repo's `.git/config`
	- Work tree: In a repo's `.git/config.worktree`
- Each more specific config overrides the previous more global configs
- While git only uses certain keys, you can still store any key-value pair that you want in the config
- Git's keys are also made up of two parts, a "section" and a "key". This way you can have for example the user section, with the name and email keys
- `git init` creates a `.git` file in whatever directory you're in
- `git config set` is used to set configs key-value pair style
- `git config get` retrieves the value of a key
- `git config set --global init.defaultBranch master` is used to set the name of the default branch globally
- `git config unset` is used to remove a value from a key, but not from a section
	- This means you can unset "user.name", but not "user" itself
- Git apparently allows you to have duplicate keys too, so you can have multiple "user.name" keys, and you can unset them all using `git config unset --all <section.key>`
- While you can unset a section, you can still remove it with `git config remove-section <section>`
# How git works
- Git hashes and compresses everything to save up on space and remain lightweight
- It uses trees (directories) and blobs (files) to save data
- A hash commit is made up of:
	- The commit message
	- The author's name and email
	- The date and time
	- Parent commit hashes
- Everything in git is an object, the commits, tags, trees, blobs, everything
- Git stores a full snapshot of files on a per-commit level. This means that while git doesn't duplicate files, it doesn't just store the changes you made in a file, but an entire new file containing the changes, with a new hash.
- This is how git is able to track a file's changes throughout the branch's history, and how it's able to revert back to an old version of the file
- A branch is a pointer to a specific commit
	- A branch isn't your typical `C` pointer, but rather it's a ref (hash) or named pointer that points to the tip commit of a branch (last commit of a branch) which is also a hash, but not a ref, a ref only points to another hash
- Normally git only needs the first 7 digits of a hash to function, but from what I found, you can actually pass just the first 4 to a command and it will work just fine
- An example of this system is that:
	- If you `git log` and get the hashes of the commits
	- You can then `git cat-file` with the hash of the tree the commit is showing
	- This will bring back the blob, then `git cat-file` on the blob's hash will show the file's contents
- From these findings it's obvious that git uses a tree data structure to function
- The head though which is normally a references that references other refs (branch hashes) can also be detached and point at a commit directly
	- If this happens you basically end up with a linked list as you will only know about this one branch, which is why it's detached
	- You can rejoin society though by creating a proper branch off of the detached head
# Merging and rebasing
#### Merge
- When you merge two branches, one of two scenarios are possible
	1. One branch is ahead of the other, and otherwise they have the same commits
		- In this case a fast-forward merge is done where the changes of branch B are added on top of branch A
		- A fast-forward merge means that the pointer of the base (branch you're merging into) branch will be moved to the tip of the feature (branch you're merging from) branch
	2. The two branches have a common parent, but have diverged into two different path
		- Here, if you merge branch B into branch A, then a merge commit will be created, where first branch A's commits are applied, then branch B
		- If the changes affect different files then both branches commits will co-exist just fine, but if both branches affected the same function in the same file for example, you run into a conflict that must be resolved
- After the merge is done, `git log` will show the merge commit as the top commit and it will have 2 parents instead of one, which are the two tips of the branches that got merged
#### Rebase
- When we rebase, we basically alter the history of the feature branch (technically the base branch now since we need to be checked out here before rebasing) to match the base branch, then we add the changes on top of the tip of the base branch.
- Essentially, this means that git will apply the base branch's commits one by one starting from the best common ancestor (where both branches diverged), all the way till the tip of the base branch, then it will add the commits of the feature branch on top of it
#### How they compare
- If you mainly rely on merging, then you'll maintain the true form of the history of your repo, but it may get bloated with a bunch of merge commits
- Rebasing doesn't maintain the true history, but it maintains a more linear and cleaner history that's easier to read and work with
- However, under no circumstance should you rebase a public branch onto any other branch, as public branches like div and main are your ultimate source of truth that all other team members rely on, and altering them will disrupt everyone else
# Reset
- Git reset can be used in two forms
	- `git reset --soft <commit_hash>`
	- `git reset --hard <commit_hash>`
- The difference here is that `--soft` will take you back to the previous commit, while keeping your changes uncommitted and staged in the staging area (the work tree), and already uncommitted changes will remain as is, staged or unstaged
- Similarly, `--hard` takes you back to a previous commit, but this time, all changes in the work tree, and any commit after the one you're going back to, are discarded
# Remote
- `git remote add <name> <url>` adds another git repo, on a server or even on your own device as a remote related repo, that by convention is the "authoritative source of truth" for your repo
- By convention when we add a remote repo, we name it "origin"
- `git fetch` fetches all meta data from the remote branch without applying the changes to your local repo
	- It's useful if you want to check where your colleagues are at with their own changes before applying it with `git pull` in order to review the project's progress and potentially avoid merge conflicts
- We can also `git merge [<remote>/<branch>]` to merge a remote branch into a local branch
- When we `git push origin <branch_name>` we have some options
	- The most common is just `git push origin main` to merge our local branch "main" with the remote branch "main"
	- We can also do `git push origin <localbranch>:<remotebranch>` to push a local branch to a remote branch with a different name
	- Or we can do `git push origin :<remotebranch>` keeping the local branch part empty in order to delete a remote branch
- Same options are possible with `git pull` but I need to verify this
# Workflow
#### Solo
1. Make changes to files
2. `git add .` (or `git add <files>` if I only want to add specific files)
3. `git commit -m "a message describing the changes"`
4. `git push origin main`
#### Team
1. Update my local `main` branch with `git pull origin main`
2. Checkout a new branch for the changes I want to make with `git switch -c <branchname>`
3. Make changes to files
4. `git add .`
5. `git commit -m "a message describing the changes"`
6. `git push origin <branchname>` (I push to the _new_ branch name, not `main`)
7. Open a [pull request](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests) on GitHub to merge my changes into `main`
8. Ask a team member to review my pull request
9. Once approved, click the "Merge" button on GitHub to merge my changes into `main`
10. Delete my feature branch, and repeat with a new branch for the next set of changes
# Gitignore
- Super useful for keeping files untracked by git
- Examples of files that you want to keep out of your remote repo are:
	1. Ignore things that can be _generated_ (e.g. compiled code, minified files, etc.)
	2. Ignore dependencies (e.g. `node_modules`, `venv`, `packages`, etc.)
	3. Ignore things that are personal or specific to how you like to work (e.g. editor settings)
	4. Ignore things that are sensitive or dangerous (e.g. `.env` files, passwords, API keys, etc.)
- When working with .gitignore, you can use the following patterns
	- You can have multiple .gitignore files in your repo nested in different directories
	- `*.txt` or any other file extension to inform git that it should ignore all files with this extension for example
	- Using `/` will ignore files that in the same directory as the .gitignore file only
	- If you want to ignore all files of a specific type `*.txt` except for a few examples, you can use `!/important.txt` to specify the files you want to exclude
	- You can add comments with `#` too
	- Also remember that the order is super important, so doing something like `temp/*` then `!temp/instructions.md` will exclude the instructions files from .gitignore, but if you reverse the order, it will be ignored
# GIT PART 2
- `git commit --amend` allows you to amend a commit message that maybe you made a mistake on, however, it also alters the commit's hash, so it basically makes a new one
- Forking a repo is actually not a git operation, but rather a feature offered by many Git hosting services. Forking allows you to copy a repo into your own account so that you can play around with it without affecting the original
## PRs from a Fork
- If you want to contribute to an open-source project, you need to:
	1. Fork their repo into your account
	2. Clone your fork to your local machine
	3. Create a new branch (let's call it your_feature)
	4. Make changes
	5. Commit and push changes to your fork's remote your_feature branch
	6. Create a pull request to original_owner/repo main from your_username/repo your_feature
## Reflog
- Logs the changes to a reference
- It's basically git log but with a step by step path made of every action taken, such as commits and branch switches all the way to step 0 which is where you're at now
- Reflog also, unlike log, doesn't just show you what's currently available in the branch, but if you were to `git reset`, which would remove a commit and by consequence remove it from `git log`, `git reflog` will tell you that you reset a commit
- Reflog basically follows the movement of the HEAD ref
- Imagine you deleted a branch with a unique command on it, and you need that commit, now what?
- Well, thanks to reflog, you can recover the hash of that commit, and follow it down with `git cat-file` all the way till you find the blob's contents
- The resulting chain of commands would look like this
```bash
git reflog # find the commit sha at HEAD@{1}
git cat-file -p <commit sha>
git cat-file -p <tree sha>
git cat-file -p <blob sha> > slander.md
git add .
git commit -m "B: recovery"
```
- There is actually a better way of doing that
## Merge
- `git merge <commitish>` takes a commitish as an argument. What's a commitish you say? Well, that's anything that looks like a commit, such as a branch, tag, commit, HEAD@{1}, etc... basically anything that has a hash it seems
- Using `git merge` we get
```bash
git merge HEAD@{1}
```
- Yes really, _the more you know_ I guess
# Conflicting Changes
- Isn't it great that every developer works on _different_ lines of code when working on a project? Ahhhhh, so nice... or it would be if it was true, but then that chapter on merge conflicts wouldn't exist
- Merge conflicts occur when you're trying to merge two commits that both make changes to the same lines of code and they're not in a parent-child relationship, after all, git doesn't know which version of the change to keep, so it flags it as a conflict and tells you to handle it, mr distinguished engineer
- Remember, in a merge conflict, 'ours' refers to the branch we're currently on, the one HEAD will be pointing to, and 'theirs' is the branch we're merging into our branch
## Checkout Conflict
- Turns out 'ours' and 'theirs' aint just terminology, git actually uses this terminology as flags for some commands
- For example, we can fix conflicts using git's own tools instead of manually editing files
- Here's an example of how `git checkout --theirs path/to/file`
- This also means that `git checkout` isn't just an outdated version of `git switch`, it still has it's own unique use
- Also, remember when resolving conflicts that you still need to provide a message to the merge conflict resolution commit to document how the conflict was resolved
- That also means that this is the only type of merge where git wont generate its default "Merge branch 'branch-name'" message, since you have to provide a custom one
## Rebase Conflicts
- Since rebase checks out the source branch that you're rebasing on top of to then replay your branch's commits on top, the conflict here will have the 'theirs' branch as the HEAD unlike what happened in the merge conflict. So in this context, 'ours' is the main branch, and 'theirs' is the feature branch (again because git switched branches under the hood) despite the fact that we're rebasing while being checked out on the feature branch
- Also, during a rebase conflict, you won't be on a branch at all, you'll be in "detached HEAD" until you resolve the conflict
- Again, if we use `git checkout --theirs` here, it will be the reverse of during a merge conflict, as now it refers to the feature branch (previously 'ours') and vice versa for `git checkout --ours`, this distinction is really important
- Also, in a rebase, after resolving the conflict we `git add` but we don't `git commit`, instead we `git rebase --continue`
- During a rebase conflict, if you choose to resolve the conflict by keeping the changes from the base branch instead of the feature branch, keep in mind that the commit you decided to forsake from the feature branch, will in fact, be gone from history, since it is not pointless, but because reflog is based, it will still keep mention of it as usual
- If we instead keep some changes from the feature branch commit, even if not all of them, git would still reference it in the history
## Repeat Resolution Setup
- Sometimes, especially with rebase conflicts, you may find yourself resolving the same conflict over and over again, which can be very annoying especially when you have multiple long-running feature branches being rebased off of main
- Git gives us a solution though called "rerere" or Reuse Recorded Resolution
- This setting allows git to remember how you usually resolve a specific conflict and then it can resolve it automatically
- This hidden tech also applies to merging
- To enable "rerere": `git config set --local rerere.enabled true`
- Rerere cache can be cleared in case you don't want the recorded resolutions to persist: `rm -rf .git/rr-cache`
# Squashing
- Squashing is the act of basically merging multiple commits into one
- It's generally useful for keeping your history clean, or in case you work with a team that prefers single PR commits
- Squashing is actually not a separate command, but rather, we do it with `git rebase`, here's how:
	1. Start an interactive rebase with the command `git rebase -i HEAD~n` where n is the number of commits you want to squash
	2. Git will open the default editor with a list of commits. Change the word 'pick' to 'squash' for all but the first commit
	3. Save and close the editor
-  `-i` stands for interactive, which allows us to edit the commit history before Git applies the changes
- `HEAD~n` is how we reference the last n commits since HEAD points to the current commit while we're in a clean state, so `~n` means n commits before HEAD
- The reason we use rebase here, is that rebase is used for replaying changes
- When we rebase onto a specific commit, we tell Git to replay all changes, on top of that commit, instead of them being separate commits
- The interactive flag then lets us squash all those changes into a single commit
- Also to rename a branch: `git branch -m old-branch-name new-branch-name` or just `git branch -m new-branch-name`should rename the branch you're on I believe
- As a personal opinion regarding squashing and rebasing in general, always make a "temp_branch-name" of the branch you want to rebase or squash to make sure the history and changes are what you want them to be, before actually removing that branch and renaming the temp branch to the original branch's name. This could just add a safety net in case you mess up and want to try again so that the mistake is not permanent
#### Force Push
- Now that we completely removed some commits from main, it became out of sync with the remote main, and a normal `git push origin main` won't work anymore
- To push these changes we need to do `git push origin main --force`, which tells Git to just make the remote branch the same as the local one
#### Squashing PRs
- It's common practice to squash the commits on a feature branch if the team you're working with prefers a PR with a single commit, a possible workflow for that is:
	1. Create a new branch off of `main`.
	2. Go about your work on the feature branch making commits as you go.
	3. When you're ready to get your code into `main`, squash all your commits into a single commit.
	4. Push your branch to the remote repository.
	5. Open a pull request from the feature branch into `main`.
	6. Merge the pull request once it's approved.
# Stash
- `git stash` records the current state of the working directory and index (staging area) and records those changes in a safe place while reverting the work tree back to match the HEAD commit, you can also `git stash list` to see all your stashes
- `git stash pop`: applies the stash to the current working directory and removes it from the stash list
- The stash is a LIFO stack so you add and pop stashes
- You can also stash changes with a message `git stash -m "message"`
- `git stash apply`: applies the most recent stash without removing it from the list
- `git stash drop`: remove the most recent stash from the list
- `git stash apply stash@{2}`: stashes are zero indexed, so this applies the 3rd most recent stash in the list
# Revert
- `git revert <commit-hash>` is a bit less brutal than `git reset` as instead of removing a commit, it just creates a commit that reverses it, effectively undoing the change while keeping track in the history that that undoing was done
#### Diff
- `git diff` shows you the difference between various things, like the work tree, commits and so on. As is, the command will show you the difference between the work tree, and last commit
- `git diff HEAD~1` shows the diff between the previous commit and the current state + last commit
- `git diff COMMIT_HASH_1 COMMIT_HASH_2` shows the diff between two commit
#### Reset VS Revert
- Just like with merging and rebasing, if you're working on your own branch and you want to undo a mistake you made, then `git reset` is a good option
- If you're trying to undo a change on a publish branch though, then it's definitely better to go for `git revert` so that everyone would know what changed
# Cherry Pick
- `git cherry-pick <commit-hash>` is mainly useful when you just want to yoink a specific commit, or a couple from a branch, without the rest of the branch, so you don't want to either merge or rebase
- To cherry pick, you need:
	1. First, you need a clean working tree (no uncommitted changes).
	2. Identify the commit you want to cherry-pick, typically by `git log`ing the branch it's on.
	3. Run: `git cherry-pick <commit-hash>`
# Bisect
- `git bisect` uses binary search to find a commit that introduced a bug
- This command isn't only meant for bugs, it's meant for any change that you want to find really but the most common use case is bugs or performance regressions
- Bisecting involves 7 steps:
	1. Start the bisect with `git bisect start`
	2. Select a "good" commit with `git bisect good <commitish>` (a commit where you're sure the bug wasn't present)
	3. Select a bad commit via `git bisect bad <commitish>` (a commit where you're sure the bug was present)
	4. Git will checkout a commit between the good and bad commits for you to test to see if the bug is present
	5. Execute `git bisect good` or `git bisect bad` to say the current commit is good or bad
	6. Loop back to step 4 (until `git bisect` completes)
	7. Exit the bisect mode with `git bisect reset`
- At the end, `git show` on the commit hash can show the commit message and diff of before the commit and after the commit
- Also `git blame <commit-hash> <path/to/file>` can then show the change again, but also who made it. Although `git cat-file` and `git show` also show the author, but blame shows the author for every line in the file
- Bisect also has a super neat feature, where you can actually pass it a script to automate the process of locating the bad commit, but the script needs to exit with 0 if the file is good and between 1 and 127 (except 125) if it's bad (possible CICD idea?)
- You'll need to start the bisect and mark the good and bad commits, then you can `git bisect run <path/to/script>` and let bisect do its thing
# Worktrees
- First thing first, worktree, working tree, and working directory, are interchangeable
- This directory is where the code Git is tracking for you lives, which is usually the root of the repo
- `git worktree list` shows you all the worktrees that you created
- Worktrees are mainly useful when:
	1. You want to switch back and forth between the two change sets without having to run a bunch of git commands (not branches or stash)
	2. You want to keep a light footprint on your machine that's still connected to the main repo (not clone)
#### The main worktree
- Contains the .git directory with the entire state of the repo
- Getting a new "main" worktree would require a `git clone` or `git init`
#### Linked Worktree
- This worktree contains a .git _file_ with a path to the main working tree
- This makes it really light since it doesn't contain any data, so it's as light as a branch
- It can however be complicated to use when env files and secrets are involved
#### Create a Linked Worktree
- `git worktree add <path> [<branch>]` makes a new worktree, and the branch bit is optional, otherwise it will use the last part of the path as the branch name
- You run this command from your repo, where path is the path to the linked worktree directory
- Linked work trees behave just like normal repos, where you can create branches, tags, etc..
- However, you can't work on a branch, that's currently checked out, on another worktree
- References for worktrees are located in .git/worktrees
- Also, making a change in a linked worktree automatically reflects in the main worktree, since that's where the .git directory is anyway
- `git worktree remove WORKTREE_NAME` removes a worktree
- You can also delete the linked work directory manually, then `git worktree prune` will remove the references of deleted worktrees
- Deleting the worktree, wont delete its default branch though, or any branch created by it I suppose
# Tag
- A tag is a name linked to a commit that doesn't move between commits
- It can be created and deleted, but not modified
- `git tag` lists all tags, and `git tag -a "tag name" -m "tag message"` creates a tag on the current commit
- Semantic versioning applies here too, just like with docker images [[PG Docker#Publishing]]
![[semantic_versioning.png]]
- A semantic version has 2 primary purposes:
	1. To give us a standard convention for versioning software
	2. To help us understand the impact of a version change and if it's safe (how hard it will be) to upgrade to
- The rules for semantic versioning are:
	- MAJOR increments when we make "breaking" changes (this is typically a big release, for example, Python 2 -> Python 3)
	- MINOR increments when we add new features in a backward-compatible manner
	- PATCH increments when we make backward-compatible bug fixes
- Major version 0 is considered to be pre-release software
- Tags serve multiple purposes, but mainly it's to denote releases
- An example of a tag `git tag -a v3.10.2 -m "Fixed a lil bug"`
- Tags are commitish as we said before, so they can be used anywhere a commit hash can be used
- Finally, a commit, can actually have multiple tags, and you can push tags to the remote repo with `git push origin --tags`