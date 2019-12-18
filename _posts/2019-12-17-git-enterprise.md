---
layout: post
title: "How I GIT in the Enterprise"
date: 2019-12-17 12:00:00
published: true
categories: posts
tags:  git enterprise feature branch workflow
---

## How I GIT in the Enterprise

Within a few large Enterprises that I have had the chance to work at, a mature GIT forking-github strategy has been rare to find. I encourage my colleauges to contribute and become involved with OSS projects to help mature their GIT-foo. Below are a assorted collection of GIT tips that you would more likely find on an Enterprise team working in a Feature-branch workflow.
This post will change over time. Last updated 12/17/19.

*a recommended development workflow for GIT*

A Git Workflow is a recipe or recommendation for how to use Git to accomplish work in a consistent and productive manner. Git workflows encourage users to leverage Git effectively and consistently.

In an effort to keep repositories clean and the master branch the releasable/error free branch, I had adopted the [Git Feature Branch Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)

> The core idea behind the Feature Branch Workflow is that all feature development should take place in a dedicated branch instead of the master branch. This encapsulation makes it easy for multiple developers to work on a particular feature without disturbing the main codebase. It also means the master branch will never contain broken code, which is a huge advantage for continuous integration environments.

The Feature Branch Workflow assumes a central repository, and master represents the official project history. Instead of committing directly on their local master branch, developers create a new branch every time they start work on a new feature. Feature branches should have descriptive names.

## Recommended Tools

- [SourceTree](https://www.sourcetreeapp.com/)
    - Sourcetree simplifies how you interact with your Git repositories so you can focus on coding. Visualize and manage your repositories through Sourcetree's simple Git GUI.


## Step-by-step guide
Using myproject repo as the example

1. Set up and clone the repository locally

    ```bash
    git clone git@gitlab.enterprise.com:MYORG/myproject.git
    ```
2. Check out master

    ```bash
    git checkout master
    ```

3. Update master with a pull - all feature branches should be created off the latest code state of a project

    ```bash
    git pull origin master
    ```

4. Create a new branch - use a descriptive name, start with the Jira ticket for the feature - do not use a number to start the branch name

    ```bash
    git checkout -b MYORG-1234-new-feature
    ```
    This checks out a branch called new-feature based on master, and the -b flag tells Git to create the branch if it doesn’t already exist.

5. Update, add, commit, and push changes - On this feature branch, edit, stage, and commit changes in the usual fashion, building up the feature with as many commits as necessary. In the git message start with the Jira ticket.

    ```bash
    git status
    git add <some-file>
    git commit -m "MYORG-1234 a helpful message"
    ```
6. Updating with Master - as development continues in the feature branch, master branch will also progress with reviewed merge requested code. Keeping the feature branch updated with master code useful to manage any code conflicts early in their development rather than waiting to the end of the feature work and dealing with many conflicts.

    Utilizing [git rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase) during the review and merge stages of a feature branch will create enforce a cohesive Git history of feature merges.

    To update the Feature branch with the latest Master:

    ```bash
    git status
    git checkout master
    git pull origin master
    git checkout MYORG-1234-new-feature
    git rebase master
    ```

    Rebase is one of two Git utilities that specializes in integrating changes from one branch onto another.

    **Watch for conflicts**

    **NEVER PERFORM A "GIT PULL" ON A FEATURE BRANCH FOLLOWING A REBASE**


    At this point, after running the rebase command, it may complete without issue or it will stop the rebase if it runs into conflicts

    - Resolving Conflicts - git may return that conflicts have occurred. At this point, stop and resolve the conflicts. Run a git status and follow the instructions.

    ```bash
    $ git status
    <edit the files listed in the conflict>
    $ git add <some resolved conflicted files>
    $ git rebase --continue
    ```

    this flow may be repeated until all conflicts are resolved. 

    *Do not "`git commit`" while resolving conflicts. Only "`git add`"*

7. (Optional) Rebase interactively - Running `git rebase` with the `-i` flag begins an interactive rebasing session. Instead of blindly moving all of the commits to the new base, interactive rebasing gives you the opportunity to alter individual commits in the process. This lets you clean up history by removing, splitting, and altering an existing series of commits.

    ```bash
    $ git rebase --interactive master
    //this will open an interactive editor:


    pick 2231360 some old commit
    pick ee2adc2 Adds new feature
    # Rebase 2cf755d..ee2adc2 onto 2cf755d (9 commands)
    #
    # Commands:
    # p, pick = use commit
    # r, reword = use commit, but edit the commit message
    # e, edit = use commit, but stop for amending
    # s, squash = use commit, but meld into previous commit
    # f, fixup = like "squash", but discard this commit's log message
    # x, exec = run command (the rest of the line) using shell
    # d, drop = remove commit
    ```

    Use this to drop any unwanted commits or more often to squash commits to create a cleaner history in the Feature branch

8. Finishing Feature branch work - time to push. Once work has completed in the Feature branch, push the branch to the central repository for others to review and to open a Merge Request

    ```bash
    $ git push -u origin MYORG-1234-new-feature
    ```
    This command pushes *MYORG-1234-new-feature* to the central repository (origin), and the `-u` flag adds it as a remote tracking branch. After setting up the tracking branch, you can `git push` without any parameters to push

9. Open a merge request - In your hosted GIT environment navigate to the project -> branches. In myproject example, find your branch and press Merge Request button.

10. Updating branch with Merge Request feedback - sometimes Assignee's or Approver's will give feedback in a Merge Request that will require code updates before the MR is accepted. As well between the time your Merge Request is opened and by the time it is reviewed, other MR's may have been accepted - moving Master branch ahead and causing your branch to fall behind requiring a repeat of the rebase actions.

    ```bash
    $ git status
    $ git checkout MYORG-1234-new-feature
    <make some changes>
    $ add <some-file>
    $ git commit -m "MYORG-1234 a helpful message"
    <need to catch up with master>
    $ git status
    $ git checkout master
    $ git pull origin master
    $ git checkout MYORG-1234-new-feature
    $ git rebase master
    ```
    **Watch for conflicts**

    **NEVER PERFORM A "`GIT PULL`" ON A FEATURE BRANCH FOLLOWING A REBASE**

11. Push updates to your remote branch - you have updated your branch and rebased. Rebase always causes history to be rewritten. The local version of the Feature branch and the remote version of the Feature branch will now have diverged in history. You will need to force push to overwrite the remote Feature branch. We will use a force push with lease as a more cautious push.

    ```bash
    $ git status
    $ git checkout MYORG-1234-new-feature
    $ git push --force-with-lease origin MYORG-1234-new-feature
    ```
    *If anyone is collaborating on the Feature branch, all collaborators will have to remove their local copy of the Feature branch if another user does a rebase/force push*



## Release Branch
Following a Release, a protected branch of that release will be created. The current release branch for v1.0.0 is release/1.0.0

Workflow to know whether to make an update to master or release/1.0.0

- Do you have a production Bug? 
    - No - go back master branch
    - Yes, continue below

Follow all the below steps for creating a Feature Branch from release/1.0.0:

```bash
# check where you are at (master or otherwise)
$ git status
# checkout release branch
$ git checkout release/1.0.0
# make sure you are up-to-date
$ git pull
# branch from release branch
$ git checkout -b MYORG-1234-bug-fix
```

This checks out a branch called MYORG-1234-bug-fix based on release/1.0.0, and the `-b` flag tells Git to create the branch if it doesn’t already exist - please do not use a number to start the branch name

Update, add, commit, and push changes - On this feature branch, edit, stage, and commit changes in the usual fashion, building up the feature with as many commits as necessary.

```bash
git status
git add <some-file>
git commit -m "MYORG-1234 a helpful message"
```

Finishing Feature branch work - time to push. Once work has completed in the Feature branch, push the branch to the central repository for others to review and to open a Merge Request

```bash
$ git push -u origin MYORG-1234-bug-fix
```
This command pushes MYORG-1234-new-feature to the central repository (origin), and the `-u` flag adds it as a remote tracking branch. After setting up the tracking branch, you can git push without any parameters to push

Open a merge request - In your GIT environment navigate to the project -> branches. In myproject example, find your branch and press Merge Request button. This is a merge to release/1.0.0

## Close/Archive Release Branch
Following a major Release, the previous protected branch of the previous release will need to be closed/archived. The current release branch for 1.0.0 is release/1.0.0.

The below steps will be performed by the "release manager".

Depending on your GIT management tools, Gitlab, Github, or Bitbucket may provide some tools to manage this.

Below will be the recommended release branch archive flow that is GIT-neutral. This will use branch release/1.0.0 as the example of the branch to be archived.

- Merge Release to Master
    
    This process assumes you have already merged release to master. There should be no more commits on release branch beyond the merge commit to master. 

    ```bash
    $ git checkout master
    $ git merge release/1.0.0 --no-ff
    ```
- Create an archive tag from a branch

    Git tags aren't too different from branches. They are a point-in-time backup of the repository at a specific commit level or in the case of this workflow a specific branch. The  naming convention to make finding the archive tags easier, we chose the name archive/[branchname]

    ```bash
    $ git tag archive/release/1.0.0 release/1.0.0
    ```

- Deleting a branch from the local Git working copy
    Deleting a branch from Git is really simple but there are two prerequisites. You must have already merged the branches commits to another branch such as master, and if the local branch is configured to track against a remote Git server you must have pushed all local commits to the origin. With these two steps out of the way you can delete the local Git branch with `git branch -d [branchname]`

    ```bash
    $ git branch -d release/1.0.0
    ```
- Deleting a branch from the remote Git origin

    For this operation, log into the GIT environment. Go to the project repository, go to branches and find release/1.0.0 branch. Click to Delete the branch. A repository maintainer may have to remove the protected locks on the branch.

- Pushing your new Git tags to the origin server
    ```bash
    $ git push --tags
    ```

- Restoring a tagged/archived branch

    If you ever need to restore an archived branch you have tagged and deleted you can do so with the Git checkout command. The -b flag tells Git to create a new branch and check it out in the same step. The first branch name is the name of the new branch, and the second is the name of the tag to use as the source for the new branch.

    ```bash
    $ git checkout -b release/1.0.0 archive/release/1.0.0
    ```

## Other Topics
### Squash all commits by index reset

Typically squashing commits is done by an interactive rebase (`rebase -i`). Another way to do this is via a soft reset of your branch and creating a new commit. ***This is an advanced use, be careful***.

```bash
$ git checkout yourBranch
$ git reset $(git merge-base master yourBranch)
$ git add -A
$ git commit -m "one commit on yourBranch"
```
Follow with a `git push --force` to really reset your branch. If a rebase has become unmanageable you can use the above to reset your branch. Be careful not to delete any of your work. After the reset and before the add, you can switch to another new branch to try out your changes - likely will have to use a stash.

### Branch Creator
For the last commit author in each branch (roughly assuming they are the branch creator). Change directory into the GIT repository in question and run:

```bash
$ git for-each-ref --format='%(color:cyan)%(authordate:format:%m/%d/%Y %I:%M %p)    %(align:25,left)%(color:yellow)%(authorname)%(end) %(color:reset)%(refname:strip=3)' --sort=authordate refs/remotes
```





## Related Links

- [The Git Rebase Introduction I Wish I'd Had](https://dev.to/maxwell_dev/the-git-rebase-introduction-i-wish-id-had)
- [Git Feature Branch Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)
- [git rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)
[split a git repo](https://help.github.com/en/github/using-git/splitting-a-subfolder-out-into-a-new-repository)
- [Force push with lease](https://blog.developer.atlassian.com/force-with-lease/)