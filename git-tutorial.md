# Learning to use Git effectively
By Audrey Beard

Contents:
1. [Cheat Sheet](#cheat-sheet)
1. [What's Git, and why use it?](#whats-git-and-why-use-it)
1. [Using it](#using-it)
    1. [Prerequisites](#prerequisites)
    1. [Overview](#overview)
    1. [If you're working on your own project](#if-youre-working-on-your-own-project)
    1. [If you're working on a team](#if-youre-working-on-a-team)
    1. [Forking](#forking)
    1. [Fixing Things](#fixing-things)
1. [Best Practices](#best-practices)

## Cheat Sheet
Below you'll find all of the most common commands you'll use on a day-to-day basis

Command | Example | Use Case
--------|---------|---------
`pull` | `git pull` | Fetches new changes from remote branch (on GitHub/GitLab), AND updates current branch (on PC)
`clone` | `git clone <YOUR_URL>` | Clones a repository from `<YOUR_URL>` onto your local machine
`add` | `git add <FILE_OR_FILES_OR_DIR>` | Tells Git to track changes in `<FILE_OR_FILES_OR_DIR>`. Can supply one file, multiple files, or a directory (like `.` or `..` or `./dir`)
`commit` | `git commit -m “<MESSAGE>”` | Commits your changes to the official record. This is the smallest chunk of time/changes that Git knows, so it’s valuable to commit often
`push` | `git push` | Pushes your local commits to the remote (GitHub/GitLab). If this is your first time pushing a new branch, you’ll have to add `--set-upstream origin <BRANCH>`
`branch` | `git branch <BRANCH>` | Creates a new branch. If you just want to see which branches exist on your machine, leave off `<BRANCH>`
`checkout` | `git checkout <BRANCH>` | Checks out a specified branch
`status` | `git status` | Displays various statuses, like untracked files or uncommitted changes
`merge` | `git merge <OTHER_BRANCH> -m “<MESSAGE>”` | Merge changes from `<OTHER_BRANCH>` to your current branch and commits. Adds `<MESSAGE>` to commit

## What's Git, and why use it?
Git is a version control system, or VCS. If you've used G--gle Docs before and
looked at the "Version History" of a document, you've seen version control in
action. Git is the most popular way of doing version control for software
developers, data scientists, and a lot of folx doing creative work with their
computers. GitHub and GitLab are sites for hosting and syncing projects with Git -- they
rely on Git, but they're not Git. They're also not necessary for Git to work,
though they make some things easier.

If you're here, you probably see the value in Git (and a Git-oriented site), but in case you don't:

- Git allows you to fix big mess-ups by reverting to an older "commit"
- Git makes collaboration a lot easier to do (and do effectively)
- Git makes it a lot easier to "brach" or "fork" off someone's project and do your own version of it
- GitHub or GitLab make sharing your work a lot easier
- GitHub or GitLab trivializes backups of your work

## Using it

### Prerequisites
Before starting this tutorial, you'll need a few things:
- A GitHub or GitLab account
- EITHER:
  1. A computer terminal pulled up and Git installed:
      - Protip: You can check if a program `X` is installed with `which X`, which
        should return something like `/usr/bin/git` on Mac or Linux, so you could
        do `which brew` and `which git` to check if you have either installed.
      - Mac:
        - Open the `Terminal` app
        - First install [Homebrew](https://brew.sh/)
        - Then install Git:
          - `brew install git`
      - Ubuntu:
        - Open the `Terminal` app
        - Install Git (if you don't have it):
            - `sudo apt install git`
      - Windows:
        - Set up [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10) (WSL) and install Ubuntu
        - Open the WSL app (might also be called "bash")
        - Install Git (if you don't have it):
            - `sudo apt install git`
  1. [GitHub Desktop](https://desktop.github.com/) 
      - This makes it easy to use Git if you're not interested in using the command line, or if you prefer graphical user interfaces (GUI's)
      - TODO

### Overview
The way I usually use Git follows the pattern of "pull, make changes, commit, push." That is:

1. `pull`
    - update your local code (on the current branch) with the remote machine's code on the same branch (we'll get to branches later)
2. Make changes on your own machine, do some testing, throw a brick, etc.
3. `commit`
    - When you've finished working on something (a feature, a chapter, a piece, whatever) `commit` it to Git's log - this lets you come back to it later
4. `push`
    - send your local `commit`s to the remote `master` branch (often on GitHub or GitLab)

This is the general workflow, but your usage could be different depending on
whether you're collaborating, what your role is, and what you want to do.

Unless otherwise noted, every time you see a command like `this`, you should
put it into a terminal verbatim, based on the use case. Placeholders look
`<LIKE_THIS>` and should be replaced with whatever specific thing you're doing.

### If you're working on your own project
If you're backing up your own project, you might not need different branches at
all - I'll assume that's the case here. If you'd like to learn more about
branches, check out the section about [working on a team](#if-youre-working-on-a-team).

#### Starting a project
Before you get started working on your project, you should figure out where you
want to host it (i.e. where you back it up, and publish it if you make it public).
1. Create a repo on the site, like GitHub.com or GitLab.com (only need to do this once)
1. In a terminal, clone the repo to your machine, wherever you want it (only need to do this once):
    - `git clone <YOUR_REPO_URL>`

#### Working on a project on one computer
This is the simplest use case -- you're only doing work on one machine, so
you're using a Git site purely as a backup and/or portfolio:

1. Create your files, make your changes, etc.
1. Add any files to Git that aren't already added:
    - `git add <YOUR_FILE_NAME> <YOUR_NEXT_FILES_OPTIONALLY>`
    - Alternatively, track everything in this directory with `git add .`
1. Commit your changes so you can keep track of it (and revert if you mess something up):
    - `git commit -am <YOUR_COMMIT_MESSAGE_WITH_QUOTES>`
1. Push your changes to the remote repository
    - `git push` 

#### Working on a project on multiple computers
Using Git with multiple computers is really where the `pull`-`push` pattern
comes into play. In short, always pull new changes onto your local machine
before pushing, and always push your changes if you know you're going to be
working on a different machine later.
1. Pull any remote updates into your local `master` branch:
    - `git pull`
1. Create your files, make your changes, etc.
1. Add any files to Git that aren't already added:
    - `git add <YOUR_FILE_NAME> <YOUR_NEXT_FILES_OPTIONALLY>`
1. Commit all your changes so you can keep track of it (and revert if you mess something up):
    - `git commit -am <YOUR_COMMIT_MESSAGE_WITH_QUOTES>`
1. Push your changes to the remote repository
    - `git push` 

### If you're working on a team
Generally, if you're working on a team, you each have your own branch (or maybe more). Different teams have different protocols for updating the `master` branch, and I'll address some of those here.

#### You're a brand new contributor on a project
Before you do anything, you'll need to get the code and probably make your own
branch. This makes it easier to track and merge changes, especially if two
people work on the same file (which risks merge conflicts).
1. Clone the repo from remote to your local machine
    - `git clone <YOUR_REPO_URL>`
1. Create a new branch on your machine
    - `git branch <YOUR_BRANCH_NAME>`
1. Check out that branch
    - `git checkout <YOUR_BRANCH_NAME>`
1. Make some changes to your branch
1. Track all new changes in this directory (AKA folder)
    - `git add <YOUR_FILE_NAME> <YOUR_NEXT_FILES_OPTIONALLY>`
    - Alternatively, track everything in this directory with `git add .`
1. Commit those changes to Git’s working tree
    - `git commit -am <YOUR_COMMIT_MESSAGE_WITH_QUOTES>`
1. Push your branch to remote by setting the upstream branch
    - `git push --set-upstream origin <YOUR_BRANCH_NAME>`


#### Working on an existing branch, making unrelated changes
If you've already got a clone of the branch on your machine, and you need to
make a few unrelated changes, you probably want to give each major change a
meaningful commit message. Note that this part of the tutorial shouldn't be
entered verbatim, but shows an example instead:
1. Make sure you’re on your branch
    - `git checkout <YOUR_BRANCH>`
1. Make sure your branch is up-to-date:
    - `git pull`
1. Make changes to some file (`seize_the_means.py`)
1. Add those changes to the to-be-committed list
    - `git add seize_the_means.py`
1. Write new commit message for clarity, accountability, and easy fixes if
   something breaks. Use a meaningful commit message, like this example!
    - `git commit -m “wrote multiprocessing function to unite the workers”`
1. Suppose now we deleted a file that we don't actually need (`landlords.py`)
1. Remove that file from Git’s tracking
    - `git rm landlords.py`
1. Commit that change, giving it a meaningful commit message
    - `git commit -m “decided to cancel rent”`
1. Push all your changes from local to remote
    - `git push`


#### Existing branch, merging changes from another branch
If you're working with a team, chances are you're working on different things
that might rely on each other. For this reason, it's a good idea to update your
local development branch with updates from a different branch. Note that I also
am using example commit messages and file names here, so don't copy that
verbatim:
1. Check out other branch
    - `git checkout <OTHER_BRANCH>`
1. Pull remote changes into local machine
    - `git pull`
1. Switch back to your branch
    - `git checkout <YOUR_BRANCH>`
1. Make sure your branch is up-to-date:
    - `git pull`
1. Merge other branch into your branch. Use meaningful commit message!
    - `git merge <OTHER_BRANCH> -m “bring in Lamya's bug fixes”`
1. Make changes to file (`facilitation_guide.md`)
1. Add those changes to the to-be-committed list
    - `git add facilitation_guide.md`
1. Write new commit message for clarity, accountability, and easy fixes if something breaks. Use a meaningful commit message, like this example.
    - `git commit -m “finished writing the camp facilitation guide!”`
1. Push all your changes from local to remote
    - `git push`


#### Pull requests
Many teams don't like when members push directly to `master`, so they either ask (or force) their members to submit pull requests (called merge requests by GitLab) in order to update the `master` branch. They may do this to make sure changes are necessary, working, or reviewed by a PM or someone. 
1. After you push to your remote branch, navigate to your repo
1. Next step depends on your site:
    - GitHub:
        - "Pull requests"
        - Under the repo name near the top of the page
    - GitLab:
        - "Merge Requests"
        - About 1/3 of the way down the sidebar on the left


### Forking
You might see a project that you want to adapt for your own purposes. Do do
that, you can navigate to that repo on GitHub or GitLab, then click the "Fork"
button on the upper right


### Fixing Things
You might (probably will, tbh) end up committing something you didn't want to.
If you just need to revert back to your previous commit, you can reset the
branch you're on by one using `git reset HEAD~`.

1. Suppose you make some serious mistake:
    - `git add capitalism.py`
    - `git commit -m "capitalism works"`
1. You realized your last commit was garbo, so you panickedly reset it:
    - `git reset HEAD~`
1. Make whatever changes you wanted, etc.
1. Commit those changes with the commit message you're actually using
    - `git commit -am "capitalism doesn't actually work we should just delete it"`
1. Push those changes to remote
    - `git push`


## Best Practices
### Commit often
I know it’s a pain to commit all the time, but it’s the best way to keep track
of changes. Don’t feel the need to do it every change, but if you work on one
feature or function, it’s best to just commit that change before starting
something new.

### Don’t push to master (unless you’re the only developer)
Pushing to master makes changes really hard to track and manage. If you want to
put your stuff in the master branch, submit a pull request

### Pull before you push
If your remote branch was changed, you might not be able to push before you
pull. Even if you can, Git might automatically merge those changes in an
unexpected way. It’s best practice to pull, and verify any changes, before you
push to remote

