.. _introduction:

==============
 Introduction
==============

How To Use This Document
------------------------

This guide instructs both novice and experienced users on building and
running CESM. The majority of the CESM User's Guide is contained in the CIME User's Guide.

.. todo:: provide link to CIME User's Guide

If you are a new user, we recommend that the introductory sections of
the CIME User's Guide be read before moving onto other sections or the
Quick Start procedure. This document is written so that, as much as
possible, individual sections stand on their own and the user's guide
can be scanned and sections read in a relatively ad hoc order. In
addition, the web version provides clickable links that tie different
sections together.

The chapters attempt to provide relatively detailed information about
specific aspects of CESM such as setting up a case, building the model,
running the model, porting, and testing. 

::

    Throughout the document, this presentation style indicates shell
    commands and options, fragments of code, namelist variables, etc.
    Where examples from an interactive shell session are presented,
    lines starting with '>' indicate the shell prompt.

Please feel free to provide feedback to CESM about how to improve the
documentation.

CESM Model Version Naming Conventions
-------------------------------------

CESM model release versions include three numbers separated by a dot (.)
- CESM X.Y.Z

-  X - corresponds to the major release number indicating significant
   science changes.

-  Y - corresponds to the addition of new infrastructure and new science
   capabilities for targeted components.

-  Z - corresponds to release bug fixes and machine updates.

CESM Overview
=============

The Community Earth System Model (CESM) is a coupled climate model for
simulating Earth's climate system. Composed of separate models
simultaneously simulating the Earth's atmosphere, ocean, land, land-ice,
and sea-ice, plus one central coupler component, CESM allows researchers
to conduct fundamental research into the Earth's past, present, and
future climate states.

CESM can be run on a number of different `hardware platforms <../modelnl/machines.html>`__, and has a relatively flexible
design with respect to `processor layout <#case_conf_setting_pes>`__ of
components. CESM also supports both an internally developed set of
component interfaces and the ESMF compliant component interfaces (See ?)

The CESM project is a cooperative effort among U.S. climate researchers.
Primarily supported by the National Science Foundation (NSF) and
centered at the National Center for Atmospheric Research (NCAR) in
Boulder, Colorado, the CESM project enjoys close collaborations with the
U.S. Department of Energy and the National Aeronautics and Space
Administration. Scientific development of the CESM is guided by the CESM
working groups, which meet twice a year. The main CESM workshop is held
each year in June to showcase results from the various working groups
and coordinate future CESM developments among the working groups. The
`CESM website <http://www2.cesm.ucar.edu/>`__ provides more information
on the CESM project, such as the management structure, the scientific
working groups, downloadable source code, and online archives of data
from previous CESM experiments.

CESM Software/Operating System Prerequisites
--------------------------------------------

The following are the external system and software requirements for
installing and running CESM.

-  UNIX style operating system such as CNL, AIX and Linux

-  csh, sh, and perl scripting languages

-  subversion client version 1.4.2 or greater

-  Fortran (2003 recommended, 90 required) and C compilers. pgi, intel,
   and xlf are recommended compilers.

-  MPI (although CESM does not absolutely require it for running on one processor)

-  `NetCDF 4.2.0 or
   newer <http://www.unidata.ucar.edu/software/netcdf/>`__.

-  `ESMF 5.2.0 or newer
   (optional) <http://www.earthsystemmodeling.org/>`__.

-  `pnetcdf 1.2.0 is required and 1.3.1 is
   recommended <http://trac.mcs.anl.gov/projects/parallel-netcdf/>`__

-  `Trilinos <http://trilinos.sandia.gov/>`__ may be required for
   certain configurations

-  `LAPACKm <http://www.netlib.org/lapack/>`__ or a vendor supplied
   equivalent may also be required for some configurations.

-  `CMake 2.8.6 or newer <http://www.cmake.org/>`__ is required for
   configurations that include CISM.

The following table contains the version in use at the time of release.
These versions are known to work at the time of the release for the
specified hardware.

+--------------------+-------------------------------+
| Machine            | Version Recommendations       |
+====================+===============================+
| Cray XT Series     | pgf95 12.4.0                  |
+--------------------+-------------------------------+
| IBM Power Series   | xlf 12.1, xlC 10.1            |
+--------------------+-------------------------------+
| IBM Bluegene/P     | xlf 12.01, xlC 10.01          |
+--------------------+-------------------------------+
| Linux Machine      | ifort, icc (intel64) 12.1.4   |
+--------------------+-------------------------------+

Table: Recommmended Software Package Versions by Machine

    **Caution**

    NetCDF must be built with the same Fortran compiler as CESM. In the
    netCDF build the FC environment variable specifies which Fortran
    compiler to use. CESM is written mostly in Fortran, netCDF is
    written in C. Because there is no standard way to call a C program
    from a Fortran program, the Fortran to C layer between CESM and
    netCDF will vary depending on which Fortran compiler you use for
    CESM. When a function in the netCDF library is called from a Fortran
    application, the netCDF Fortran API calls the netCDF C library. If
    you do not use the same compiler to build netCDF and CESM you will
    in most cases get errors from netCDF saying certain netCDF functions
    cannot be found.

Parallel-netCDF, also referred to as pnetcdf, is optional. If a user
chooses to use pnetcdf, version 1.2.0 or later should be used with CESM.
It is a library that is file-format compatible with netCDF, and provides
higher performance by using MPI-IO. Pnetcdf is enabled by setting the
PNETCDF\_PATH variable in the Macros file. You must also specify that
you want pnetcdf at runtime via the io\_typename argument that can be
set to either "netcdf" or "pnetcdf" for each component.

