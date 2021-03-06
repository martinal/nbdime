===============
nbdime commands
===============

nbdime provides the following CLI commands::

    nbshow
    nbdiff
    nbdiff-web
    nbmerge
    nbmerge-web

Pass --help to each command to see help text for usage details.

Additional commands are available for :ref:`git-integration`.

nbshow
======

:command:`nbshow` gives you a nice, terminal-optimized summary view of a notebook.
You can use it to quickly peek at notebooks without launching the full notebook web application.

TODO: screenshot


Diffing
=======

nbdime offers two commands for viewing diffs. :command:`nbdiff` for command-line diffing,
and `nbdiff-web` for rich web-based diffing of notebooks.

.. seealso::

    For more details on how nbdime compares notebooks: :doc:`diffing`

nbdiff
------

:command:`nbdiff` does a terminal-optimized rendering of notebook diffs.
Pass it two notebooks you would like to compare,
and you should get a nice, readable presentation of the changes in the notebook:

TODO: of console diff


nbdiff-web
----------

Like :command:`nbdiff`, :command:`nbdiff-web` compares two notebooks.
But instead of a terminal rendering, it will open a browser and compare the two notebooks,
showing the rich rendered diff of images and other outputs.

TODO: screenshot of web diff


Merging
=======

One of the hardest things to deal with for notebooks is merging changes and dealing with conflicts.
Line-based tools can produce invalid notebooks that you have to fix by hand,
which is no fun at all, and risks data loss.

nbdime provides some tools for merging notebooks,
taking into account knowledge of the notebook file format
to ensure that a valid notebook is always produced.
Further, by understanding details of the notebook format,
nbdime can automatically resolve conflicts on generated fields.

.. seealso::

    For more details on how nbdime merges notebooks: :doc:`merging`


nbmerge
-------

:command:`nbmerge` merges two notebooks with a common parent.
If there are conflicts, they are stored in metadata of the destination file.
:command:`nbmerge` will exit with nonzero status if there are any unresolved conflicts.

:command:`nbmerge` writes the output to stdout by default,
so you can use pipes to send the result to a file,
or the `-o` argument to specify a file in which to save the merged notebook.

Because there are several categories of data in a notebook (such as input, output, and metadata),
nbmerge has several ways to deal with conflicts,
and can take different actions based on the type of data with the conflict.

.. note::

    Conflict-resolution is the area where there is the most active development
    in nbmerge, and is subject to change.

The ``-m, --merge-strategy`` option lets you select a global strategy to use.
The following options are currently implemented:

inline
    This is the default.
    Conflicts in input and output are recorded with conflict markers, while
    conflicts on metadata are stored in the appropriate metadata (actual
    values are kept as their base values).

TODO: Make a note about how it is stored (format, tag names, etc.)

    This gives you a valid notebook that you can open in your usual notebook editor
    and resolve conflicts, just like you might for a regular Python script.
use-base
    When a conflict is encountered, use the value from the base notebook.
use-local
    When a conflict is encountered, use the value from the local notebook.
use-remote
    When a conflict is encountered, use the value from the remote notebook.
union
    When a conflict is encountered, include both the local and the remote
    value, in that order (local then remote). Conflicts on non-sequence
    types (anything not list or string) are left unresolved.

.. note::

    The union strategy might resolve to nonsensical values, while still marking
    conflicts as resolved, so use this carefully.

The ``--input-strategy`` and ``--output-strategy`` options lets you specify a
strategy to use for conflicts on inputs and outputs, respecively. They accept
the same values as the ``--merge-strategy`` option. If these are set, they will
take precedence over ``--merge-strategy`` for inputs and/or outputs.

To use nbmerge, pass it the three notebooks:

- base: the base, common parent notebook
- local: your local changes to base
- remote: other changes to base that you want to merge with yours

::

    nbmerge base.ipynb local.ipynb remote.ipynb > merged.ipynb

TODO: screenshot of auto merge


nbmerge-web
-----------

:command:`nbmerge-web` is just like :command:`nbmerge` above,
but instead of automatically resolving or failing on conflicts,
you get a webapp for manually resolving conflicts::

    nbmerge-web base.ipynb local.ipynb remote.ipynb -o merged.ipynb

TODO: screenshot of merge tool

