---
tags: 
- Other
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]

# Porcelain and plumbing
- Git commands are split into 2 groups
- High level commands are known as Porcelain, while low level commands are plumbing
- Porcelain commands include:
	- `git status`
		- Shows you the status of your repo
		- Shows untracked, unstaged, committed files, etc..
	- `git add <files-to-stage>`
		- Stages files in preparation for committing them
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
	- Worktree: In a repo's `.git/config.worktree`
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
- Essentially, this means that git will apply the base branche's commits one by one starting from the best common ancestor (where both branches diverged), all the way till the tip of the base branch, then it will add the commits of the feature branch on top of it
#### How they compare
- If you mainly rely on merging, then you'll maintain the true form of the history of your repo, but it may get bloated with a bunch of merge commits
- Rebasing doesn't maintain the true history, but it maintains a more linear and cleaner history that's easier to read and work with
- However, under no circumstance should you rebase a public branch onto any other branch, as public branches like div and main are your ultimate source of truth that all other team members rely on, and altering them will disrupt everyone else
# Reset
- Git reset can be used in two forms
	- `git reset --soft <commit_hash>`
	- `git reset --hard <commit_hash>`
- The difference here is that `--soft` will take you back to the previous commit, while keeping your changes uncommitted and staged in the staging area (the worktree), and already uncommitted changes will remain as is, staged or unstaged
- Similarly, `--hard` takes you back to a previous commit, but this time, all changes in the worktree, and any commit after the one you're going back to, are discarded
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