.. _updating_a_repository:

=====================
Updating a Repository
=====================

.. highlight:: console

The Working Set
===============

Adding or updating files in git is a multi-step process. The first thing you need to understand is that there is a working set (often called the stage, index, or the files to be committed - I prefer the term working set, and this document will use that term) that determines what files will be added or updated. Unlike CVS or SVN, a commit doesn't automatically add all the changes that are working copy (it is possible, however, with git to do that). Most developers have at one point or another accidentally committed changes they didn't intend to commit, and git fixes this by requiring developers to explicitly add files to be included to the working set. For example, if I run a commit with no files in the working set, even though there are changes that could be committed, git will inform me that there are no files staged to be committed::

  $ git commit
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes not staged for commit:
  	modified:   README.txt

  no changes added to commit

You can see what files are in the working set by running the **status** command, which clearly deliniates which files are in the working set and which files are not::

  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

  	modified:   runme.sh

  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

  	modified:   README.txt

Adding/Removing Files to the Working Set
========================================

You can add one or more files to the working set with the **add** command::

  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

  	modified:   README.txt
  	modified:   fix.txt
  	modified:   pom.xml
  	modified:   runme.sh

  no changes added to commit (use "git add" and/or "git commit -a")
  $ git add README.txt fix.txt pom.xml runme.sh
  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

  	modified:   README.txt
  	modified:   fix.txt
  	modified:   pom.xml
  	modified:   runme.sh

To remove one or more files from the working set, you use the **reset** command. Issuing a reset command with no arguments will clear out the working set (it will not revert the changes)::

  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

  	modified:   README.txt
  	modified:   fix.txt
  	modified:   pom.xml
  	modified:   runme.sh

  $ git reset
  Unstaged changes after reset:
  M	README.txt
  M	fix.txt
  M	pom.xml
  M	runme.sh
  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

  	modified:   README.txt
  	modified:   fix.txt
  	modified:   pom.xml
  	modified:   runme.sh

  no changes added to commit (use "git add" and/or "git commit -a")

The **reset** command can also change where the special **HEAD** tag in the repository points to, based on some switches and an optional commit id::

  $ git reset [--soft|--mixed|--hard|--keep] [commit_id]

Depending on the following switches, you can reset changes in the commit stream, working set, or working copy:

+ **--soft**: if a commit id is specified, the **HEAD** tag will be moved to the specified id. The working set and working copy are unchanged. If no id is specified, this results in no action.
+ **--mixed**: this will clear the working set but not the working copy, so changes are essentially unstaged. If a commit id is also specified, the **HEAD** tag will be moved to the specified id. This is the default.
+ **--hard**: this will clear the working set and discard any changes in the working copy. If a commit id is specified, only the changes since that id are discarded, and the **HEAD** tag will be moved to the specified id.
+ **--merge**: this will clear the working set and then reset any files that had changes between the specified commit id and **HEAD** (you are resetting the repository back to the given commit id) but if any of those files also have changes that have not yet been committed to the repository, those changes will be preserved. If no commit id is specified, this behaves the same as **--mixed**.
+ **--keep**: this will clear the working set and reset any files that had changes between the specified commit id and **HEAD**. If any of those files have changes that have not been committed to the repository, the reset will abort. If no commit id is specified, this behaves the same as **--mixed**.

If you just want to undo all changes and return to the current **HEAD** commit, you can also use the **checkout** command with a **-f** switch::

  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

  	modified:   README.txt
  	modified:   fix.txt
  	modified:   pom.xml
  	modified:   runme.sh

  $ git checkout -f
  Your branch is up-to-date with 'origin/master'.
  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  nothing to commit, working directory clean

Ignoring Files
==============

Git only updates files in a repository when they are part of the working set, so files that are temporary (like build files) won't show up in commands like **status** unless they are in directories under the root that are already versioned. If you want git to ignore files, you can create a **.gitignore** file at any level of the repository (it applies to the containing directory and any subdirectories). The lines inside the file are patterns that should be matched against. Pattern syntax:

+ a blank line matches no files - it is just whitespace for readability
+ a line starting with **#** is a comment
+ a line starting with **!** is a negated pattern, so any file not matching the rest of the pattern is ignored
+ a line ending in a slash is considered a directory and will match that directory and any files under it (**foo/** will match the directory foo and all paths below it, but will not match a symbolic link **foo**)
+ a line not containing a slash is by default a glob pattern that matches files in the same directory as the .gitignore file
+ a line starting with a slash matches the beginning of the pathname (**/*.c** will match all files with a **.c** extension in the same directory as the .gitignore file, but will not match **foobar/menu.c**)
+ a single asterisk in a line will match any valid filename character except the file separator (**abc*.txt** will match files starting with **abc** and ending in **.txt** but doesn't include directories starting with **abc** containing files ending with **.txt**)
+ a double asterisk followed by a slash will match all directories (**\*\*/foo** will match file or directory **foo** in any directory at the same level as the .gitignore file)
+ a slash followed by a double asterisk will match everything inside that directory (**abc/\*\*** will match everything inside the directory **abc**)
+ a slash followed by a double asterisk followed by another slash matches zero or more directories (**a/\*\*/b** will match **a/b**, **a/x/b**, **a/x/y/b**, etc.)

Committing the Working Set
==========================

When you are ready to commit your changes to the local respository, you just run the **commit** command. The command will either start your favorite editor so that you can provide a commit message, or you can pass the message with the **-m** switch::

  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

  	modified:   README.txt
  	modified:   fix.txt
  	modified:   pom.xml
  	modified:   runme.sh

  $ git commit -m 'modified a few files for a demo'
  [master c040ec2] modified a few files for a demo
   4 files changed, 5 insertions(+), 3 deletions(-)

If you want to just commit all changes in the working copy without an explicit **add**, you can add the **-a** switch to your commit.
