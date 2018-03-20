.. _introduction:

============
Introduction
============

.. highlight:: console

This document will focus on how you can do version control operations using Git. It is assumed that you already understand the basic concepts of version control like adding/removing files to a repository, committing changes to those files, etc. This is not an exhaustive guide on all the different ways that git commands can be used, but instead focuses on the most common. Git has extensive help documentation (for any command, the short version can be seen with a **-h** switch and the man page version can be seen with a **--help** switch).

Git was created in 2005 by Linux Torvalds for the development of the Linux kernel. It was inspired by `BitKeeper <https://en.wikipedia.org/wiki/BitKeeper>`_ and `Monotone <https://en.wikipedia.org/wiki/Monotone_(software)>`_. It is a distributed version control system that has a number of characteristics that make it suitable for large numbers of developers (such as open source projects like the Linux Kernel):

+ **Strong support for non-linear development**: git supports rapid branching and merging, and includes tools for visualizing a non-linear development history.
+ **Distributed development**: git gives every developer a local copy of the development history and all changes are synchronized between repositories.
+ **Simple APIs**: git has a number of different ways of interacting with tools, but they are all based on simple standard protocols such as HTTP, FTP, or SSH.
+ **Scalable**: git has been developed to be fast even with large projects.

If you are familiar with version control repositories like SVN or CVS, you will find that git mostly works the same. There are some key differences, however:

+ Git is a distributed repository, and each developer effectively maintains their own repository with a separate commit stream. Changes made in other repositories can be merged into a developer's local repository to make the two repositories effectively identical, but each repository can have commits that are not synchronized automatically to other repositories.
+ Git is optimized for branching and merging, and so developers use branches often for single features. The main branch (called **master**) is intended to be always production ready, and developers usually work off a development or feature branch instead.
+ Commits to a repository in git are not necessarily sequential. Normally they will be applied in order when doing merges, but in some cases, it may be required to reorder commits [1]_. Because commits are not required to be sequential, they are given random unique ids instead of sequential ones. These unique ids are long, but you can reference a commit by a portion of the id as long as it uniquely identifies the commit.
+ There is only one commit stream across an entire repository. Unlike SVN or CVS where each branch has their own commit ids and history, git only has one history that is often visualized as lines that run in parallel with dots on those lines representing commits.

.. [1] A good example of this is when merging changes from a remote repository into a local repository when additional changes have occurred in the local repository. If some of the incoming changes conflict, it could require a manual merge. These manual merges can often result in many different changes getting bundled into one big commit which then causes other developers to also have to do merges when those changes are pushed to other repositories. Git offers the option of doing a **rebase**, where some commits are rolled back, the incoming commits applied, and then the rolled back commits are re-applied with possible manual merges along the way. By doing this rebase, new changes are applied at the end which reduces the likelyhood of other developers having to manually merge when these changes are pushed.
