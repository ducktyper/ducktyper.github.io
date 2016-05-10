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

There are two main spaces in git world: untracted world and tracted world.

Stage is a kind of suttle delivering untracted changes from untracted world
to tracted world. When you make commit, git add changes in stage into
tracted world. Git only knows things in tracted world.

```
Untracted world                    Tracted world
______________        Stage       ________________
|            |       _______      |              |
|            |      |_o___o__\    |              |
|____________|                    |______________|
```

## What changes you can add to  Stage?

- A new file
- A change to a file (git knows)
- A deleted file (git knows)

> A new empty folder can't be added to Stage becuase the changes are based on files.

## What each commit contains?

* ID: 069821239330ac1daf83f5051d47a4977e09064c
* Message: [Description of what has changed]
* Parent ID: fe38c140e4280e89d7c2ea5469ed2879f32215d1
* Changes: [Create a file a.txt, Replace second line to "new line", ...]

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
it is created including id and message.
You can use a rebase feature to edit commits but this actaully recreate the commits.
For example when you have 10 commits and you edited a commit message of the
second commit using rebase, git recreates the second commit and 8 followed commits
because their parent ids are changed.

## How git handles untracked files?

Untracted files are always ignored in any git operations.

## What checkout does?

You can go back to a particular point in git history or switch to a different
branch using checkout. The checkout process changes the working directory
to match the checkout point. You won't see the changes added after that point
and in other branch.

> Checkout ignores untracted files so they will servive from the checkout.

> Checkout complains if you made changes not committed. The good practice is
to make a clean state before run checkout.

## Understand Graph

You will see git graph when you play with any visual git applications.
Git graph has circles represent commits and they are linked from the bottom to top.
From the graph below, when you checkout the commit with id 4, then the working directory
has the changes from commits 1 to 4 resulting having two files a.txt and b.txt.

```
  (5) Delete a.txt          => [b.txt("b")]
   |
  (4) Append "b" to b.txt   => [a.txt("a"), b.txt("b")]
   |
  (3) Add b.txt
   |
  (2) Append "a" to a.txt
   |
  (1) Add a.txt
```

## Understand Branch

You can think a branch as a pointer to a particular commit. When you create
a branch, git creates the branch pointing to the latest commit in the current
branch. When you create a commit in the new branch, git creates a commit
and points the new branch to the new commit.

```
Checkout (1) and create branch1      Create a commit (3) in branch1
_______________________________      ______________________________

master -> (2)                        master -> (2) (3) <- branch1
           |                                    | /
          (1) <- branch1                       (1)
```

> `master` is the main branch

