.. _moving_removing_files:

============================================
Moving or Removing Files From the Repository
============================================

.. highlight:: console

If you want to move or remove files from the repository, use the **mv** or **rm** command (note that this doesn't immediately move or delete the files from the repository but instead adds a move/delete change to the working set that must then be committed)::

  $ git mv README.txt README.md
  $ git rm fix.txt
  rm 'fix.txt'
  $ ls
  build.gradle  pom.xml  README.md  resources  runme.sh  src
  $ git status
  On branch master
  Your branch is ahead of 'origin/master' by 1 commit.
    (use "git push" to publish your local commits)
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

  	renamed:    README.txt -> README.md
  	deleted:    fix.txt

The mv or rm command will fail if your local copy is not up to date with the repository unless you pass a **-f** switch. Moving/removing directories requires the **-r** switch if the directory contains other files. If you aren't certain about the effects of the command, you can pass a **-n** switch to do a dry run::

  $ ls
  build.gradle  pom.xml  README.md  resources  runme.sh  src
  $ git rm -r src -n
  rm 'src/Division.groovy'
  rm 'src/Main.groovy'
  rm 'src/Square.groovy'
  rm 'src/Subtract.groovy'
  rm 'src/Sum.groovy'
  rm 'src/main/java/com/github/App.java'
  rm 'src/test/java/com/github/AppTest.java'
  $ ls src
  Division.groovy  Main.groovy    Subtract.groovy  test
  main             Square.groovy  Sum.groovy

If you would like to remove a file from the repository but keep it in your local filesystem, add the **--cached** switch::

  $ ls
  build.gradle  fix.txt  pom.xml  README.txt  resources  runme.sh  src
  $ git rm --cached README.txt
  rm 'README.txt'
  $ ls
  build.gradle  fix.txt  pom.xml  README.txt  resources  runme.sh  src
  $ git status
  On branch master
  Your branch is ahead of 'origin/master' by 1 commit.
    (use "git push" to publish your local commits)
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

  	deleted:    README.txt

  Untracked files:
    (use "git add <file>..." to include in what will be committed)

  	README.txt
