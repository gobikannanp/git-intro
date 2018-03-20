.. _creating_a_repository:

=====================
Creating a Repository
=====================

.. highlight:: console

It is very easy to start a project with git. You can create an empty project or you can start with a folder that already has content in it. You can also **clone** a repository from somewhere else, but that is covered in the :ref:`using_remote_repositories` section. All you need to do to initialize a git repository is to run the **init** command::

  $ mkdir newproject
  $ cd newproject/
  $ git init
  Initialized empty Git repository in /home/tellmejeff/work/newproject/.git/

The working directory has been initialized, and a **.git** directory is created at the root of the project to store all the metadata about the repository.
