.. _quickstart:

=============================
 Quick Start (CIME Model Workflow)
=============================

The following quick start guide is for versions of CIME that have
already been ported to the local target machine. If &cime has not yet
been ported to the target machine, please see ?. If you are new to CIME,
please consider reading the `introduction <#ccsm_overview>`__ first

These definitions are required to understand this section:

-  $COMPSET refers to the component set.

-  $RES refers to the model resolution.

-  $MACH refers to the target machine.

-  $CIMEROOT refers to the CIME root directory.

-  $CASE refers to the case name.

-  $CASEROOT refers to the full pathname of the root directory where the
   case ($CASE) will be created.

-  $EXEROOT refers to the build directory. ($EXEROOT IS NORMALLY
   NOT THE SAME AS $&CASEROOT).

-  $RUNDIR refers to the directory where CESM actually runs. This is
   normally set to $&EXEROOT/../run. (Note: changing $&EXEROOT does not
   change $&RUNDIR as these are independent variables. However by default
   they are both located under $&CIME_OUTPUT_ROOT)

This is the procedure for quickly setting up and running a CIME case.

Download CIME  (see `Download CESM <#download_cesm>`__).

Select a component set, and a resolution for your case.  Details of available
component sets and resolutions are available from the manage_case tool located
in the $CIMEROOT/scripts directory

::

    > cd $CIMEROOT/scripts
    > manage_case --help

See the `supported component sets <../modelnl/compsets.html>`__,
`supported model resolutions <../modelnl/grid.html>`__ and `supported
machines <../modelnl/machines.html>`__. for a complete list of CESM
supported component sets, grids and computational platforms.

Create a case.

The CREATE\_NEWCASE command creates a case directory containing the
scripts and xml files to configure a case (see below) for the requested
resolution, component set, and machine. CREATE\_NEWCASE has several
required arguments (invoke create\_newcase --help for help).

If running on a supported machine, ($MACH), that machine will normally be recognized
automatically and therefore it is not required to specify the --machine argument to create_newcase
then invoke CREATE\_NEWCASE
as follows:

::

    > create_newcase --case $CASEROOT \
             --compset $COMPSET \
             --res $RES 

If running on a new target machine, see porting in ?.

Setting up the case run script

Issuing the CASE\_SETUP command creates a $CASEROOT/case.run script
along with user\_nl\_xxx files, where xxx denotes the set of components
for the given case configuration. Before invoking CASE\_SETUP, modify
the env\_mach\_pes.xml file in $CASEROOT using the xmlchange command
as needed for the experiment.

``cd`` to the $CASEROOT directory.

::

    > cd $CASEROOT

Modify settings in ENV\_MACH\_PES.XML (optional). (Note: To edit any of
the env xml files, use the `xmlchange <#faq_xmlchange>`__ command.
invoke XMLCHANGE -h for help.)

Invoke the CASE\_SETUP command.

::

    > ./case_setup  

Build the executable using the case.build command.

Modify build settings in ENV\_BUILD.XML (optional).

Run the build script.

::

    > case.build 

Run the case.

Modify runtime settings in ENV\_RUN.XML (optional). In particular, set
the $DOUT\_S variable to FALSE.

Submit the job to the batch queue.

::

    > case.submit

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
