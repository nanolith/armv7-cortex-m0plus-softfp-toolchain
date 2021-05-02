ARM Cortex M0+ Toolchain Builder
================================

The script in this project builds a toolchain for compiling and linking firmware
written in C and C++ for ARMv7 Cortex M0+ parts.  This script assumes a soft
float and soft float ABI.  If others are interested in making this configurable,
it would be pretty easy to do.

The script downloads, verifies, extracts, configures, tests, and installs the
major prerequisites for building Binutils and GCC.  It then builds a bootstrap
version of GCC, uses this to build Newlib, and then builds the full GCC suite
for C and C++.

Assumptions
-----------

This script assumes a Unix-like environment (e.g. OS X, Linux, \*BSD, Cygwin,
Windows 10?).  It performs some basic sanity checking to ensure that requisite
utilities are available.  However, I have only tested this script on OS X and
Linux.  Please open a PR if you need to modify this script for your use or if
you want to make non-breaking enhancements.

Limitations
-----------

While this script uses GPG to verify signatures and adds missing public keys to
the keyring, it does not perform any steps to verify that these public keys are
valid.  It fetches them from the MIT key server, but for true security, each of
these public keys should be manually verified.  At some point in the future,
this script may be enhanced to perform proper sanity checking, but as it stands,
signature verification only counts to ensure that the tarballs downloaded have
been downloaded completely.  The actual authenticity of these tarballs should
not be assumed.  That being said, this is typically more checking than the
average porting tool does, so tinfoil types are welcome to send me a PR that
enhances this script to do the right thing here, and I'll review it.

License
-------

This script is hereby placed in the public domain.  It may be used for any
purpose.  I provide no warranty for this script, nor do I take any
responsibility for anything it may do.  Use it at your own risk.  I highly
suggest reading the script so you understand what it does before you execute it.

Build Requirements
------------------

To run this script, you will need the following tools.

    gcc
    g++
    git
    curl
    openssl
    gpg
    gzip
    bzip2
    xz
    sed
    libtool

Since these tools often come with the OS or have OS-specific requirements that I
can't anticipate, this script does not fetch or build these if they are missing.
Instead, I suggest installing these using the package management tools for your
platform.  This script will not download and install these.  It will, however,
check that these tools are within the current path.

Usage Instructions
------------------

To use this script, the environment variable, ARM\_TOOLCHAIN\_DIR, should be set
to the installation location.  This directory needs to be created ahead of time.
The script will verify that it exists.  The CC and CXX environment variables can
be set to the compiler to be used to build this toolchain.  In the case of OS X,
it is highly recommended that a modern GCC implementation is used to build this
toolchain.  Clang / LLVM has some problems dealing with generated code in the
ARM target.

Optionally, the MAKE\_OPTS variable can be set for options to be passed to GNU
Make during builds.  For instance, setting the jobs option to the number of
cores on your machine will significantly speed up builds (e.g. MAKE\_OPTS=-j4 or
MAKE\_OPTS=-j8).

The script should be run in a scratch directory.  If you clone this repo, you
can use the workspace as your scratch directory for one-off builds.  Anyone
planning to enhance this script should use a separate scratch directory.  As the
script runs, it will download tarballs, create build subdirectories in the
scratch directory, and create dot files to track its progress.  The script can
recover from failed downloads if the failed tarball is deleted, and it can
restart the extract, configure, build, test, and install steps for each of the
packages if it is interrupted, or if you need to fix a portability issue.  For
most users, the script should just execute once, after which point, the ARM
toolchain will exist in the directory pointed to by the ARM\_TOOLCHAIN\_DIR
environment variable.

Package Versions
----------------

This script currently uses the following packages and versions.  Note that this
list should be updated if these versions are changed.

| Package      | Version              |
|-------------:|:---------------------|
| GMP          | 6.2.1                |
| MPFR         | 4.1.0                |
| MPC          | 1.2.1                |
| binutils     | 2.36.1               |
| gcc          | 11.1.0               |
| newlib       | 4.1.0                |
| gdb          | 10.2.0               |
