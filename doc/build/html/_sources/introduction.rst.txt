.. _introduction:

==============
 Introduction
==============

How To Use This Document
------------------------

This guide instructs both novice and experienced users on building and
running CESM2.   CESM2 is built on the CIME_ framework.

The majority of the CESM User's Guide is contained in the CIME_ User's Guide.

If you are a new user, we recommend that the introductory sections of
the CIME_ User's Guide be read before moving onto other sections or the
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

Please feel free to provide feedback to the `CESM forum <https://bb.cgd.ucar.edu/>`_ about how to improve the
documentation.  

CESM Model Version Naming Conventions
-------------------------------------

CESM model release versions include three numbers separated by an underscore (_)
- CESM X_Y_Z

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

CESM can be run on a number of different `hardware platforms <http://www.cesm.ucar.edu/models/cesm2.0/cesm/machines.html>`__, and has a relatively flexible
design with respect to `processor layout <http://esmci.github.io/cime/doc/build/html/users_guide/customizing-a-case.html#customizing-the-pe-layout>`__ of
components.

The CESM project is a cooperative effort among U.S. climate researchers.
Primarily supported by the `National Science Foundation (NSF) <https://www.nsf.gov/>`_ and
centered at the `National Center for Atmospheric Research (NCAR) <https://ncar.ucar.edu/>`_ in
Boulder, Colorado, the CESM project enjoys close collaborations with the
`U.S. Department of Energy (DOE) <https://energy.gov/>`_ and the
`National Aeronautics and Space Administration (NASA) <http://www.nasa.gov>`_.
Scientific development of the CESM is guided by the CESM
working groups, which meet twice a year. The main CESM workshop is held
each year in June to showcase results from the various working groups
and coordinate future CESM developments among the working groups. The
`CESM website <http://www.cesm.ucar.edu/>`__ provides more information
on the CESM project, such as the management structure, the scientific
working groups, downloadable source code, and online archives of data
from previous CESM experiments.

CESM Software/Operating System Prerequisites
--------------------------------------------

The following are the external system and software requirements for
installing and running CESM.

-  UNIX style operating system such as CNL, AIX or Linux

-  python 2.7 and perl 5 scripting languages

-  subversion client version (1.8 or greater recommended)

-  Fortran (2008 recommended, 90 required) and C compilers. 

-  MPI (although CESM does not absolutely require it for running on one processor)

-  `NetCDF 4.3 or
   newer <http://www.unidata.ucar.edu/software/netcdf/>`__.

-  `ESMF 5.2.0 or newer (optional) <http://www.earthsystemmodeling.org/>`__.

-  `pnetcdf 1.7.0 is required and 1.8.1 is optional but recommended <http://trac.mcs.anl.gov/projects/parallel-netcdf/>`__

-  `Trilinos <http://trilinos.sandia.gov/>`__ may be required for
   certain configurations 

-  `LAPACK <http://www.netlib.org/lapack/>`__ or a vendor supplied
   equivalent may also be required for some configurations.

-  `CMake 2.8.6 or newer <http://www.cmake.org/>`__ 

.. warning:: NetCDF must be built with the same Fortran compiler as CESM. In the netCDF build the FC environment variable specifies which Fortran compiler to use. CESM is written mostly in Fortran, netCDF is written in C. Because there is no standard way to call a C program from a Fortran program, the Fortran to C layer between CESM and netCDF will vary depending on which Fortran compiler you use for CESM. When a function in the netCDF library is called from a Fortran application, the netCDF Fortran API calls the netCDF C library. If you do not use the same compiler to build netCDF and CESM you will in most cases get errors from netCDF saying certain netCDF functions cannot be found.

Parallel-netCDF, also referred to as pnetcdf, is optional. If a user
chooses to use pnetcdf, version 1.7.0 or later should be used with CESM.
It is a library that is file-format compatible with netCDF, and provides
higher performance by using MPI-IO. Pnetcdf is enabled by setting the
PNETCDF\_PATH variable in the Macros file. 

.. _CIME: http://esmci.github.io/cime/doc/build/html/index.html
