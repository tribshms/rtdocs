Executables
===========
tRIBS executables for both MacOS (compatible with Intel or Silicon chips) and Ubuntu are provided here as self-extracting compressed archive files. These files are formatted as shell scripts and can be unpacked by running the file from the command line. You will be prompted with the license and option for installing in the default ``TIN-based Real-time Integrated Basin Simulator-5.2.0`` directory or a ``bin`` directory. Each file contains both a serial (tRIBS) and parallel (tRIBSpar) version of the model. These executables are only compatible with the specific operating systems listed below. Additionally, in order to run tRIBSpar you will need to have version 5.0 of `OpenMPI <https://open-mpi.org/>`_ installed. If one of these executatbles is not compatible with you operating system we recommend that you either use the tRIBS :doc:`Docker` Image or build the tRIBS from the source code following

macOS
-----------

Silicon
~~~~~~~
Built on M1/Sonoma MacOS with OpenMPI 5.0.3

:download:`Download tRIBS-5.2.0-Silicon.sh <https://github.com/tribshms/tRIBS/raw/main/packaging/macOS/Silicon/tRIBS-5.2.0-Silicon.sh>`.

Intel
~~~~~
Built on Intel/Ventura MacOS with OpenMPI 5.0.3

:download:`Download tRIBS-5.2.0-Intel.sh  <https://github.com/tribshms/tRIBS/raw/main/packaging/macOS/Intel/tRIBS-5.2.0-Intel.sh>`.

Linux
-------------

Ubuntu
~~~~~~
Built on Ubuntu 22.04 with OpenMPI 5.0.3

:download:`Download tRIBS-5.2.0-Ubuntu.sh <https://github.com/tribshms/tRIBS/raw/main/packaging/Ubuntu/tRIBS-5.2.0-Ubuntu.sh>`
