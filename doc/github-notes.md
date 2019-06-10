# Github Process Workflow

## Useful Links
https://help.github.com/categories/administering-a-repository/
https://help.github.com/categories/collaborating-with-issues-and-pull-requests/
https://blog.jetbrains.com/idea/2018/10/intellij-idea-2018-3-eap-github-pull-requests-and-more/
https://embeddedartistry.com/blog/2017/12/21/jenkins-kick-off-a-ci-build-with-github-push-notifications
https://medium.com/@mreigen/integrate-jenkins-builds-into-github-pull-requests-33bc053d6210

## Development Models
See [here](https://help.github.com/articles/about-collaborative-development-models/) for more information.

### Fork and Pull Model
For collaboration on public open source projects. People *Fork* (qv) the whole repository (thus creating a new repo),
work on changes, then submit a *Pull Request* (qv) to the original repository's maintainer. The collaborators do not
need access to the original project's repository.

This approach is probably not suited to us (and is not intended for development teams).

### Shared Repository Model
All colaborators have access to the repository and may push their development branches directly to the single repo.
In this model, *Pull Requests* (qv) are made before merging branches in the one repo.

This is the model for small teams and private collaborations.


## Recomended Workflow

The Recomended Repo topology is:
- a single "base" branch -- traditionally called "develop" or "master" or some such name. Only admin has direct push/merge authority to this branch.
- unlimited user "feature" branches. There is no hard rule about what these branches are named. But I *strongly* suggest the following rules:
	1. the branch name must start with the developer's name. eg: `iainb/feature`
	2. feature branches are *always* deleted when merged into the base branch (do not use rebase merging)
	3. feature branches that have not been touched for a while are either "archived" (renamed `z-archive/original-name`) or simply deleted from Github.

The workflow is as follows (according to Jetbrains, this whole workflow is available with Intellij as of IDEA 2018.3):

1. Update your local base branch:
   `git checkout base-branch`
   `git pull`

2. Branch off the base branch to develop your feature:
   `git checkout -b iainb/new-feature`

3. Develop the feature on branch `iainb/new-feature`, make many commits and push them to github (on that branch!)

4. Your feature is complete, you like it and local unit-tests pass. Prepare your branch for a PR review:

	i. Resolve merge conflicts with the base branch (this can be done at any stage from now on, but is easiest here):
    ```
    git checkout master
    git pull
    git checkout iainb/new-feature
    git merge master
    ```
  This grabs all the changes pushed to `base-branch` while you have been working on your feature and forces you to resolve merge conflicts.
  Once this is done, your feature branch is fully compatible with the latest base branch
  `git push`

5. The branch is now ready for PR. Go to Github and create the PR (should offer you a pop-up to create a pr of the latest branch push).
   Please, please fill in the PR text field (we can provide a template for this and Github Markdown is supported). Reviewers *should not*
   pass badly documented PR's. Remember that the PR is a audit document!

   Once you have explained why the feature branch should be included in the base, append reviewers (on the right-hand-side of the GUI) who are
   relevant to this feature and submit the PR. The reviewers will be notified and the automated check process will start.

6. **Cleanup:**
   Once you have submitted the PR, it will automatically run the required checks. If it fails *it is up to you* to readthe unit-test output and
   correct it. You can continue to push commits to the feature branch. Each push will summon the automated required checks and re-test your changes.

   Please read the reviewers' comments. A reviewer can append line by line comments in the diff and request specific changes or clarification. It is
   up to the coder and the reviewer to forge a good working relationship. No reviewer approval, no code merging. It's that simple.

   Once the reviewers are happy, and the required checks pass. You can merge the branch (the **big** green button! Push the big green button!). When prompted, "delete the feature branch" This erases the branch from github. Please do this, it reduces clutter.

   Now go back to step 1 and squash the next bug!

   NB: It is up to you to maintain the branches on your own machine. You can delete merged branches or let them pile up, nobody cares, it's your computer.
   To delete branches locally use: `git branch -D branch-name` (you cannot delete the currently checked out branch!)
   To delete branches from github: `git push origin --delete branch-name` **This is NOT recoverable!** and permissions may be refused!



## Github Terminology

- **Pull Request (PR)**: see (here)[https://help.github.com/articles/about-pull-requests/] for all about PRs.
  - A Pull Request is a request to merge your changes to another branch.
    Github has a number of features that can be automated into PR generation:
  	  - (Required Status Checks)[https://help.github.com/articles/about-required-status-checks/]: Here you can check build, syntax and unit test completion.
      - Jenkins has first class Github (Support)[https://resources.github.com/whitepapers/practical-guide-to-CI-with-Jenkins-and-GitHub/]
      - status checks can be "loose" or "strict". Strict checks require the merging branch be up-to-date with the base branch.
  - It is important to note that PR's are also an audit trail -- making it much easier to find when and where the code broke down.
  - Once a PR is created, you can continue to make commits to the PR feature branch. Pushing such a commit to Github causes the automated checks to be re-run.

- **Fork**: a personal copy of someone else's project. This retains ties to the original project and it remains possible to submit a PR to that project.
  It is really intended for making major contributions to someone else's project.


## Pull Request Merge Methods
https://help.github.com/articles/about-merge-methods-on-github/

- **Merge PR** (Default): once all checks have passed and the "merge PR" button is pressed, all commits from the feature branch are added to the base branch in a git merge
- **Squash and Merge PR**: the feature branch commits are squashed into single commit and then merged via fast-forward. This condenses the history on the feature branch, but makes the base branch cleaner.
- **Rebase and Merge PR**: all commits from feature branch are added to the base branch individually without merge commit -- this is *not* recomended in this context (see ["consider these disadvantages"](https://help.github.com/articles/about-merge-methods-on-github/))

