.. _using_tags:

==========
Using Tags
==========

.. highlight:: console

If you have used CVS or SVN in the past, you are probably familiar with tags already. A tag in SVN, however, is more like a branch (they are logically distinct but function the same way - you can make commits on a tag). Tags in git work like they do in CVS; they are just an alias for a commit id.

Each commit in the commit stream has a long identifier associated with it. Git will let you get away with only typing part of the identifier if it can uniquely identify the commit::

  $ git show d2280d
  commit d2280d000c84f1e595e4dec435ae6c1e6c245367
  Author: Matthew McCullough <matthew@github.com>
  Date:   Tue Jul 24 22:07:55 2012 -0700

      Adding maven build script

  diff --git a/.gitignore b/.gitignore
  index 6a03ed1..7c82083 100644
  --- a/.gitignore
  +++ b/.gitignore
  @@ -2,3 +2,4 @@
   *.log
   output/
   build/
  +target/
  ...

However, it can be useful to apply more meaningful names to certain commits, such as release numbers. To do this, you use the **tag** command::

  $ git tag v1.0 37dd

You can then use this tag anywhere that a commit id is normally expected. For example, you could compare the repository at the v1.0 commit against the special HEAD tag that always points to the last commit in the repository::

  $ git diff v1.0 HEAD | head
  diff --git a/.travis.yml b/.travis.yml
  new file mode 100644
  index 0000000..17eccd6
  --- /dev/null
  +++ b/.travis.yml
  @@ -0,0 +1,2 @@
  +sudo: false
  +language: java
  diff --git a/LICENSE b/LICENSE
  new file mode 100644
  ...
