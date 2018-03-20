.. _viewing_history:

==========================
Viewing Repository History
==========================

.. highlight:: console

Like CVS or SVN, you can see the history of changes to the repository by using the **log** command::

  $ git log
  commit ef7bebf8bdb1919d947afe46ab4b2fb4278039b3
  Author: Jordan McCullough <jordan@github.com>
  Date:   Fri Nov 7 11:27:19 2014 -0700

      Fix groupId after package refactor

  commit ebbbf773431ba07510251bb03f9525c7bab2b13a
  Author: Jordan McCullough <jordan@github.com>
  Date:   Wed Nov 5 13:00:35 2014 -0700

      Update package name, directory

  commit 45a30ea9afa413e226ca8614179c011d545ca883
  Author: Jordan McCullough <jordan@github.com>
  Date:   Wed Nov 5 12:59:55 2014 -0700

      Update package name, directory
  ...

Because of the long ids that git uses, the **log** command provides some alternative ways of specifying filter criteria for the log. The most commonly used options:

+ **--author=<pattern>** / **--committer=<pattern>**
+ **--until=<date>** / **--before=<date>**
+ **--since=<date>** / **--after=<date>**
+ **-<number>** / **-n <number>** / **--max-count=<number>**
+ **-grep=<pattern>**
