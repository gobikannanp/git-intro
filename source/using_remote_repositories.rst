.. _using_remote_repositories:

=========================
Using Remote Repositories
=========================

.. highlight:: console

Up until this point, all the commands that we have talked about could be done just on the local machine. Git, however, is meant to interact with remote repositories as well [1]_. Usually teams have a central remote repository that they copy to their local and then merge changes back to when they are done.

Cloning a Remote Repository
===========================

To start working with a remote repository, you can use the **clone** command. You can clone any repository, even one that is located on the same machine. You just specify the path or URL of the respository to clone::

  $ git clone /home/tellmejeff/work/newproject newprojectclone
  Cloning into 'newprojectclone'...
  done.
  $ git clone https://github.com/tellmejeff/mesos-marathon
  Cloning into 'mesos-marathon'...
  remote: Counting objects: 13, done.
  remote: Compressing objects: 100% (10/10), done.
  remote: Total 13 (delta 1), reused 10 (delta 1), pack-reused 0
  Unpacking objects: 100% (13/13), done.
  Checking connectivity... done.

Now that the repository is cloned, you can start using it like any regular local repository. You can add new files or update existing ones and then commit the changes. The changes won't be applied to the remote repository until you do a push (covered later).

Connecting an Existing Repository to a Remote Repository
========================================================

There are a few different scenarios in which you may want to link your local repository to a remote repository. Imagine first that you and a friend want to work on code together, and he has already created a branch on a remote server. You want to create a branch in your local that tracks changes on the remote (you can pull any changes there down to your local). First, you need to tell git about this remote server using the **remote** command (origin is the default name for a remote, but you can use any name you like)::

  $ git remote add origin https://github.com/tellmejeff/newproject.git

Now you need to execute a **fetch** command. This will update the remote tracking branches of the remote repository into your repository in a safe way that won't affect your local branches (if you don't specify a name after the **fetch**, the **origin** remote is used)::

  git fetch
  From https://github.com/tellmejeff/newproject
  * [new branch]      dev        -> origin/dev
  * [new branch]      master     -> origin/master

If you execute the **branch** command with the **-r** switch, you will now see these remote branches::

  $ git branch -r
  origin/dev
  origin/master

You now have a couple of options of how you make a local branch that is connected to the remote branch. You can checkout a new branch based on the remote branch by using the **checkout** command with the **--track** and **-b** switch::

  $ git checkout --track -b dev origin/dev
  Branch dev set up to track remote branch dev from origin.
  Switched to a new branch 'dev'

You can also reuse an existing branch by switching to that branch and then merging the remote branch::

  $ git checkout master
  $ git merge origin/master

You can also explictly set the upstream branch for a branch::

  $ git branch --set-upstream-to=origin/master master
  Branch master set up to track remote branch master from origin.

Updating From a Remote Repository
=================================

In CVS or SVN, when you want to get the latest changes from the repository, you just execute an **update** command. Git has similar commands, but because git is a distributed repository, it splits the update into two commands, **fetch** and **pull**. If you want to get the latest changes but you aren't sure whether it will disrupt something in your local repository, you can use **fetch** to pull it down to a separate branch. I did this in the previous section when we were connecting the local repository to a remote one. You can then use **merge** to bring those changes over from the remote branch to your local branch.

If you just want to bring all the changes down like a normal update would, you use the **pull** command. If you haven't made any changes since the last time you pulled from the remote repository, the commits pulled down are simply added on to the end of your commit stream in your local repository. If, however, you have made your own commits that aren't in the remote repository, the pull command will attempt to merge the changes in and then you will have to commit the merge if there is any overlap in the files from your commits and the updates pulled down [2]_.

Rebasing
========

Merge commits can be problematic [3]_, so if you do have changes that overlap, you may want do what is called a **rebase**. Effectively, you want to reconstruct the changes that you made in your local repository, but you want to apply them after the changes that were made in the remote repository. Git will unwind your commits (roll back to the point where you last pulled from the remote repository), apply all of the commits from the remote repository, and then start replaying your commits afterwards. You do this by adding the **--rebase** switch to the pull command::

  $ git pull --rebase

If there are any conflicts that git can't resolve when replaying your commits, it will stop so that you can manually fix the conflict. You then can resume the rebase using the **rebase** command with a **--continue** switch::

  $ git rebase --continue

You can also skip over a commit that has a conflict by using the **rebase** command with a **--skip** switch::

  $ git rebase --skip

If you want to abort the rebase and return the repository back to the original state, you use the **rebase** command with an **--abort** switch::

  $ git rebase --abort

.. [1] Technically the term remote is a misnomer. Any repository you synchronize with doesn't have to be at a different location, it could even be on the same machine. What is really meant by the term remote is a completely separate repository that has its own commit stream.

.. [2] Because your commits are already in your repository, adding the commits from the remote repository wouldn't make sense. Instead, they'll just have to be added in as a new commit. However, the log message that is saved with the merge will reference the commits from the remote repository so that there is still a reference to them should someone want to understand the history of how these changes came to be. Similarly, because your commits diverged from the remote repository, when you go to push, you'll end up pushing a merge commit to the remote. This is problematic, however, because you could be collapsing several commits into one, and if someone else pulls your changes down and has a conflict that git can't automatically resolve, they'll have to manually merge the changes in, and that can be quite disruptive. See the `Rebasing`_ section for details about how to avoid this issue.

.. [3] A merge commit is when you pulled some changes down from the remote repository but they weren't able to just be applied in a fast-forward merge, so you had to manually merge the changes and then commit them as a new commit. This leads to the problem mentioned in the previous footnote where pushing the merge and your additional changes as well can be problematic.
