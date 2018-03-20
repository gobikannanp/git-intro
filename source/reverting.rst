.. _reverting:

=================
Reverting Changes
=================

.. highlight:: console

Every now and then, you are going to make mistakes and need to undo some changes. To do this, you use the **revert** command. You will be prompted for a commit message and the actual revert will be added as a new commit (the old commit will still remain)::

  $ git revert bc055

You can revert changes from any previous commit in the repository. Should a straight reversal of the change not be possible due to a conflict (other changes have occurred that overlap), you may have to manually perform a merge of the revert. If you request to revert more than one commit, git will process the reverts in order, and should a conflict arise, it will stop. You can resume the revert by adding the **--continue** switch::

  $ git revert --continue

If you are in the middle of applying several reverts and decide to abort, you can simply issue a **revert** command with the **--abort** switch if you want to discard all changes (remove any commits that were added) or with the **--quit** switch if you want to just stop and preserve any commits that already have been added.
