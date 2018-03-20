.. _stashing_changes:

================
Stashing Changes
================

.. highlight:: console

Every now and then, you have some changes in flight, but you are not ready to commit those changes. Imagine that you need to switch to a different branch or tag. You could clone the source repository from the specific branch or tag, but then you might end up with lots of copies of the repository, and this can get messy. Instead of making new repositories, you can instead **stash** your changes which effectively makes the working copy in sync with **HEAD** in the local repository (note that this stashes the working directory and the working set that is staged for commit)::

  $ vi src/Main.groovy
  $ vi src/Square.groovy
  $ git add src/Main.groovy
  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

  	modified:   src/Main.groovy

  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

  	modified:   src/Square.groovy

  $ git stash
  Saved working directory and index state WIP on master: ef7bebf Fix groupId after package refactor
  HEAD is now at ef7bebf Fix groupId after package refactor
  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  nothing to commit, working directory clean

Notice that you can stash changes multiple times, and you can list the different stashes you have::

  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

  	modified:   src/Sum.groovy

  no changes added to commit (use "git add" and/or "git commit -a")
  $ git stash
  Saved working directory and index state WIP on master: ef7bebf Fix groupId after package refactor
  HEAD is now at ef7bebf Fix groupId after package refactor
  $ git stash list
  stash@{0}: WIP on master: ef7bebf Fix groupId after package refactor
  stash@{1}: WIP on master: ef7bebf Fix groupId after package refactor

To see the contents of a specific stash, you can use the **show** command (you can add a **-p** switch to see the diff output)::

  $ git stash show stash@{1}
   src/Square.groovy | 3 ++-
   1 file changed, 2 insertions(+), 1 deletion(-)
  $ git stash show -p stash@{1}
  diff --git a/src/Square.groovy b/src/Square.groovy
  index fde3319..2c0e09e 100644
  --- a/src/Square.groovy
  +++ b/src/Square.groovy
  @@ -1,3 +1,4 @@
  +// square a number
   static int square(int base) {
          base * base
  -}
  \ No newline at end of file
  +}

To restore the most recent stash, you can use the **pop** command::

  $ git stash pop
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

  	modified:   src/Sum.groovy

  no changes added to commit (use "git add" and/or "git commit -a")
  Dropped refs/stash@{0} (a752e03b89490bd696d5c7234ebfe4456b295ec4)

You can also pop a specific stash::

  $ git stash list
  stash@{0}: WIP on master: ef7bebf Fix groupId after package refactor
  stash@{1}: WIP on master: ef7bebf Fix groupId after package refactor
  stash@{2}: WIP on master: ef7bebf Fix groupId after package refactor
  $ git stash pop stash@{1}
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

  	modified:   src/Main.groovy

  no changes added to commit (use "git add" and/or "git commit -a")
  Dropped stash@{1} (218d1217408bfda1e7441a466d2bb84cbe97533a)

