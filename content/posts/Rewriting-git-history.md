---
title: "Altering Git history"
date: "2020-01-19T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/altering-git-history/"
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
  - "Alter"
  - "Rewrite"
description: "This post will be about learning how to alter and rewrite the commit history using interactive rebase tool and git commit --amend."
---
![GitHUB](/media/post18-image1.png)
This is another post on git, which will be covering how to alter and rewrite git history.

Requisites for this post is my last three posts on the git
- [Git core concepts](https://mountainfirefly.dev/posts/learn-about-git/)
- [Git branching and merging](https://mountainfirefly.dev/posts/git-branching-and-merging/)
- [Git remotes](https://mountainfirefly.dev/posts/learn-about-git-remote/)

### Topics consists in this post
- Reword last commit message
- Interactive Rebase
  - Reword other commit messages
  - Deleting commits
  - Reordering commits
  - Squash commits together

Many times, when you are working with git, you may need to revise the git commit history which can include anything from changing the commit message to changing the order and content of the commit to squashing commits together.

To start with the above topics, first we need to create local git repository to do that follow the below steps:
1. Create a local directory.
```zsh
$ mkdir rewriting-git-history
$ cd rewriting-git-history
```
2. Use `git init` to initilize the empty git repository.
```zsh
$ git init
Initialized empty Git repository in /Users/mountainfirefly/tmp/rewriting-git-history/.git/
```
3. Add some sample JS files to work with git.
```zsh
$ echo "console.log('file 1 content')" >> file1.js
$ echo "console.log('file 2 content')" >> file2.js
```
4. Let's stage and commit them.
```zsh
$ git commit -m "first commit"
[master (root-commit) 5e63564] first commit
 2 files changed, 2 insertions(+)
 create mode 100644 file1.js
 create mode 100644 file2.js
```
5. To see git logs, use `git log` command.
  ```zsh
  $ git log --all --decorate --oneline --graph
    * 5e63564 (HEAD -> master) first commit
  ```

Now, we'll move to our first topic.

### Reword last commit message and its content
You can change the most recent commit in the commit history by using `git commit --amend` command. The `git commit --amend` command provides you a convenient way to modifying the most recent commit. It lets you combine the staged changes with the previous commit instead of creating the entirely new commit.

It can also be used to edit the previous commit message without affecting the snapshot.

Let's change our most recent commit message that is ***first commit*** to ***add js files***. To achieve this, run the below commands in your terminal inside your repository.

```zsh
$ git commit --amend -m "add js files"
[master b7cf4bb] add js files
 Date: Sun Jan 19 14:19:29 2020 +0530
 2 files changed, 2 insertions(+)
 create mode 100644 file1.js
 create mode 100644 file2.js
```
Run `git log` to see the changed commit message.

```zsh
$ git log --all --decorate --oneline --graph
* b7cf4bb (HEAD -> master) add js files
```

Now, let's add one more js file under the same commit message.

First, we need to stage the file that we want to add to the commit and then use `git commit --amend` to amend it in the commit history.

```zsh
$ echo "console.log('file 3 content')" >> file3.js
$ git add .
$ git commit --amend -m "add js files"
[master ca2833c] add js files
 Date: Sun Jan 19 14:19:29 2020 +0530
 3 files changed, 3 insertions(+)
 create mode 100644 file1.js
 create mode 100644 file2.js
 create mode 100644 file3.js
```

Here, if you run `git log` in the terminal you can see that we have successfully added the new content to the previous commit.

```zsh
$ git log --all --decorate --oneline --graph
* ca2833c (HEAD -> master) add js files
```

### Interactive Rebase
Rebasing the process of moving and combining a sequence of commits to a new base commit. Run `git rebase` with `-i` option to rewrite, replace, delete and merge individuals commits in the history. The primary reason for rebasing is to make the linear commit history.

#### Reword other commit messages
To modify the commit message that is much farther back in the commit history, we must move to the most complex tool which is an interactive rebase. With interactive rebase you can stop after each commit that you want to modify and change the message, add files or whatever you wish.

To change the commit which is much farther in the commit history, we need to add a couple of more commits to our `rewriting-git-history` project.

Let's add.
```zsh
$ echo "console.log('file 4 content')" >> file4.js
$ git add .
$ git commit -m "add file4"
[master c093603] add file4
 1 file changed, 1 insertion(+)
 create mode 100644 file4.js
$ echo "console.log('file 5 content')" >> file5.js
$ git add .
$ git commit -m "add file5"
$ echo "console.log('file 6 content')" >> file6.js
$ git add .
$ git commit -m "add file6"
[master 10c729c] add file6
 1 file changed, 1 insertion(+)
 create mode 100644 file6.js
```

Let's see the history.
```zsh
$ git log --all --decorate --oneline --graph
* 10c729c (HEAD -> master) add file6
* 063f1ab add file5
* c093603 add file4
* ca2833c add js files
```

Let's change the ***add file4*** message to ***change add file4 commit message*** in the commit history. We will be using the `git rebase -i` command which will be taking an argument the parent of the last commit that you want to edit, that is `HEAD~2^` or `HEAD~3`.

```zsh
$ git rebase -i HEAD~3
```

The above command will display below output in the default terminal editor, for me it is displaying it in the **vim** editor.

Here it is.

```txt
pick c093603 add file4
pick 564b7b2 add file5
pick 853d328 add file6

# Rebase ca2833c..853d328 onto ca2833c (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

In the output, you can see that these commits are listed in the opposite order that you normally see with the `git log` command.

Also, there are listed many commands that you can use to perform the mentioned operation next to it.

You need to edit the script so that it stops where want to edit the commit. To do so, change the word `pick` to `edit` for each of the commits you want to stop the script.

In our case, we have planned to modify the `add file4` commit so we'll be changing the `add file4` `pick` to `edit` as below I have changed and save the file using `Ese` `:wq` command.

```txt
edit c093603 add file4
pick 564b7b2 add file5
pick 853d328 add file6

# Rebase ca2833c..853d328 onto ca2833c (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

Now, you will see this output in the terminal.
```
$ git rebase -i HEAD~3
Stopped at c093603...  add file4
You can amend the commit now, with

  git commit --amend

Once you are satisfied with your changes, run

  git rebase --continue
```
The script has stopped on the `add file4` commit, here you can stage the new file and use `git commit --amend` to amend it.

To continue the rebasing process run `git rebase --continue` which will close the rebasing process if there are not anymore stops.

For now, we'll just modify the commit message.

To do so, run below command or you can stage a file and run the below command.

```zsh
$ git commit --amend -m "change add file4 commit message"
[detached HEAD e31f338] change add file4 commit message
 Date: Sun Jan 19 15:09:01 2020 +0530
 1 file changed, 1 insertion(+)
 create mode 100644 file4.js
```

Run `git rebase --continue` to resume the rebasing process.

```zsh
$ git rebase --continue
Successfully rebased and updated refs/heads/master.
```

We didn't have any more stops so it has simply existed.

#### Delete a commit
You can also use the interactive rebase for deleting a commit as well. To delete a commit, you have to remove the commit line from the script file.

Run `git rebase -i HEAD~3`
```zsh
$ git rebase -i HEAD~3 
```

You will see this output in the editor
```txt
pick c093603 change add file4 commit message
pick 564b7b2 add file5
pick 853d328 add file6
```

To remove the `add file6` commit from the history, change it to this.

```txt
pick c093603 change add file4 commit message
pick 564b7b2 add file5
```

Save and exit the file.

Run `git log` to see the changes.
```zsh
$ git log --all --decorate --oneline --graph
* 289c24a (HEAD -> master) add file5
* e31f338 change add file4 commit message
* ca2833c add js files
```

You can see that you don't have that commit anymore in the commit history also the `file6` file is not there in the working directory too because it has been removed with the commit itself so be cautious whenever you doing these types of changes.


#### Reordering the commits
Reordering commits is very simple, all you need to do is open the interactive rebase and reorder the lines of commits in the file and save it.

Let's change the order of our project commits in history.

```zsh
$ git log --all --decorate --oneline --graph
* 289c24a (HEAD -> master) add file5
* e31f338 change add file4 commit message
* ca2833c add js files
```
Currently, our commit history looks like this, let's switch `add file5` and `change add file4 commit message` commits with each other.

Open interactive rebase:
```zsh
$ git rebase -i HEAD~2
```

Change this:
```txt
pick e31f338 change add file4 commit message
pick 289c24a add file5
```

To this:
```txt
pick 289c24a add file5
pick e31f338 change add file4 commit message
```

Save and exit the file.

Run `git log` to the changes.

```zsh
$ git log --all --decorate --oneline --graph
* b3305c1 (HEAD -> master) change add file4 commit message
* a964e23 add file5
* ca2833c add js files
```

You can see here the order of the commit has been changed.

#### Squashing commits
It is also possible to take the series of commits and squash them down into a single commit using interactive rebase tool.

For squashing commit we need to use `squash` instead of `edit` and `pick`. Let's squash our last two commits into one.

Trigger interactive rebase
```zsh
$ git rebase -i HEAD~2
```
Which will display the below output, change the `pick` to `squash` command for the second commit, that will get merged into the first commit.

```txt
pick a964e23 add file5
squash b3305c1 change add file4 commit message

# Rebase ca2833c..b3305c1 onto ca2833c (2 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

Save and exit of the above script will open the below script, which tells you the `change add file4 commit message` commit getting squashed into `add file5` commit.

```txt
# This is a combination of 2 commits.
# This is the 1st commit message:

add file5

# This is the commit message #2:

change add file4 commit message

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sun Jan 19 15:10:26 2020 +0530
#
# interactive rebase in progress; onto ca2833c
# Last commands done (2 commands done):
#    pick a964e23 add file5
#    squash b3305c1 change add file4 commit message
# No commands remaining.
# You are currently rebasing branch 'master' on 'ca2833c'.
#
# Changes to be committed:
#       new file:   file4.js
#       new file:   file5.js
#
```
Save and exit this script.

Run `git log` to see the changes.

```zsh
$ git log --all --decorate --graph
* commit 37d80516a5a4d954cbc5f1636110f82db07dbcd1 (HEAD -> master)
| Author: mountainfirefly <pankajdcoder@gmail.com>
| Date:   Sun Jan 19 15:10:26 2020 +0530
|
|     add file5
|
|     change add file4 commit message
|
* commit ca2833ca9fb97024e8508ea00e9007665279622c
  Author: mountainfirefly <pankajdcoder@gmail.com>
  Date:   Sun Jan 19 14:19:29 2020 +0530

      add js files
```

Here you can see that `add file5` and `change add file4 commit message` have squashed down to one commit.

That is all for this post, I hope you must have enjoyed it.

I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev)
