---
layout: post
title:  "How Git works"
categories: tools
---

I prepared some questions and answers about git to help you to visualize
how Git works.

## Where git saves all the information?

Git metadata is saved under .git folder. If you delete this folder you will
loose the data including all commit history.

## What is Stage?

There are two main spaces in git world: `Untracted world` and `Tracted world`.

The stage is a kind of suttle delivering untracted changes from the untracted world
to the tracted world. In order to record your untracted changes to git, the first
thing you need to do is adding these changes to the stage so you can commit them.

```
Untracted world                    Tracted world
______________        Stage       ________________
|            |       _______      |              |
|            |      |_o___o__\    |              |
|____________|                    |______________|
```

## What changes you can add to the stage?

- A new file
- A change to a file (git knows)
- A deleted file (git knows)

> A new empty folder can't be added to the stage becuase git only cares files

## What each commit contains?

* `ID`: 069821239330ac1daf83f5051d47a4977e09064c
* `Message`: [Description of what has changed]
* `Parent ID`: fe38c140e4280e89d7c2ea5469ed2879f32215d1
* `Changes`: [Create a file a.txt, Replace second line to "new line", ...]

You can think a commit as a node in a `linked list`. Commits are connected by
having a parent commit id.

```
___________________     __________________     __________________
|   First Commit  |     |  Second Commit |     |  Third Commit  |
| ID:        1    | ___ | ID:         2  | ___ | ID:         3  |
| Parent ID: null |     | Parent ID:  1  |     | Parent ID:  2  |
|_________________|     |________________|     |________________|

```

## Can you edit commits?

Commits are `immutable` so you can't change any properties of a commit once
it is created including the id and message.
You can use a rebase feature to edit commits but this actually recreates the
all related commits.
For example when you have 10 commits and you edited a commit message of the
second commit using rebase, git recreates the second commit and 8 followed commits
because their parent ids are changed.

## How git handles untracked files?

Untracted files are always ignored in any git operations.

For example, when you checkout to another branch, all git tracted files are
changed to match that branch but untracted files will be still there.

## What checkout does?

You can go back to a particular point in git history or switch to a different
branch using checkout. The checkout process changes the working directory
to match the state of the checkout point. You won't see the changes added after that point
and added in other branch.

> Checkout ignores untracted files so they will servive from the checkout.

> Checkout complains if you made changes not committed. The good practice is
to make a clean state before run checkout.

## Can you read Branch Graph?

You will see branch graphs when you play with any visual git applications.
A branch graph has circles represent commits and they are linked from the bottom to top.
From the graph below, when you checkout the commit with id 4, then the working directory
is applied the changes from commits with ids from 1 to 4 resulting having two files a.txt and b.txt.

```
  (5) Delete a.txt          => [b.txt("b")]
   |
  (4) Append "b" to b.txt   => [a.txt("a"), b.txt("b")]
   |
  (3) Create b.txt
   |
  (2) Append "a" to a.txt
   |
  (1) Create a.txt
```

## What happens to the branch when you create a commit

You can think a branch as a pointer to a particular commit. When you create
a branch, git creates the branch pointing to the latest commit in the current
branch. When you create a commit in the new branch, the new branch points to the
new commit.

```
Checkout (1) and create branch1      Create a commit (3) in branch1
_______________________________      ______________________________

master -> (2)                        master -> (2) (3) <- branch1
           |                                    | /
          (1) <- branch1                       (1)
```

> `master` is the main branch

## What are Remotes

A remote is a reference to another git repository. Using a remote
you can push your changes to the remote repository or pull changes
from the remote repository. When you clone a repository, git creates a remote
`origin` which references to the original respotiroy.

## Where branches from remotes are saved?

Git saves branches from remotes as the same way as local branches.
The `master` branch in the `origin` remote is saved as a branch with the name
`origin/master`. However, remote branches are normally hidden from you
unless you order to show them.

```
How git saves the master branch from the origin remote
______________________________________________________

master -> (2)
           |
          (1) <- origin/master
```

## How to get the latest changes from the remote

You can update remote branches using `fetch` command.
Fetch process looks through all remote branches and

* Create branches not yet exist
* Download commits yet have

Before fetch

```
Working repository                   Remote repository
_______________________________      ______________________________

*master -> (2) <- origin/master       master -> (3)
            |                                    |
           (1)                                  (2) (4) <- branch1
                                                 | /
                                                (1)
```

After fetch

```
Working repository                   Remote repository
_______________________________      ______________________________

           (3) <- origin/master       master -> (3)
            |                                    |
*master -> (2) (4) <- origin/branch1            (2) (4) <- branch1
            | /                                  | /
           (1)                                  (1)
```


## What's different between local and remote branches

The local branch `master` and remote branch `origin/master` are different branches.
However, git provides a clever way to interact between branches having the same name.
Here is a list of magicks.

1. Checkout magick

When you checkout to a branch yet exists, git looks for remote branches already
has with the same name. If git finds one, git creates a new branch with the given
name pointing to the same commit the remote branch is pointing to.

```
git checkout branch1

Before                               After
_______________________________      ______________________________

              (3)<- origin/branch1     *branch1 -> (3) <- origin/branch1
              /                                    /
*master -> (2) <- origin/master       master -> (2) <- origin/master
            |                                    |
           (1)                                  (1)
```

2. Push magick

When you push your branch to the remote for the first time, git creates and updates
the branch to the remote branch with the same name. Also git creates the branch
`origin/branch1` which points to the same commit the `branch1` branch is pointing to.

```
git push origin branch1

Before                               After
_______________________________      ______________________________

  branch1 -> (3)                       branch1 -> (3) <- origin/branch1
             /                                    /
master -> (2) <- origin/master       master -> (2) <- origin/master
           |                                    |
          (1)                                  (1)
```

## What happends when you merge changes from another branch

You can update new changes from another branch using the command `merge`.

The merge command looks new commits in the target branch (merge from) which are
not in the current branch (merge to) and apply them to the current branch.

The simplest way git handles the merge is to move the current branch to point
where the target branch is pointing to. This solution is called `fast-forward`.

```
git merge branch1

Before                               After
_______________________________      ______________________________


              (2) <- branch1         *master -> (2) <- branch1
              /                                  |
*master -> (1)                                  (1)
```

However, this solution does not work if the current branch (merge to) has changes
not in the target branch (merge from) like below.

```
git merge branch1

Before                               Not working (Loose commit(3))
_______________________________      ______________________________

*master -> (3) (2) <- branch1                   (3) (2) <- branch1 <- *master
            | /                                  | /
           (1)                                  (1)
```

Becuase each commit has the parent commit id can't be changed, there is no way
to merge commit(3) and commit(2) without re-creating one of them. In this case,
git creates one more commit having an extra parent id which is called `merge commit`.
If you see the commit history of the `master` branch after the merge, you will see
new commits in the `branch1` branch followed by the merge commit.

Same as other commits, merge commits also can include changes and we normally
use it for solving conflicts.

```
git merge branch1

Before                               After
_______________________________      ______________________________

                                     *master -> (4)
                                                 | \
                                                 |  (2) <- branch1
                                                 |   |
*master -> (3) (2) <- branch1                   (3)  |
            | /                                  | /
           (1)                                  (1)
```

```
The order of commits in the master branch after the merge

(1) -> (3) -> (2) -> (4)
```


to be continue ...
