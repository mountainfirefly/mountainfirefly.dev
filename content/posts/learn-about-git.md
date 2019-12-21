---
title: "Learn about Git Core Concepts"
date: "2019-12-21T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/learn-about-git/"
category: "Git"
tags:
  - "Programming"
  - "Javascript"
  - "git"
  - "github"
  - "branching"
  - "commit"
  - "version control"
description: "In this post, we will learn about conceptual areas of git and how we can install git on your system. And concluding this article with usage of basic git commands."
---
![GitHUB](/media/post18-image1.png)

### What is git?
Git is a version control system and a VCS enables us to record changes over time.

### What is git commit graph?
Git commit graph a list of snapshots/commits that has taken by user over time. User can always go back to specific commit and check it out.

Usually we make a commit when a logic of unit is done.

### Git three conceptual areas?
- #### Working Area
  It is what we see on our file system. We add, delete, and edit files in working tree. Here we have full control on it.
- #### Staging Area or Index:
  It is area where user can add files or dirs from working area to the staging area and commit those files that is in the staging area. With this user has freedom to commit specific files and dirs he wants.
- #### History
  It is equivalent to git commit graph, where we see list of commits that has made by the user over time. This history is kept in the hidden directory .git and if you share this .gt dir with other user, he’ll have full control over repository with all the versions of it.

### Install Git
Before your start using git, you have to make it available on your system.
- **MAC OS**:
```zsh
$ brew install git
```
If you don't have brew install on your system, follow this install [Homebrew](https://brew.sh/).
- **Ubuntu**:
```zsh
$ sudo apt-get update
$ sudo apt-get install git
```

Verify the installation was successful by typing ```git --version```:
```zsh
$ git --version
git version 2.9.2
```

### Configure Git username and email (Administrator task)
Whenever a user make a commit it adds username, email, and timestamp to the commit by this, other users can see git who has the made the commit and when.

Now the username and email will be used by every repos user has it on its system. The user shouldn’t have to do it again. If you happen to needed a different name for specific repo and all you need to do it add local tag.

Global config:
```zsh
$ git config --global user.name "mountainfirefly"
$ git config --global user.email "pankajdcoder@gmail.com"
```

For local repo config, use:
```zsh
$ git config user.name "mountainfirefly"
$ git config user.email "pankajdcoder@gmail.com"
```

We have made git available to our system, now will see how we can use it. For that, let's create a dir inside your system.

```zsh
mkdir git-testing
cd git-testing
```

### `git init` command
We will use git init command to create an empty git repository for our git-testing project. you can do it by running `git init` command under `git-testing` dir.

```zsh
$ git init
Initialized existing Git repository in /${PATH}/git-testing/.git/
```
Executing `git init` will creates a `.git` subdirectory under your current working directory, which contains all the necesarry meta-data for new repository.

### `git status` command
It tells us how things stands in our working tree and in the staging area.

```zsh
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```
In our current case we don't have anything in working tree and staging area.

### `git add` command
It is used to add files from the working tree to the staging area. Let create a new file to see how does this `git add` command works.

To create a file and put a same text executes these commands:
```zsh
$ touch test.txt
$ echo Hello World
```

Now if you execute `git status` command git will display a different output.

```zsh
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	test.txt

nothing added to commit but untracked files present (use "git add" to track)
```
Right now the git is showing our file under untracked file because we haven't added it to the staging area. To track down this file all you need to do it execute `git add` command.

```zsh
git add test.txt
```
By executing this command, git will add our `test.txt` file to the staging area. Let's run `git status` command to see it.

```zsh
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   test.txt

```
So this is our staging area, where it is test.txt file and it is also showing command to unstage this file. Git is really being helpful here.

To commit our changes we will use `git commit` command.

### `git commit` command
Git commit command create the commit with whatever is in the staging area.

For use it is just `test.txt` file

```zsh
$ git commit -m "Add file test.txt"
[master (root-commit) ab49f67] Add file test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
```
Above you can see we have used the `-m` option, this is for short message and which is used to described what have being changed.

Now we have first commit in our project and you can change the text of `test.txt` file and it will again display under the staging area.

```zsh
$ echo Hello to the world >> test.txt
$ cat test.txt
Hello World
Hello to the world
```

Run `git status` to see the difference.

```zsh
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   test.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
Here, again it is displaying in the staging area because we have made the change under `test.txt` file. Now this file is getting tracked.


To add and commit execute these commands.
```
$ git add test.txt
$ git commit -m "update test.txt file"
[master 358a518] update test.txt file
 1 file changed, 1 insertion(+)
```
In the output it is showing one file has changes and once insertion has happened.

This is all for this article, in the next article we will learn how to add remote origin to our local git repository and push the local changes to the remote repository.

I hope you must have enjoyed this article. I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev)
