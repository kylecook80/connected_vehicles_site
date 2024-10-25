---
title: Using Git
author: Kyle Cook
date: \today
indent: "false"
---

# Introduction

Version control software is very helpful in managing your code. We will focus on using **git**.

A **Version Control System** is a system that records changes to a file or set of files over time so that you can recall specific versions later.
Can be used to revert to a previous state, recover work, compare changes over time, or decide when an issue was introduced.

Types of version control:

- Local: Local database that stores files. Changes are recorded as "patches" over time. Difficult to collaborate with other developers.
- Centralized: A central server maintains all versions of files. Allows admins fine-grained control over who has access to what and what they are working on. However, a central server can be a single point of failure. Individual files are checked out to developers, not the entire repository.
- Distributed: Fully mirrors the repository on each users machine. Every backup is a full clone of the repo. Can work well with multiple remote systems.

# Quick Start

To get going quickly, you first need to configure git. Follow the `Git setup` section.

## Create New Repo

To create a new repo, run `git init` in the directory you want to become a git repository.
Modify your files as usual, then then run `git add .` to add your changes.
Then you can save your changes with `git commit -m "Message here"`.
Be sure to change "Message here" to a description of what you changed.

## Clone a repo from Github

To clone an already existing repo from Github, you must first setup your SSH key and attach it to your Github profile.

Inside of your Unix environment (Ubuntu on WSL or the Terminal app in macOS), type

```
ssh-keygen -t ed25519
```

Leave the fields blank and hit enter until you get back to a shell prompt.

[](/images/wsl_ssh_keygen.png)

Now, go to https://www.github.com, create an account if you don't already have one, and then click on your profile in the top right corner to get the following menu

![](/images/github_profile.png)

Click on settings. Then on the left hand side, click on SSH and GPG Keys.

![](/images/github_profile_options.png)

Click the "New SSH key" button. Add a title (doesn't matter).

Now, go back to your terminal and type `cat .ssh/id_ed25519.pub`. Copy what is displayed into the "Key" section on the Github website.

On Github, go to the repository you want to clone.

![](/images/repo_clone.png)

Under the "SSH" tab, copy the URL.

Now, in your terminal type:

```
git clone <url>
```

where "\<URL\>" is the url you copied from Github.

The project should now be cloned.

---

# Git Basics

Git stores data as snapshots, not as differences. Almost every operation is local, which makes it easy to continue working if internet access is unavailable. Git has inherent integrity in every operation it does. Every file, commit, etc are checksummed using SHA-1. Git typically only adds data. It is very difficult to change or remove data in the git log.

Three Main States of Git:

- **Commited** files are safely stored in the git database.
- **Modified** files are those that have changed, but not committed to the database yet.
- **Staged** files are files that have been marked in its current version to go into the next commit snapshot.

The **`.git`** directory is where metadata and objects are stored. This is the part that is copied when you clone from another system.

The **working directory** is a single checkout of one version of the project. Files are decompressed from the git database and put into the folder for usage or modification.

The **staging** area stores information about the next commit. Also referred to as the **Index**.

## Git setup

The user's identity is important in git.
Your identity is stored in the commits you make so that you know who made them when sharing work with others.

To configure your information do the following:

```bash
git config --global user.name "Your Name"
git config --global user.email email@domain.com
```

## Common commands

`git init` - Create new git repository.

`git add .` - Add all changes to the staging area.

`git add <file>` - Adds a new file or a modified file to the staging area.

`git commit -m "Message"` - Save all staged files to a new commit.

`git clone https://www.github.com/git/git` - Clone a repo (in this case, the official git source code).

`git status` - View the current status of the repo.

`git diff` - Show the difference between the staging area and the last commit.

`git rm <file>` - Remove a file and add its removal to the staging area for commit.

`git rm --cached <file>` - Removes a file from the staging area and leaves the working directory alone.

`git log` - Show the git commit log.

`git fetch` - Fetch the latest commits, but do not check them out

`git pull` - Fetch the latest commits and also check them out

`git push` - Push your commits to a remote

### Danger

`git commit --amend` - Amend the last commit.

`git reset HEAD <file>` - Reset a file to its version from the last commit.

## Tags

Two types of tags:

- **Lightweight tags** are just a pointer to a particular commit.
- **Annotated tags** contain metadata, authors, and other data just like any other commit, but it just points to a commit.

Tags must be explicitly pushed to the server.

Checking out a tag and making changes is a bad idea.

## Branching

A **branch** is a new pointer that points to some commit. The master/main branch is not special, it is just like any other branch. A pointer called HEAD points to whatever branch/commit is currently checked out.

```bash
git branch testing
git checkout testing
git merge otherBranch
```

### Merge conflicts

If the same part of a file was edited in two difference branches, a conflict occurs. The merge process is then paused. The user must manually fix the issue and then restart the merge. Once the conflict has been fixed, stage the file, and then commit. The merge then continues as before.

Different Git branching workflows:

- Long-running branches: branches that live indefinitely, usually indicating some level of stability. Multiple branches merging into the same branches repeatedly.
- Topic branches: branches dedicated to one particular feature or bugfix.
- Remote branches: branches that you cannot change. Indicates the the state of a remote repo at the last time you communicated with it.
- Tracked branches: local branches that have an association with a remote branch.

`git fetch` merely updates the remote branches, but does not change the working tree at all. Merging is done by the user.

`git pull` fetches the remote data, but automatically attempts to merge it for you.

### Rebasing

Rebasing replays commits on top of each other instead of merging them.
Results are the same as a merge, but it makes for a cleaner history, and possibly and easier time if there are a lot of merge conflicts. Rebasing is common when contributing code to a project that you don't maintain.

> **Perils of rebasing**:
>
> DO NOT REBASE COMMITS THAT EXIST OUTSIDE YOUR REPOSITORY.
> Do not ever force push a repo to another system without proper verification that it is absolutely necessary. Git gives you warnings and errors for a good reason.

## Github

After you've mastered git for local repositories, you can also learn about storing your code repositories on a git server.
One of the most popular services is GitHub.
While these are often treated as synonymous, they are not.
Sign up for a github for education account for free using your utulsa email address if you don't have one already on github.com.

## SSH Keys

I find it best to interact with GitHub using SSH and a linked SSH key.
This means you will not need to enter a password every time you want to clone a private repo.
If you have not already created an SSH key on your local computer, you can follow the following guide to make on (it is essentially a one liner in your Terminal):
https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

Once you have created an SSH key on your local machine, you can add it to your GitHub account following these instructions:
https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
