---
title: "Git Branching and Merging"
date: "2019-12-29T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/git-branching-and-merging/"
category: "Git"
tags:
  - "Programming"
  - "Javascript"
  - "git"
  - "github"
  - "branching"
  - "commit"
  - "merge"
  - "version control"
description: "In this post will be learning about all the necessary features of git that are required for git branching and merging."
---
![GitHUB](/media/post18-image1.png)

In my last post, I have covered the core concepts of git and introduction to the basic git commands you can check it out [here](https://mountainfirefly.dev/posts/learn-about-git/).

In this post will be learning about all the necessary features of git that are required for git branching and merging and those are the following:
- Set up a git repository to work with.
- Create, checkout and delete branches.
  - HEAD pointer
- Merging.
  - Handle conflicts
  - Fast forward merge
  - 3 ways merge
- Stashing changes.

### Setup git repository
First will be set up out git repository (it will be a test project or you can also create a real project for this) to do it follow these steps.

1. Create a directory with this command and move into it. 
```zsh
$ mkdir test-project
$ cd test-project
```
2. Here, we will create a git repository and do that run following command
```zsh
$ git init
Initialized empty Git repository in ${PATH}/test-project/.git/
```
3. If you do `git status` it shows that we are on the master branch. We didn't create it, Git by default create a master branch for us.
```zsh
$ git status
On branch master
nothing to commit, working tree clean
```
4. Now, let's add a sample text file into it.
```zsh
$ echo Hello to the world from test1 file >> test1.txt
```
5. Let's get this sample file stage.
```zsh
$ git add test1.txt
$ git commit -m "add test1 file"
[master (root-commit) eabe754] add test1 file
 1 file changed, 1 insertion(+)
 create mode 100644 test1.txt
```
  Now, if you do `git log` you will see this commit in history.
6. Now, let's add one more file to our working directory to make it our commit history a little longer.
```zsh
$ echo Hello to the world from test2.txt file >> test2.txt
$ git status
  On branch master
  Untracked files:
    (use "git add <file>..." to include in what will be committed)

    test2.txt
  nothing added to commit but untracked files present (use "git add" to track)
```

7. Let's stage it.
```zsh
$ git add test2.txt
$ git commit -m "add test2.txt file"
[master 4dfa023] add test2.txt file
 1 file changed, 1 insertion(+)
 create mode 100644 test2.txt
```
  If you do `git log` you will see two commits in the history.

### Create, checkout and delete branches.
Let's first understand why we need branches because it allows us to work on the different versions of the same file in parallel. The files on each branch can be independent and we can merge each branch version of a file to a single version that we will be learning next.

A branch is just a pointer to one of these commits that are in our history. Normally, the given branch you are in points to the commit you last made. Branches let us work on different purposes, for example, we could have a development branch for development, production branch for our released code, bug fixes branch for fixing bugs.

Now will learn how to create a branch, it is very simple all you need to do it `git branch branchName` `branchName` will the name of purpose you want this branch for.

```zsh
$ git branch development
```
Execute `git branch` to see the list of branches you have now.
```zsh
$ git branch
  development
* master
```
It is showing asterisk sign(*) in front of the master branch because you are inside the master branch right now, if you want to switch to the development branch, you can purse it by `git checkout development` command.

```zsh
$ git checkout development
Switched to branch 'development'
$ git branch
* development
  master
```
Now, you can see it is showing asterisk sign in front of the development branch.

##### HEAD
HEAD is a pointer, which tells git in which branch user is currently in. It normally points to a branch and git terminology HEAD tells us what we have checked out.

If you execute `git log` command, it will give you below's output where the `HEAD` is pointing the `development`, there you can see an arrow pointing to the development. So currently we are in the development branch.
```zsh
$ git log
commit 4dfa0231d889288eb94fc55e22d275f36ce0377b (HEAD -> development, master)
Author: mountainfirefly <pankajdcoder@gmail.com>
Date:   Sat Dec 28 16:10:41 2019 +0530

    add test2.txt file

commit eabe754abd6aac4436ac2da589abf1089f343a89
Author: mountainfirefly <pankajdcoder@gmail.com>
Date:   Sat Dec 28 15:59:03 2019 +0530

    add test1 file
```

##### Work on branches
Let's create one more branch so that we can merge on the different branches separately.

```zsh
$ git checkout -b staging
Switched to a new branch 'staging'
```
This is a shortcut for creating and switching to the branch at the same time.

Now if you execute `git log` in your terminal the `HEAD` will be pointing to the `staging`, `development` and `master` branch because all the branches have similar changes inside them.

Let's stage some changes to this `staging` branch. Open `test1.txt` with your editor and add some more content inside it.

```txt
Hello to the world from test1 file
Some more text for testing.
```

```zsh
$ git add test1.txt
$ git commit -m "update test1.txt"
[staging 1bdb01f] update test1.txt
 1 file changed, 1 insertion(+)
```
```zsh
$ git log --all --decorate --oneline --graph
* 1bdb01f (HEAD -> staging) update test1.txt
* 4dfa023 (master, development) add test2.txt file
* eabe754 add test1 file
```
I have used couple of options for decorating the git log output. Now, you can see `HEAD` is only pointing to the `staging` branch because we have some difference between branches.

Now, check out to the `development` branch and do some changes there as well.

```zsh
$ git checkout development
Switched to branch 'development'
```

Let's stage some changes to the `development` branch. Open `test1.txt` with your editor and add some more content inside it.

`test1.txt` file:- here you can see we don't have changes that we have done under the `staging` branch because that only belongs to the `staging` branch and we have merged those changes into this that will learn soon.
```txt
Hello to the world from test1 file and development branch
```
Here, we have done some different changes as compared to the `staging` branch.

```zsh
$ git add test1.txt
$ git commit -m "Update test1.txt"
[development 24a4562] Update test1.txt
 1 file changed, 1 insertion(+), 1 deletion(-)
```

If you execute it `git log`, you can see `HEAD` no longer points to the `development` and `master` branch instead it only points to the `development` branch.

```zsh
$ git log --all --decorate --oneline --graph
* 24a4562 (HEAD -> development) Update test1.txt
| * 1bdb01f (staging) update test1.txt
|/
* 4dfa023 (master) add test2.txt file
* eabe754 add test1 file
```

Now we want both branches' changes ` staging` and `development` on to the master branch so we need to merge them.

### Merging
Git merge lets you take an independent line of code created by the git branch and integrate them into a single branch. There are two types of merge
- Fast-forward merge
- Three-way merge

##### Fast-Forward Merge
If there is a direct path between the base branch and to be merged branch git can perform what we call fast-forward merge. In this case, git only needs to change `master` pointer on to the `staging` branch.

First, we need to move on to the master branch.
```zsh
$ git checkout master
Switched to branch 'master'
```

Let's merge.

```zsh
$ git merge staging
Updating 4dfa023..1bdb01f
Fast-forward
 test1.txt | 1 +
 1 file changed, 1 insertion(+)
```
Here, in the output, you can see git has performed the Fast-Forward merge.

### Three-way Merge
A three-way merge uses a dedicated commit to tie together the two histories. It merges the two branches and creates a new commit that is called merge commit.

Now, let's merge `development` branch into our `master` branch.

Execute `git status` to check in which branch you are in.

```zsh
$ git status
On branch master
nothing to commit, working tree clean
```
We are in the master branch, if you are not in the master branch, do a checkout to it.

Let's merge `development` into `master`.
```zsh
$ git merge development
Auto-merging test1.txt
CONFLICT (content): Merge conflict in test1.txt
Automatic merge failed; fix conflicts and then commit the result.
```
The automatic git merge is failed because we have done changes in the same file and the same line inside both branches.

Now, will learn how to resolve conflicts.

### Resolve Conflicts
If two branches you're trying to merge both changed the same part of the file, git won't be able to know which version to use. When such a situation occurs, it stops right before the merge conflicts so you can merge it manually.

Now if you open `test1.txt` file inside your editor, it will look like this.
```txt
<<<<<<< HEAD
Hello to the world from test1 file
Some more text for testing
=======
Hello to the world from test1 file and development branch
>>>>>>> development
```

It is up to us which change we want to keep or we can merge both changes. Like this
```txt
Hello to the world from test1 file and development branch
Some more text for testing
```
Now, our conflicts are resolved, we have kept changes of the `development` branch and some from the `master` branch.

NOTE: You have to remove `>>>>>>>` and `=======` lines from your file.

If you do `git status` now it will show you, you can abort the merge or stage the file and commit it.
```zsh
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:   test1.txt
no changes added to commit (use "git add" and/or "git commit -a")
```

Let's stage and commit the file.
```zsh
$ git commit -a -m "fix conflicts"
[master b438809] fix conflicts
```
You can use `-a` option to stage and commit tracked files.

```zsh
$ git log --all --decorate --oneline --graph
*   b438809 (HEAD -> master) fix conflicts
|\
| * 24a4562 (development) Update test1.txt
* | 1bdb01f (staging) update test1.txt
|/
* 4dfa023 add test2.txt file
* eabe754 add test1 file
```
Here, you can see our `master` is updated now.

### Delete branches
Deleting a branch is simple. All you need to do it to execute `git branch -d branchName` for deleting a branch.

Before you delete a branch, you make sure if it merged with the master branch.
```zsh
$ git branch --merged
  development
* master
  staging
```
It tells us that `development` and `staging` branches are merged with `master` branch and now we can safely delete them.

```zsh
$ git branch -d development
Deleted branch development (was 24a4562).
$ git branch -d staging
Deleted branch staging (was 1bdb01f).
```

**NOTE**: If the branch you want to delete is not merged with the base branch then you need to use `-D` option instead of `-d` to delete it.

### Git Stash
Until now, when we checkout a branch or merged branch our working tree was already clean. Sometimes if our working tree is not clean we won't be able to perform such operations before clearing it up.

The excellent way to quickly get around this is to use git stash. It let you store your change temporary that you can retrieve them back later.

Let's see how it works. First will create a branch.

```zsh
$ git checkout -b stage
Switched to a new branch 'stage'
```
Now, will change our `test1.txt` file content.
```txt
Hello to the world from test1 file and stage branch
Some more text for testing
```
And stage and commit it.

```zsh
$ git commit -a -m "update test1.txt"
[stage f6f1ff6] update test1.txt
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git checkout master
Switched to branch 'master'
```

Now, we are on the master branch and here will again change the `test1.txt` file.

```txt
Hello to the world from test1 file and master branch
Some more text for testing
```

If you try to checkout to the `stage` branch, you won't be able to do it.
```zsh
$ git checkout stage
error: Your local changes to the following files would be overwritten by checkout:
    test1.txt
Please commit your changes or stash them before you switch branches.
Aborting
```
Now, we will do `git stash` which stores our changes so that we can apply them later.

```zsh
$ git stash
Saved working directory and index state WIP on master: b438809 fix conflicts
$ git status
On branch master
nothing to commit, working tree clean
```
`git status` shows us that we are back on your clean working tree. Now we can freely check out our branches and perform operations like merge.

Let's again do some changes and stash them. We will change our test1.txt file again.
```txt
Hello to the world from test1 file and again stashing change on the master branch
Some more text for testing
```

Let's stash them again.

```zsh
$ git stash
Saved working directory and index state WIP on master: b438809 fix conflicts
```

We can see list of stashes with `git stash list` command
```zsh
$ git stash list
stash@{0}: WIP on master: b438809 fix conflicts
stash@{1}: WIP on master: b438809 fix conflicts
```
Here, you can see we have two stashes.

We can apply these stashes by anytime. Let's see how to apply them.

```zsh
$ git stash apply
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   test1.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

This by default picks the most recent stash. Also, the `git stash apply` doesn't remove the stash from the list.

Now will discard the changes and apply the specific stash again.

```zsh
$ git checkout -- test1.txt
$ git stash list
stash@{0}: WIP on master: b438809 fix conflicts
stash@{1}: WIP on master: b438809 fix conflicts
```
We still have two stashes, as I told you `git stash apply` doesn't remove the stash from the list.

Here, will learn how to apply the specific stash with a label defined in front of it.

```zsh
$ git stash apply stash@{1}
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   test1.txt

no changes added to commit (use "git add" and/or "git commit -a")
```
Here, we will stage and commit this stash.

```zsh
$ git commit -a -m "commit stashed changes"
[master 7039051] commit stashed changes
 1 file changed, 1 insertion(+), 1 deletion(-)
```

So, if you want to apply and remove the stash from the stash list, you can do it by `git stash pop`.

This was the last topic of this article, I hope you must have enjoyed it.

I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev)

