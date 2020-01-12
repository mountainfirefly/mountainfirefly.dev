---
title: "Learn about Git Remote"
date: "2020-01-12T11:46:37.121Z"
template: "post"
draft: false
slug: "/posts/learn-about-git-remote/"
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
  - "remotes"
description: "In this post, we'll be working with git remote, learning to add remotes, create a repository on remote and how to push/pull changes on to it."
---
![GitHUB](/media/post18-image1.png)
This is the third post on git which will be covering how to work with git remotes.

There are some prerequisites for this post, which are my last two posts on [git core concepts](https://mountainfirefly.dev/posts/learn-about-git/) and [git branching and merging](https://mountainfirefly.dev/posts/git-branching-and-merging/). Please check them if you want.

### Topics that we'll be covering in this article.
- Git remotes
- Intro on Github
- How to create a repository
- `git push` command
- `git pull` command
- How to handle multiple remotes on the single repository? 

### What is git remote?
What is git remote?
A remote is a repository in another location, which will be used as a backup on someone else’s computer. To create a git remote, you can use one of the popular services like GitHub, Gitlab, and bitbucket.
For this post, I will be using the GitHub service for creating out remote repository.

### Github
Github is a global company that provides hosting for software version control using Git. Github brings together the world’s largest community of developers to discover, share and build better software.

### Create a repository in GitHub
* Go to the https://github.com/ and login into your account.
* If you don’t have GitHub account, go ahead and create one.
* Once you are logged in, https://github.com/new you can click this link which is going to take you the create repository page or you will find add repository button on the GitHub home page.
* Please fill in necessary details in the create repository form and hit create repository button to create it.
* If you have selected the `Initialize this repository with a README` checkbox it must have created a README.md file for you.

### Clone repository
I am assuming that you have git installed your system if you don’t have please checkout prerequisites.

To clone a repository from the GitHub on to your local system. For that first, you need to copy your git repository address.
To copy it you need to open your GitHub repository and click on the `clone or download` button and from `Clone with HTTPS` copy your GitHub repository address.

We will be using the `git clone` command for cloning our repository.

```zsh
$ git clone https://github.com/mountainfirefly/git-remote-example.git
Cloning into 'git-remote-example'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
```

Replace your own GitHub repository address with the above command.

### `git remote` command
Now you have your GitHub repository cloned on your local. You can go inside it by changing the directory.

```zsh
$ cd git-remote-example
```

Here execute the command `git remote`, it displays a list of remotes in your current repository.
```zsh
$ git remote
origin
```

After the clone, we have default remote origin which is pointing to the Github repository `https://github.com/mountainfirefly/git-remote-example.git`.

To view the full location of the remote you can add `-v` option with `git remote` command.

```zsh
$ git remote -v
origin    https://github.com/mountainfirefly/git-remote-example.git (fetch)
origin    https://github.com/mountainfirefly/git-remote-example.git (push)
```

### `git push` command
`git push` command is used to push the local repository content to the remote repository. Pushing is how your local git repository commits get transfer to the remote repository.

To see how does the git push works, first, we need to do some changes to how local repository.

Create `example.txt` file under `git-remote-example` directory and put whatever text you want to put inside.

Below command will create an example.txt with `Hello World` inside it.
```zsh
$ echo Hello World >> example.txt
```

Now if you execute `git status`, you will see a new file as the untracked files.
```
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)

    example.txt

nothing added to commit but untracked files present (use "git add" to track)
``

Add it by with `git add` command to the staging area.

```zsh
$ git add example.txt
```

If you do `git log` it shows us commit history. If there is `README.md` file there in your repo, you can see you already have one commit there, which has been created by git to add the `README.md` file to your repository.

```zsh
$ git log --all --decorate --oneline --graph
* a5b133a (HEAD -> master, origin/master, origin/HEAD) Initial commit
```
In the above output `HEAD` is pointing to `master`, `origin/master` and `origin/HEAD`, here the `origin/master` and `origin/HEAD` refering to the `remote` master branch and `HEAD`.

Let's commit our example.txt file to the commit history.

```zsh
$ git commit -m "second commit"
[master ed1bbca] second commit
 1 file changed, 1 insertion(+)
 create mode 100644 example.txt
```

```zsh
$ git log --all --decorate --oneline --graph
* ed1bbca (HEAD -> master) second commit
* a5b133a (origin/master, origin/HEAD) Initial commit
```

In the above output, you can see that our local master is ahead of the remote master branch. To sync the `origin/master` with our local `master` we need to push our changes to the remote repository.

To push changes to the remote branch we use `git push` command along with remote alias and remote branch.
```zsh
$ git push origin master
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 294 bytes | 294.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/mountainfirefly/git-remote-example.git
   a5b133a..ed1bbca  master -> master
```

Now if execute `git log` in your terminal, you could see that HEAD is now also pointing to `origin/master` and `origin/HEAD`.

```zsh
$ git log --all --decorate --oneline --graph
* ed1bbca (HEAD -> master, origin/master, origin/HEAD) second commit
* a5b133a Initial commit
```

### `git pull` command
Git pull command is used to update the local version of the repository with the remote version. Merging remote upstream changes to your local repository is a common task in the Git-based collaboration workflow.

The `git pull` command is a combination of two commands, `git fetch` followed by `git merge`.

Right now, we don't have any changes that exist on the remote repository and don't exist on the local repository.

But, we'll gonna execute it anyway.

```zsh
$ git pull origin master
From https://github.com/mountainfirefly/git-remote-example
 * branch            master     -> FETCH_HEAD
Already up to date.
```
`git pull` runs along with two params remote name and branch name.

The output is showing that our branch is updated to date with the remote branch.

### How to handle multiple remotes on the single repository?
For understanding this concept we have two options: one is to fork a repository and the second one is to create a separate repository. We'll be moving ahead with forking a repository, which is going to create a separate copy of that repository in our GitHub account.
To do it follow these steps:
- I found this https://github.com/rmccue/test-repository repository on GitHub, which we'll fork. To fork, open https://github.com/rmccue/test-repository link in the browser and make sure you have logged in with your account and click on the fork button the top right of the page.
- Which is going to open a popup where you want to copy it, select your account.
- After that copy the repository location from `clone and download` and use `git clone` command to clone it to the local system.

```zsh
$ git clone https://github.com/mountainfirefly/test-repository.git
Cloning into 'test-repository'...
remote: Enumerating objects: 9, done.
remote: Total 9 (delta 0), reused 0 (delta 0), pack-reused 9
Unpacking objects: 100% (9/9), done.
```
Go inside the repository and execute `git remote -v`.

```zsh
$ cd test-repository
$ git remote -v
origin    https://github.com/mountainfirefly/test-repository.git (fetch)
origin    https://github.com/mountainfirefly/test-repository.git (push)
```

Now, what we going to do it is to merge both repository content. This is going to be fun and also the first time I am doing this kind of stuff.

Here we are going to add our remote repository location inside this repository.

To add a remote `git` have `git remote add <remote_name> <repo_url>` command. Here I will put my own repository address, please put it your's repository address in place of it.

```zsh
$ git remote add upstream https://github.com/mountainfirefly/git-remote-example.git
```
To view the list of remotes
```zsh
$ git remote -v
origin    https://github.com/mountainfirefly/test-repository.git (fetch)
origin    https://github.com/mountainfirefly/test-repository.git (push)
upstream    https://github.com/mountainfirefly/git-remote-example.git (fetch)
upstream    https://github.com/mountainfirefly/git-remote-example.git (push)
```

Now we'll pull upstream remote changes to the origin remote repository.

```zsh
$ git pull upstream master --allow-unrelated-histories
From https://github.com/mountainfirefly/git-remote-example
 * branch            master     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
 README.md   | 1 +
 example.txt | 1 +
 2 files changed, 2 insertions(+)
 create mode 100644 README.md
 create mode 100644 example.txt
```
The `--allow-unrelated-histories` option we have used to merge unrelated history if there are any.

Execute `ls` command
```zsh
$ ls
README      README.md   example.txt opml.php
```
Here we have our repository changes in this repository, to push it to the remote repo we'll be using `git push`.

Now we'll update both remote repository with each other changes.

Update our own created repository, which is on `origin` remote name.
```zsh
$ git push origin master
Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 4 threads
Compressing objects: 100% (5/5), done.
Writing objects: 100% (8/8), 1.22 KiB | 415.00 KiB/s, done.
Total 8 (delta 0), reused 0 (delta 0)
To https://github.com/mountainfirefly/test-repository.git
   e53f405..2abf1d1  master -> master
```
Update the forked repository, which is on the `upstream` remote name.
```zsh
$ git push upstream master
Enumerating objects: 12, done.
Counting objects: 100% (12/12), done.
Delta compression using up to 4 threads
Compressing objects: 100% (8/8), done.
Writing objects: 100% (11/11), 1.50 KiB | 382.00 KiB/s, done.
Total 11 (delta 0), reused 0 (delta 0)
To https://github.com/mountainfirefly/git-remote-example.git
   ed1bbca..2abf1d1  master -> master
```

Open both repositories on your browser and you can see both the repositories has each other changes.

That is all for this post, I hope you must have enjoyed it.

I write articles like this every week, you can check those out here [mountainfirefly.dev](https://mountainfirefly.dev)
