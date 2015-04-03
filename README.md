Manta Structural Variant Caller
===============================

Version: NOT RELEASED

Manta is a tool to call structural variants and indels from short paired-end
sequencing reads. It combines paired-end and split read evidence during SV
discovery and scoring to improve performance, but does not require split reads
or successful breakpoint assemblies to report a variant in cases where there is
strong evidence of an imprecise variant. It provides genotypes and quality
scores for variants in single diploid samples, and will also call somatic
variants when a matched tumor sample is specified. Manta can detect all classes
of structural variants which can be identified in the absence of copy number
analysis and large-scale assembly. See the user guide for a full description of
capabilities and limitations.


Build instructions
------------------

For Manta users it is strongly recommended to start from one of the release
distributions of the source code provided on the Manta [releases] page. Acquiring
the source via a git clone or archive could result in missing version number
entries, undesirably stringent build requirements, or an unstable development
intermediate between releases. Additional build notes for developers can be
found below.

Note that this README is _NOT_ part of an end-user release distribution.

[releases]:https://github.com/StructuralVariants/manta/releases

### Prerequisites

Manta has been built and tested on linux systems only. It is currently
maintained for CentOS 5,6 and Ubuntu 12.04,14.04 (with gcc updated to meet
the minimum version where required).

#### Compilation prerequisites:

Manta requires a compiler supporting most of the C++11 standard. These are the
current minimum versions enforced by the build system:

* python 2.4+
* gcc 4.7+ OR clang 3.2+ (OR Visual Studio 2013+, see windev note below)
* libz (including headers)

#### Runtime prerequisites

* python 2.4+

#### Prerequisite package names (RHEL/CentOS)

* g++
* make
* zlib-devel

### Build procedure

After acquiring a release distribution of the source code, the build procedure is:

* Unpack the source code
* Create a separate `build` directory (note an out-of-source build is
  required.)
* Configure the build
* Compile
* Install

Example:

    wget https://github.com/StructuralVariants/manta/releases/download/vA.B.C/manta-A.B.C.tar.bz2
    tar -xjf manta-A.B.C.tar.bz2
    mkdir build
    cd build
    ../manta-A.B.C/src/configure --prefix=/path/to/install
    make
    make install

Note that during the configuration step, the following compilation
dependencies will be built if these are not found:

* cmake 2.8.0+
* boost 1.53.0

To optionally avoid this extra step, ensure that (1) cmake is in your PATH and (2)
BOOST\_ROOT is defined to point to boost 1.53.0 (the boost version is required to
be an exact match). If either of these dependencies are not found, they will be
built during the configuration step, To accelerate this process it may be
desirable to parallelize the configure step over multiple cores. To do so
provide the `--jobs` argument to the configuration script. For example:

    ${MANTA_SRC_PATH}/configure --prefix=/path/to/install --jobs=4

Compiling Manta itself can also be parallelized by the standard make procedure, e.g.

    make -j4
    make install

To see more configure options, run:

    ${MANTA_SRC_PATH}/configure --help


Data analysis and Interpretation
--------------------------------

After completing the installation, see the Manta user guide for instructions on
how to use Manta, interpret results, and a high-level overview of the method.
The user guide can be found in:

    ${MANTA_INSTALL_PATH}/doc/html/mantaUserGuide.html


Developer build configuration
-----------------------------

When the Manta source is cloned from github, it is configured for development
rather than end-user distribution. As such, all builds include -Werror. If
cppcheck is found any detected issue is converted to a build error.


Windows developer support
-------------------------

Manta does not link or run on windows. The build system does however
facilitate developers preferring to use Visual Studio. During
windows cmake configuration all final library linking is disabled and all
third party libraries are unpacked such that their headers can be
included, but the libraries are not compiled. Cmake generated VS solutions allow
the c++ code to be browsed, analyzed and compiled to the library level.
Note that unit tests can not be run under this scheme -- just like the other
runtime binaries, they can't be linked without building 3rd party libraries.

Note that the c++11 features used by manta require at least Visual Studio
2013. In addition to VS2013 and cmake, a zlib installation is required. The
simplist way to do this may be to use the gnuwin32 pacakge here:

http://gnuwin32.sourceforge.net/packages/zlib.htm

This library will enable building for 32 bit only.

