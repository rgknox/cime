.. _downloading:

==================
 Downloading CESM
==================

Downloading the code and scripts
--------------------------------

CESM release code are available through a Subversion
repository. Access to the code requires Subversion client software
in place that is compatible with our Subversion server software, such
as a recent version of the command line client, svn. Currently, our
server software is at version 1.8.17. We recommend using a client at
version 1.8 or later, though older versions may suffice. For more information or to
download open source tools, visit:

::

    http://subversion.tigris.org/

With a valid svn client installed on the machine where CESM will be
built and run, the user may download the latest version of the release
code. First view the available release versions with the
following command:

::

    > svn list https://svn-ccsm-models.cgd.ucar.edu/cesm2/release/tags

When contacting the Subversion server for the first time, the following
certificate message will likely be generated:

::

    Error validating server certificate for
    'https://svn-ccsm-models.cgd.ucar.edu:443':
     - The certificate is not issued by a trusted authority. Use the
       fingerprint to validate the certificate manually!
    Certificate information:
     - Hostname: *.cgd.ucar.edu
     - Valid: from Tue, 12 Jun 2012 00:00:00 GMT until Wed, 17 Jun 2015
    12:00:00 GMT
     - Issuer: www.digicert.com, DigiCert Inc, US
     - Fingerprint: eb:30:7d:c5:06:e6:b1:6f:e8:4f:c6:0a:79:6f:af:ec:5c:18:e2:32
    (R)eject, accept (t)emporarily or accept (p)ermanently? p

After accepting the certificate, the following authentication message
will likely be generated:

::

    svn list https://svn-ccsm-models.cgd.ucar.edu/cesm2/release/tags
    Authentication realm: <https://svn-ccsm-models.cgd.ucar.edu:443> ccsm:models
    Password for '[username]': 
    Authentication realm: <https://svn-ccsm-models.cgd.ucar.edu:443D> ccsm:models
    Username: guestuser
    Password for 'guestuser': 

    -----------------------------------------------------------------------
    ATTENTION!  Your password for authentication realm:

       <https://svn-ccsm-models.cgd.ucar.edu:443> ccsm:models

    can only be stored to disk unencrypted!  You are advised to configure
    your system so that Subversion can store passwords encrypted, if
    possible.  See the documentation for details.

    You can avoid future appearances of this warning by setting the value
    of the 'store-plaintext-passwords' option to either 'yes' or 'no' in
    '$HOME/.subversion/servers'.
    -----------------------------------------------------------------------
    Store password unencrypted (yes/no)? yes
    cesm2_0/

Be aware that the request is set to the current machine login id and you
must enter the CESM registered default username of 'guestuser' by
pressing the 'Enter' key when prompted for a Username.

You may be prompted up to 3 times for the username and password when
checking out the code for the first time from this new Subversion path.
This is because the code is distributed across a number of different
Subversion repositories and each repository requires authentication.

Once correctly entered, the username and password will be cached in a
protected subdirectory of the user's home directory so that repeated
entry of this information will not be required for a given machine.

The release tags should follow a recognizable naming pattern, and they
can be checked out from the central source repository into a local
sandbox directory. The following example shows how to checkout model
version CESM2.0.0:

::

    > svn co https://svn-ccsm-models.cgd.ucar.edu/cesm1/release/tags/cesm2_0_0

.. warning:: If a problem was encountered during checkout, which may happen with an older version of the client software, it may appear to have downloaded successfully, but in fact only a partial checkout has occurred. To ensure a successful download, make sure the last line of svn output has the following statement:

    ::

        Checked out revision XXXXX.

This will create a directory called ``cesm2_0_0`` that can be used to
modify, build, and run the model. The following Subversion subcommands
can be executed in the working sandbox directory.

For various information regarding the release version checked out...

::

    > svn info       

For a listing of files that have changed since checkout...

::

    > svn status 

For a description of the changes made to the working copy...

::

    > svn diff 

Downloading input data
----------------------

Input datasets are needed to run the model. CESM input data will be made
available through a separate Subversion input data repository. The
username and password for the input data repository will be the same as
for the code repository.

.. warning:: The input data repository contains datasets for many configurations and resolutions and is well over 2 TByte in total size. DO NOT try to download the entire dataset.

Datasets can be downloaded on a case by case basis as needed and CESM provides tools to check and download input data automatically.

A local input data directory should exist on the local disk, and it also 
needs to be set in the CESM scripts via the variable ``$DIN\_LOC\_ROOT.``
For supported machines, this variable is preset. For generic machines,
this variable is set as an argument to ``create\_newcase``. Multiple users
can share the same ``$DIN\_LOC\_ROOT`` directory.

The files in the subdirectories of ``$DIN\_LOC\_ROOT`` should be
write-protected. This prevents these files from being accidentally
modified or deleted. The directories in ``$DIN\_LOC\_ROOT`` should generally
be group writable, so the directory can be shared among multiple users.

As part of the process of generating the CESM executable, the utility,
``check_input_data`` 

.. todo:: put lin link to check_iput_data

is called, and it attempts to locate all required input data for the
case based upon file lists generated by components. If the required
data is not found on local disk in ``$DIN\_LOC\_ROOT``, then the data
will be downloaded automatically by the scripts or it can be
downloaded by the user by invoking ``check\_input\_data`` with the -export
command argument. If you want to download the input data manually you
should do it before you build CESM.

It is possible for users to download the data using svn subcommands
directly, but use of the ``check\_input\_data script`` is highly recommended
to ensure that only the required datasets are downloaded. Again, users
are **STRONGLY DISCOURAGED** from downloading the entire input dataset from
the repository due to the size.

