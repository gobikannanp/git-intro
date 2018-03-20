.. _configuration:

=============
Configuration
=============

Git doesn't really require a lot of configuration to use (typically the minimum configuration is just a username and email address for when you make commits), but it does have some basic configuration options. Configuration options can be set for an individual repository, globally for all respositories owned by the current user, or system wide. To set a configuration for the current repository, you use the **config** command::

  $ git config core.ignoreCase true

Adding the **--global** switch sets a configuration globally for the current user::

  $ git config --global user.name "Jeffrey Poore"

Adding the **--system** switch sets a configuration globally for all users::

  $ git config --system core.bigFileThreshold 2g

There are too many configuration options to really cover here, but you can find all the configuration options here:

https://git-scm.com/docs/git-config#_variables
