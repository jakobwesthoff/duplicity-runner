================
duplicity-runner
================


About
=====

duplicity-runner is a bash shell script, which provides a convenience wrapper
about the backup utility called duplicity__. It combines the possibility of
providing base options to be used generally for every backup job mixed with
detailed options for an arbitrary number of different backup jobs.

To allow for easy configuration different configuration files with speaking
option names are used. Furthermore include and exclude lists can be provided in
separate files, which allow the easy definition of files to explicitly in- or
exclude from the backup.

Furthermore a the backup target may be automatically tested for the
availability of a certain service, before a backup is started. This is
extremely useful in case the backup is transported via Ethernet or wlan in
situations where the storage target may not always be reachable.

__ http://duplicity.nongnu.org/


Requirements
============

As this duplicity-runner is a bash script the bash shell is needed to run it.
It may work with other shell implementations as well, but hasn't been tested
with them. If you can provide information about the script running or not
running on different shells I would appreciate if you could drop me a line. If
you patched the script to work with your shell of choice, please let me know as
well. I would be happy to integrate your changes in such a case.

Further requirements are as follows:

- Bash 4.x
- GNU tools aka. grep, sed, …
- Netcat (nc binary) with support for the -z option
- Duplicity 0.6.09 or higher


Installation
============

To use this little utility simply copy the ``duplicity-runner`` file from the
``/bin`` folder of this repository to a directory inside your execution path
(eg. ``/bin``). Make sure its permissions are set for execution (``chmod ug+x
duplicity-runner``).

By default duplicity-runner searches for its configuration files below
``/etc/duplicity-runner``. To be precise it tries to locate a file called
``runner.cfg`` there. Backup jobs and their configuration files are searched
for in the a directory called jobs below that. Each job configuration consists
of a ``job.cfg`` file and two optional lists of in- and exclusions called
``include.cfg`` and ``exclude.cfg``. Jobs are stored in subdirectories of
the ``jobs`` folder. The name of their folder is used for naming them.

Make sure the directory ``/var/run/duplicity-runner`` does exist and is
writable by the user executing the script, as it does store lock files for the
different backup jobs in here.

Furthermore the file ``/var/log/duplicity.log`` should be writable by the same
user. You may however change the logfiles position using the the general or job
based configuration.

Examples of the base configuration file, as well as the job configuration files
are provided inside the ``/etc`` folder of this repository. All available
options are shown in these files. Detailed documentation for each of the
options can be found in the example files themselves.

Usage
=====

To execute a defined backup job a simple call to the duplicity-runner followed
by the job name to execute is sufficient::

    duplicity-runner some-job-name

In case you want to execute the runner using some sort of cron daemon the
output may be silenced by using the ``-q`` option::

    duplicity-runner -q some-job-name

In this case only errors during execution will be outputted to STDERR. All
status messages are effectively silenced this way.
