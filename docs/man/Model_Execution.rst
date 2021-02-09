
4.0 Model Execution
=====================

Execution of the tRIBS Model has been improved significantly for this release. A concerted effort has been made to provide a well document, standardized set of model inputs for the various types of formats previously discussed. In addition, the model output has been improved significantly in order to make use of the visualization capabilities of ArcView GIS and the time series analysis of MATLAB. Model screen output has been tailored to provide the user with only the necessary information about the status of the model run and sufficient detail as to possible errors. Despite this, the tRIBS model is a research-based code that does not provide as much error checking as could be possible or desirable. For this reason, the responsibility is placed on the user to provide the model with the appropriate inputs in the correct format. This document describes all model input and output in sufficient detail for the beginning user. More advanced users should consult the sample applications provided or the model code.

Execution of the parallel tRIBS Model is still quite experimental. We have not optimized the subbasins partitioning to achieve equitable distribution of labor on the individual processors. Nevertheless, the parallel version of the tRIBS Model is operational and can be tested by users for their own purposes. Most of the functionality (e.g., input, output, directory structure, etc.) remains identical to the original tRIBS model.

4.1 Download instructions
---------------------------

    The release of the tRIBS Model is intended solely for use as a research hydrology model. The distribution of the model code is provided only upon written consent from the authors. The tRIBS code is provided in a single tar formatted file called ``tribs.tar``. The file should be unpacked using the tar UNIX utility and placed in a directory of choice:
    ::

            % tar -xvf tribs.tar


    The tar bundle will contain the subdirectory structure and code files presented in **Tables 2.1** and **2.2**. An executable is not included in the tar bundle. A sample application for a synthetic element and for a complex watershed (Peacheater Creek) are available along with the model tar bundle. For most users, only an executable in the appropriate platform is provided along with the model examples.

4.2 Compilation Instructions
-------------------------------

    The tRIBS Model was originally written and compiled on an SGI Irix 6.5 UNIX system. Since the model design uses standard C++ programming structures, the compilation onto other UNIX platforms is straightforward. tRIBS has been compiled for a range of platforms (Mac G5, Alpha Tru64, Linux, Cygwin, IBM, Solaris). The current version should compile on all platforms without need for any major changes to the code by using the different makefiles provided. As an example, the model code is compiled for SUN Solaris by issuing the following command at the UNIX prompt:
    ::

            % make -f makeSUN [options]


    The makefile provided allows for various options to be included after the command: *clean* (erases the previously compiled objects) and *all* (cleans and compiles the code). If no options are included, then the command simply compiles the code without removing the previous objects. The user should be warned that the compilation of certain low-level objects requires all objects to be recreated for proper functioning. For this reason, the *all* option is recommended. The make utility will create an executable called ``tribs`` in the current working directory. It will also place the object files under a directory ``_Objects_`` that must be created within the directory of the executable.

    To run the model using the parallelized option, the operating system requries the appropriate MPI libraries. The current version should compile on other platforms with MPI libraries without need for any major changes to the code. Several makefiles are provided for the particular compilation of tRIBS (``makeLINUX_PAR`` and ``makeMAC_PAR``). As an example, the model code is compiled for LINUX by issuing the following command at the prompt
    ::

            % make -f makeLINUX_PAR [options]


4.3 Run Instructions
----------------------

    In order to run the tRIBS Model, a Model Input File will be required. This file can have any name, but by convention the extension ``*.in`` is used. The model can be run from the UNIX command line by using the following syntax (within the same directory as the ``tribs`` executable):
    ::

              % tribs inputfile.in [options]


    For running the model in parallel mode, mpirun (or a suitable alternative MPI command) is needed:
    ::

              % mpirun [options] ./tRIBS inputfile.in [options]


    In the above, the command mpirun is particular for parallel model applications and it contains various possible options related to the assignment of particular processors. The user is recommend to check ``man mpirun`` and also the sample applications for the tRIBS Model.

    Alternatively, the model can be run from a separate directory by specifying the pathnames of the executable and the Model Input File. An easier way of running the model is to create a Model Run File that contains the one line UNIX command presented above. Although Model Run Files can have any name, the extension ``*_run`` is used by convention. It is important to make sure that the Model Run File has read, write and execute permissions. These can be changed using the UNIX utility ``chmod``.  Running the Model Run File (called here inputfile_run) would consist of issuing the following command:
    ::

              % inputfile_run > output


    The arrow key (``>``) redirects the model screen output to a file called ``output``. Without the arrow key, the model output associated with the progress of the run is directed to the screen. Various command line options are available when running the tRIBS model, either through the command line or through the Model Run File. **Table 4.1** presents a list of these options with descriptions and default values.

      **Table 4.1** tRIBS Run Options (``*_run``)

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

    The most important of these options for the new user to be acquainted with are *-R* (write intermediate results), *-G* (run the groundwater model), *-V* (verbose screen output), *-O* (continuously on model state). The last of these should be used only if one wishes to keep the model in memory while changing the inputs specified in the Model Input File (all model inputs except the TIN Mesh can be altered). Redirecting the model screen output should not be done if the *-O* flag is used. Many of the other options have yet to be used to the fullest potential in tRIBS, especially those concerned with the use of forecasted rainfall. Further details on the run options are available in the ``tControl.cpp`` code.

4.4 Creating Model Inputs
-----------------------------

    As with any model, half the battle in getting a correct model run is in providing the appropriate model input. Without having all the correct model input, the tRIBS Model will exit with an appropriate error message. A properly setup run will begin and end by providing the user with the following message:
    ::


        ------------------------------------------------------------------------
                        tRIBS Distributed Hydrological Model
                        TIN-based Real-time Integrated Basin Simulator
                        Ralph M. Parsons Laboratory
                        Massachusetts Institute of Technology
                        Version Number and Release Date
        ------------------------------------------------------------------------

    In between this header and footer, the model run output file obtained after redirecting the run file (``output``) will contain various sections relating to the model workflow: Reading Input Parameters, Creating Mesh, Creating Stream Network, Creating Resampling for Grids, Creating Output Files, Creating Hydrologic System, Hydrologic Simulation Loop, Deleting Objects and Exiting Program. A sample output or log file can be found in the sample applications described previously. In order to obtain a proper model run, however, the user must make sure that all model inputs, parameters and files are appropriately constructed and referenced in the Model Input File.

4.4.1 Model Input File
^^^^^^^^^^^^^^^^^^^^^^^^

        The tRIBS Model Input File (``*.in``) is currently the primary user interface to the model. Although not a graphical medium, it is an easy and efficient means of manipulating all modeling options, parameters and inputs. Not all of the parameters are required for every single run since choosing a particular option may require some additional parameters that an alternate option may not. Nevertheless, it is recommended to have as complete a set of parameters as possible. Those parameters that are not required for a particular run are ignored by the model. The ``*.in`` file contains various required and optional parameters organized by keywords. The format for each parameter consists of a line of descriptive text followed by the value of the parameter itself on a second line. **Table 4.2** presents a list of the model parameters used in the tRIBS Model Input File. Note that all parameters are capitalized. The values associated with each parameter may be a number (int, double) or a string (pathname, extension). If the units are specified as ints or doubles, this implies that the parameters are dimensionless, otherwise a unit is expressed. The difference between a pathname and a base pathname is simply that the pathname includes the entire path plus the entire name of the file, including the extension, while a base pathname is only the path and the base name of the file (no extension). NOTE: All keywords in the inputfile must have a entry for proper model execution.

        **Table 4.2** List of Model Parameters in tRIBS Model Input File



--------------------------------------------------------------------------------
          *Last Update: 02-08-2021  C. Lizarraga 
