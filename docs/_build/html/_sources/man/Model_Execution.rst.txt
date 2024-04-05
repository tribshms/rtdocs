Model Execution
===============

The development, operation, and execution of the tRIBS model has been improved significantly for the release under the MIT license. Despite this, it remains the responsibility of the user to provide the model with the appropriate inputs in the correct format and to understand the various means of running the model. 

Compilation Instructions
-------------------------------

tRIBS is written in C++ and must be compiled before use. To facilitate cross-platform compilation and increase the ease of compiling tRIBS in either *parallel* or *serial* mode, we employ the CMake build system. Instructions for compiling tRIBS on your machine using CMake are outlined below. Additionally, we maintain a Docker image that contains both the serial and parallel version of tRIBS as documented `here`_.

    .. _here: https://tribshms.readthedocs.io/en/latest/man/Docker.html

    Note: these instructions are for using CMake via terminal; additional documentation_ is available for using the CMake GUI.

    .. _documentation: https://cmake.org/cmake/help/latest/guide/user-interaction/index.html

CMake
~~~~~

    1. Use `Homebrew`_ to install CMake. Alternatively, you can download CMake_ directly, but Homebrew or a similar package manager is preferred as it will catch additional dependencies.

    .. _Homebrew: https://formulae.brew.sh/formula/cmake
    .. _CMake: https://cmake.org/download/

    2. You can check to see if CMake is on your path by typing `cmake` into the command line. If it says it's not found, then you will need to set cmake to your path. For example, if you downloaded CMake and it's now in your application folder, you can use:

    .. code-block:: bash

        sudo "/Applications/CMake.app/Contents/bin/cmake-gui" --install

    3. Next, change directory to the tRIBS source code. It should look something like this but is dependent on where the tRIBS source code is located:

    .. code-block:: bash

        cd ~/Documents/tRIBS

    4. Once in the directory containing the src code subdirectory and CMakeLists.txt, execute the following code in the terminal:

    .. code-block:: bash

        cmake -S . -B build
        cmake --build build --target all

The first command tells CMake to generate the make files for tRIBS in a folder called build, followed by the second line which effectively compiles the code.

    5. After, you can check to see that the executable was made by using:

    .. code-block:: bash

        ls build/

    The executable will have a name specified in the CMakeLists.txt file. Currently, it is set to tRIBS.

Content of CMakeLists
~~~~~~~~~~~~~~~~~~~~~

In some instance, CMakeLists.txt needs to be modified, for example if you want to change the name of the executable, or specify the parallel or serial build, or add additional compiler flags.

    * Compiling the parallel or serial version of tRIBS can be achieved by setting the variable ``parallel`` to ``ON`` or ``OFF``.

    * The executable name is specified here ``set(exe "tRIBS")``, where "tRIBS" can be modified.

    * Additional compiler flags can be set here: ``set(cxx_flags "-O2")``, in this case an optimization level of 2 is being passed by ``-O2``.

Run Instructions
----------------------

In order to run the tRIBS Model, an Input File is required. This file can have any name, but by convention the extension ``*.in`` is used. The model can be run from the UNIX command line by using the following syntax (within the same directory as the ``tribs`` executable):

    ::

              % tribs inputfile.in [options]

    For running the model in parallel mode, mpirun (or a suitable alternative MPI command) is needed:
    ::

              % mpirun [options] ./tRIBS inputfile.in [options]

Alternatively, the model can be run from a separate directory by specifying the pathnames of the executable and the Input File. **Table 4.1** presents a list of the options with descriptions and default values.

      **Table 4.1** tRIBS Run Options (``*_run``) [NEEDS TO BE UPDATED]

      .. tabularcolumns:: |c|l|c|

      +----------------+-------------------------------------------------------+-------------------+
      |  Run Option    |   Description                                         |  Default Setting  |
      +================+=======================================================+===================+
      |    *-A*        |   Automatic listing of rainfall fields                |  (default = off)  |
      +----------------+-------------------------------------------------------+-------------------+
      |    *-R*        |   Write intermediate states (spatial output)          |  (default = off)  |
      +----------------+-------------------------------------------------------+-------------------+
      |    *-G*        |   Run groundwater model                               |  (default = off)  |
      +----------------+-------------------------------------------------------+-------------------+
      |    *-F*        |   Measured and forecasted rain                        |  (default = off)  |
      +----------------+-------------------------------------------------------+-------------------+
      |    *-M*        |   Do NOT write headers in output files                |  (default = off)  |
      +----------------+-------------------------------------------------------+-------------------+
      |    *-V*        |   [NodeID] Verbose mode (output run-time info)        |  (default = off)  |
      +----------------+-------------------------------------------------------+-------------------+
      |    *-O*        |   On after simulation completion, awaiting user input |  (default = off)  |
      +----------------+-------------------------------------------------------+-------------------+
      |    *-K*        |   Check input file for consistency                    |  (default = on)   |
      +----------------+-------------------------------------------------------+-------------------+
      |    *-H*        |   Write intermediate hydrographs (``*.mrf``)          |  (default = off)  |
      +----------------+-------------------------------------------------------+-------------------+

The most important of these options for the new user to be acquainted with are *-R* (write intermediate results), *-G* (run the groundwater model), *-V* (verbose screen output), *-O* (continuously on model state). The last of these should be used only if one wishes to keep the model in memory while changing the inputs specified in the Input File (all model inputs except the TIN Mesh can be altered). 
