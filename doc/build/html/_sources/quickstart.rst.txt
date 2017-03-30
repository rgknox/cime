.. _quickstart:

=============================
 Quick Start (CESM Workflow)
=============================

The following quick start guide is for versions of CESM that have
already been ported to the local target machine. If &cesm has not yet
been ported to the target machine, please see ?. If you are new to CESM,
please consider reading the `introduction <#ccsm_overview>`__ first

These definitions are required to understand this section:

-  $COMPSET refers to the component set.

-  $RES refers to the model resolution.

-  $MACH refers to the target machine.

-  $CIMEROOT refers to the CIME root directory.

-  $CASE refers to the case name.

-  $CASEROOT refers to the full pathname of the root directory where the
   case ($CASE) will be created.

-  $EXEROOT refers to the executable directory. ($EXEROOT IS NORMALLY
   NOT THE SAME AS $&CASEROOT).

-  $RUNDIR refers to the directory where CESM actually runs. This is
   normally set to $&EXEROOT/run. (Note: changing $&EXEROOT does not
   change $&RUNDIR as these are independent variables.)

This is the procedure for quickly setting up and running a CESM case.

Download CESM (see `Download CESM <#download_ccsm>`__).

Select a machine, a component set, and a resolution from the list
displayed after invoking this command:

::

    > cd $CIMEROOT/scripts
    > create_newcase -list

See the `supported component sets <../modelnl/compsets.html>`__,
`supported model resolutions <../modelnl/grid.html>`__ and `supported
machines <../modelnl/machines.html>`__. for a complete list of CESM
supported component sets, grids and computational platforms.

Create a case.

The CREATE\_NEWCASE command creates a case directory containing the
scripts and xml files to configure a case (see below) for the requested
resolution, component set, and machine. CREATE\_NEWCASE has several
required arguments and if a generic machine is used, several additional
options must be set (invoke create\_newcase -h for help).

If running on a supported machine, ($MACH), then invoke CREATE\_NEWCASE
as follows:

::

    > create_newcase -case $CASEROOT \
             -mach $MACH \
             -compset $COMPSET \
             -res $RES 

If running on a new target machine, see porting in ?.

Setting up the case run script

Issuing the CESM\_SETUP command creates a $CASEROOT/$CASE.run script
along with user\_nl\_xxx files, where xxx denotes the set of components
for the given case configuration. Before invoking CESM\_SETUP, modify
the env\_mach\_pes.xml file in $CASEROOT as needed for the experiment.

``cd`` to the $CASEROOT directory.

::

    > cd $CASEROOT

Modify settings in ENV\_MACH\_PES.XML (optional). (Note: To edit any of
the env xml files, use the `xmlchange <#faq_xmlchange>`__ command.
invoke XMLCHANGE -h for help.)

Invoke the CESM\_SETUP command.

::

    > ./cesm_setup  

Build the executable.

Modify build settings in ENV\_BUILD.XML (optional).

Run the build script.

::

    > $CASE.build 

Run the case.

Modify runtime settings in ENV\_RUN.XML (optional). In particular, set
the $DOUT\_S variable to FALSE.

Submit the job to the batch queue.

::

    > $CASE.submit

When the job is complete, review the following directories and files

$RUNDIR. This directory is set in the env\_build.xml file. This is the
location where CESM was run. There should be log files there for every
component (ie. of the form cpl.log.yymmdd-hhmmss). Each component writes
its own log file. Also see whether any restart or history files were
written. To check that a run completed successfully, check the last
several lines of the cpl.log file for the string " SUCCESSFUL
TERMINATION OF CPL7-CCSM ".

$CASEROOT/logs. The log files should have been copied into this
directory if the run completed successfully.

$CASEROOT. There could be a standard out and/or standard error file.

$CASEROOT/CaseDocs. The case namelist files are copied into this
directory from the $RUNDIR.

$CASEROOT/timing. There should be a couple of timing files there that
summarize the model performance.

$DOUT\_S\_ROOT/$CASE. This is the archive directory. If $DOUT\_S is
FALSE, then no archive directory should exist. If $DOUT\_S is TRUE, then
log, history, and restart files should have been copied into a directory
tree here.
