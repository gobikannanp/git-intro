.. _comparing_working_repository:

=========================================
Comparing the Working Copy and Repository
=========================================

.. highlight:: console

Like in CVS and SVN, to see the changes in the working copy versus the current **HEAD** state of the repository, you use the **diff** command::

  $ vi Main.groovy
  $ git diff
  diff --git a/src/Main.groovy b/src/Main.groovy
  index 6191a10..29df7f5 100644
  --- a/src/Main.groovy
  +++ b/src/Main.groovy
  @@ -3,12 +3,13 @@ import static Division.divide
   import static Subtract.subtract
   import static Sum.sum

  -def name = "Matthew"
  -int programmingPoints = 10
  +def name = "Jeffrey"
  +int programmingPoints = 20

   println "Hello ${name}"
   println "${name} has at least ${programmingPoints} programming points."
   println "${programmingPoints} squared is ${square(programmingPoints)}"
   println "${programmingPoints} divided by 2 bonus points is ${divide(programmingPoints, 2)}"
   println "${programmingPoints} minus 7 bonus points is ${subtract(programmingPoints, 7)}"
  -println "${programmingPoints} plus 3 bonus points is ${sum(programmingPoints, 3)}"
  \ No newline at end of file

You can also compare the state of the repository against a given commit (compare working copy against repository at the time of the given commit) or compare two different commits (compare the state of the repository at those two commits)::

  $ git diff ebbbf77 45a30ea
  diff --git a/src/test/java/com/ambientideas/AppTest.java b/src/test/java/com/ambientideas/AppTest.java
  new file mode 100644
  index 0000000..c1ea083
  --- /dev/null
  +++ b/src/test/java/com/ambientideas/AppTest.java
  @@ -0,0 +1,43 @@
  +package com.ambientideas;
  +
  +import junit.framework.Test;
  +import junit.framework.TestCase;
  +import junit.framework.TestSuite;
  +
  +//Pending comments
  ...

Note that the diff output is relative to the order of the commit ids. If you reverse the commit ids, the diff output will similarly be reversed::

  $ git diff 45a30ea ebbbf77
  diff --git a/src/test/java/com/ambientideas/AppTest.java b/src/test/java/com/ambientideas/AppTest.java
  deleted file mode 100644
  index c1ea083..0000000
  --- a/src/test/java/com/ambientideas/AppTest.java
  +++ /dev/null
  @@ -1,43 +0,0 @@
  -package com.ambientideas;
  -
  -import junit.framework.Test;
  -import junit.framework.TestCase;
  -import junit.framework.TestSuite;
  -
  -//Pending comments
