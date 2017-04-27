---
layout: post
title:  "Git Cheat Sheet"
categories: cheat_sheet
---

## Install git

* Install on ubuntu

```
sudo apt-get install git-core
```

* Install on mac

```
brew install git
```

* Setup

```
git config --global user.name "ducksan cho"
git config --global user.email ducktyper@gmail.com
```

* Create ssh

```
mkdir ~/.ssh
chmod 700 ~/.ssh
ssh-keygen -t rsa
```

## Use a bare repository

A bare repository is a host repository where users can clone and push. Here
is a way to create a bare repository in a server and make the first commit
from a client computer.

* Server: create a bare repository

```
cd ~/git
mkdir <repository_name>.git
cd <repository_name>.git
git init --bare
```

* Client: create a repository, make the first commit and push it to the server

```
mkdir <any_name>.git
cd <any_name>.git
git init
touch readme.txt       # add a file to commit
git add -A             # add all changes to stage
git commit -m 'initial commit'
git remote add origin <username>@<domain>:git/<repository_name>.git
git remote -v          # check remote is set correctly
git push origin master # push changes to the server
```

## Commit process

```
git status                  # see changes from the last commit
git add -A                  # add all changes to stage
git add <filename>          # add a file to stage
git add *.rb                # add ruby files to stage
git reset <filename>        # remove a file from stage
git commit -m "commit files in stage with this message"
```

## Use branch for a new task

```
## create and move to a branch
git branch <task_name>
git checkout <task_name>
## check which branch you are in
git branch
## do some work
git touch new_feature.txt
git add new_feature.txt
git commit -m "add new_feature.txt"
## merge branch to master and delete
git checkout master
git merge <task_name>
git branch -d <task_name>
```

## Rollback

* Start over from the last commit

```
git reset --hard   # rollback uncommited changes except new files
git clean -f -d    # remove new files
```


* Rollback unstaged changed to a file

```
git checkout <filename>
```

* Change the last commit message

```
git commit --amend
```

* Cancel the last commit

```
git reset --soft HEAD~
```

* Rollback the last commit pushed to remote

```
git reset --hard HEAD~
git push origin -f
```

## Use tags

* Annotate tags (e.g. set version)

Annotate tags has extra information (who created it, when it was set, and more)

```
git tag -a v1.0.1 -m "message"         # tag to the current commit
git tag -a v1.2 9fceb02 -m "message"   # tag to the specific commit
git describe                           # show annotate tags
```

* Lightweight tags (e.g. save branch name before delete)

Lightweight tags work like branches that do not change.

```
git tag <tagname>
```

* Sync tags to remote

Push and pull ignore tags by default so you need to sync them manually.

```
git push --tag
git pull --tag
```

## Stash

Remove unstaged changes temporary (except new files)

```
git stash
git pull # do some work
git stash pop
```

## Ignore changes

Sometimes you want to ignore changes to a specific file. For example, a file
contains local IP address. It could be annoying to make sure these files are not
staged. You can use --skip-worktree to ignore these files. If the ignored files
were changed to remote, you will be notified on pulling.

```
git update-index --skip-worktree <filename>     # Do not track changes
git update-index --no-skip-worktree <filename>  # Track changes again
```

## Submodule

* Get submodules after clone the repository

```
git submodule init
git submodule update
```

* Update submodules

```
git submodule foreach git pull origin master
```

* Ignore changes in submodules
Submodules sometimes create temp files not included in ".gitignore". You can
Ignore these files by adding "ignore = dirty" to .gitmodules

```
## .gitmodules
[submodule "vim/bundle/vim-ruby"]
  path = vim/bundle/vim-ruby
  url = https://github.com/vim-ruby/vim-ruby.git
  ignore = dirty # Ignore changes under the path
```

## Statistic

* count lines

```
git ls-files | xargs cat | wc -l
```

## Others

* Skip ssl certificate check

```
git config --global http.sslVerify false
```
