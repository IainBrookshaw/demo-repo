# Basic Git Workflow

## Some Git Concepts
- files are either:
	- __untracked__: they are new or have not yet been checked into the repo
	- __modified__: some change has been made (edit, move, delete) since the last commit
	- __staged__: tagged for inclusion in the next commit (in the state they were in when staged!)

## Some Git Resources

- `git command -h`
	- the `help` for any git `command`

- [The Official Git book](https://git-scm.com/book/en/v1/Getting-Started-Git-Basics)
- [Atlassian (bitbucket) documentation](https://www.atlassian.com/git)
- [Github documentation](https://guides.github.com/)
- [Gitlab documentation](https://docs.gitlab.com/ee/topics/git/)

- __When in doubt, ask!__

--------------------------------------------------------------------------------
## Some Useful Commands With Explinations

### `git status`
- what is the current status of my repo?
- this will list files as:
	- files that have been changed, but not staged for commit
	- files that have been modified
	- untracked files
- It is __strongly__ recomend to do this regularly!
	- do __not__ allow cruft to accumulate in your workspace

### `git log`
- show me the history -- all commits from this point back in time.
- make note of the SHA: the commit id (eg: 168b820377a2df68af196c0f844be7c75bb30424)
	- you can use this to navigate through the tree and as the argument to other commands

- `git log --graph` prints an ascii tree of the repo's history

### `git add`
- the command to stage files for commit. There are several ways of doing this:

1. `git add path/to/file`
	- this explicitly adds __that__ file to the staging zone

2. `git add -u`
	- this stages all tracked and modified files for commit
	- this is a good way of adding changes you've made for commit without inadvertantly adding new files

3. `git add -A`
	- __dangerous__: this will add __all__ unstaged files (including untracked files)

4. `git reset`
	- this "undoes" your adds and unstages everything
	- `git reset path/.to/file` resets a specific file

### `git diff`
- compute the difference between two objects
	- `git diff branch1 branch2`
		- the complete diff between `branch1` and `branch2`
	- `git diff -–name-only branch1 branch2`
		- the git diff (filenames only) between `branch1` and `branch2`


### `git checkout`
- how you switch between branches and create new branches
- here are some examples:
	1. `git checkout branch-name`
		- checkout the branch `branch-name` (ie: change from current branch to `branch-name`)
		- note that this will modify your repo to look like the latest commit in `branch-name` and may not be possible if you have

	2. `git checkout 6478b2c0499b9f4c36b3d291e5456b445297e598`
		- checkout a specific commit
		- this will checkout in a "detached head" state, you cannot commit in this state (as you are now back in history)
			- but you can create a new branch from this point and continue working there

	3. `git checkout -b new-branch-name`
		- this will create a new branch from the current commit with name `new-branch-name`
		- note that this creation is only on your local machine. You have to `git push` from this branch to push
		it to github

	4. `git checkout branch -- path/to/target/file/or/directory`
		- this will checkout `target/file/or/directory` from the branch `branch`
		- this is useful to restore a subset of your filesystem tree to another branch's state

### `git branch`
- `git branch` no arguments simply lists all the branches and highlights the current one
- `git branch -D branch-name` __deletes__ the branch `branch-name` from your __local__ repo. Be __very__ sure you want to do this.


### `git diff`
- get a difference between files, branches or commits
- `git diff --name-only branch-name` -- get the files that change between this branch and `branch-name`
- `git diff 6478b2c0499b9f4c36b3d291e5456b445297e598` -- get the differences between "now" and commit 6478b...
- you can also diff specific files, and all combinations
	- some simple googling can reveal more syntax and options
	- `| grep` can also be your friend if the diff gets overwhelming


--------------------------------------------------------------------------------

## A Simple Workfow
### Creating and mergin a new feature

1. Get an updated local master:
	```
	git checkout master
	git pull
	```

2. Create a new branch for your feature:
	- `git checkout -b my-new-feature`

	a. Edit some files:
	 - `git add file-path` or...
 	 - `git add -u # this will add all modified tracked files`

	b. Create some new files
		`git add new-file-path`

	c. make a commit:
	- `git status` -- see what is staged, what is modifed and what is untracked
	- add or remove files from the staging area with `git add` and `git reset`
	- `git status` -- do it again and be sure you're happy with what you're going to commit
	- `git commit -m "My descriptive commit message"`
		- note: if you do not use the `-m` (inline message) option, Git will enter which ever command-line based text editor you have set as default for you to type your commit message.
			- Vim/Vi is a popular choice, but it's really up to you

	- you have made a commit, now push to github:
		- `git push -u origin my-new-branch-name`
		- note: if you simply run `git push` it will stop and warn you that your branch isn't in the origin. It will also give you the above command. Git can be helpful that way.

3. Readying your Pull Request:

	a. update your local master:
	 ```
	 git checkout master
	 git pull
	 ```

	b. Merge your local master with your working feature branch:
	```
	git checkout my-feature-branch
	git merge master
	```
	- note: you can compress these steps by merging `origin master` into your feature branch. I like to do one thing at a time, but it is up to you
	- You may need to resolve merge conflicts at this time.
	- Doing this regularly as you work is often a good idea
	- `rebase` is also an option here. I find this more intimidating, so I recomend merge

	c. push the resultant merged `my-feature-branch` to github

	d. create your PR in the Github web UI and await review

	f. once the PR is Ok'd merge using the Github web UI (the button labled "merge").
	- You can choose to delete `my-feature-branch` from the Github repo at this time too... that's up to you