
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

            .. tabularcolumns:: |l|l|l|

            +-----------------------+-----------------+----------------------------------------------------+
            |  Keyword              |   Units         |    Description                                     |
            +=======================+=================+====================================================+
            |  *STARTDATE*          |   *date format* |   Date of start of simulation                      |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *RUNTIME*            |   *hours*       |   Total number of hours in run                     |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *TIMESTEP*           |   *mins*        |   Unsaturated zone time step                       |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *GWSTEP*             |   *mins*        |   Saturated zone time step                         |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *METSTEP*            |   *mins*        |   Meteorological data input time step              |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *ETISTEP*            |   *hours*       |   ET, interception and snow time step              |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *RAININTRVL*         |   *hours*       |   Rainfall data input time step                    |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPINTRVL*           |   *hours*       |   Output interval                                  |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *SPOPINTRVL*         |   *hours*       |   Spatial output interval                          |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *INTSTORMMAX*        |   *hours*       |   Interstorm interval                              |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *RAINSEARCH*         |   *hours*       |   Rainfall search interval                         |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *BASEFLOW*           |   *m3/s*        |   Minimum baseflow discharge                       |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *VELOCITYCOEF*       |   *double*      |   Discharge-velocity coefficient                   |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *KINEMVELCOEF*       |   *double*      |   Kinematic routing velocity coefficient           |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *VELOCITYRATIO*      |   *double*      |   Stream-hillslope velocity coefficient            |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *FLOWEXP*            |   *double*      |   Nonlinear discharge coefficient                  |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *CHANNELROUGHNESS*   |   *double*      |   Uniform channel roughness value                  |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *CHANNELWIDTH*       |   *double*      |   Uniform channel width                            |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *CHANNELWIDTHCOEFF*  |   *double*      |   Coefficient in width-area relation               |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *CHANNELWIDTHEXPNT*  |   *double*      |   Exponent in width-area relation                  |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *CHANNELWIDTHFILE*   |   *pathname*    |   Input file name for channel widths               |
            +-----------------------+-----------------+----------------------------------------------------+
            | *CHANNELCONDUCTIVITY* |   *double*      |   Hydraulic conductivity in channel                |
            +-----------------------+-----------------+----------------------------------------------------+
            |*TRANSIENTCONDUCTIVITY*|  *double*       |   Hydraulic conductivity during transient period   |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *TRANSIENTTIME*      |   *double*      |   Time until transient period ends                 |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *CHANNELPOROSITY*    |   *double*      |   Porosity in channel                              |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *CHANPOREINDEX*      |   *double*      |   Pore index parameter in channel                  |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *CHANPSIB*           |   *double*      |   Matric potential in channel                      |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTMESHINPUT*       |   *int*         |   Option for Mesh generation                       |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *RAINSOURCE*         |   *int*         |   Source of rainfall data                          |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTEVAPOTRANS*      |   *int*         |   Option for Evapotranspiration scheme             |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTSNOW*            |   *int*         |   Option for snow                                  |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *HILLALBOPT*         |   *int*         |   Option for hillslope albedo                      |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTRADSHELT*        |   *int*         |   Option for radiation sheltering of snow          |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTINTERCEPT*       |   *int*         |   Option for Interception scheme                   |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTLANDUSE*         |   *int*         |   Option for static or dynamic land cover          |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTLUINTERP*        |   *int*         |   Option for land cover interpolation              |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTSOILTYPE*        |   *int*         |   Option for soil parameter format                 |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *GFLUXOPTION*        |   *int*         |   Option for Ground heat flux scheme               |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *METDATAOPTION*      |   *int*         |   Point or Grid weather data                       |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *CONVERTDATA*        |   *int*         |   Processing weather or raingauge data             |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTBEDROCK*         |   *int*         |   Option for uniform or variable depth             |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTGWFILE*          |   *int*         |   Option for groundwater input file                |
            +-----------------------+-----------------+----------------------------------------------------+
            | *WIDTHINTERPOLATION*  |   *int*         |   Option for interpolating width variables         |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTRUNON*           |   *int*         |   Option for hillslope runon                       |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTRESERVOIR*       |   *int*         |   Option for reservoir routing                     |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OPTPERCOLATION*     |   *int*         |   Option for channel percolation losses            |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *INPUTDATAFILE*      | *base pathname* |   Input file base name for Mesh files              |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *INPUTTIME*          |   *int*         |   Time slice for Mesh file input                   |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *ARCINFOFILENAME*    | *base pathname* |   Input file base name for Arc files               |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *POINTFILENAME*      |   *pathname*    |   Input file name for Points files                 |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *SOILTABLENAME*      |   *pathname*    |   Soil parameter reference table                   |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *SOILMAPNAME*        |   *pathname*    |   Soil texture ASCII grid                          |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *LANDTABLENAME*      |   *pathname*    |   Land use parameter reference table               |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *LANDMAPNAME*        |   *pathname*    |   Land use ASCII grid                              |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *GWATERFILE*         |   *pathname*    |   Ground water ASCII grid                          |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *DEMFILE*            |   *pathname*    |   DEM for sheltering                               |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *RAINFILE*           | *base pathname* |   Radar Rainfall ASCII grids                       |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *RAINEXTENSION*      |  *extension*    |   Extension for Radar Rainfall grids               |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *DEPTHTOBEDROCK*     |  *meters*       |   Uniform depth to bedrock                         |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *BEDROCKFILE*        |  *pathname*     |   Bedrock depth ASCII grid                         |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *LUGRID*             |  *pathname*     |   Dynamic land cover ASCII grid list               |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *SCGRID*             |  *pathname*     |  Spatially-variable soil parameter ASCII grid list |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *HYDROMETSTATIONS*   |  *pathname*     |   Hydrometeorological station file                 |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *HYDROMETGRID*      |  *pathname*     |   Hydrometeorological ASCII grid list              |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *HYDROMETCONVERT*   |  *pathname*     |   Hydrometeorological data input file              |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *HYDROMETBASENAME*  | *base pathname* |   Hydrometeorological data file                    |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *GAUGESTATIONS*     | *pathname*      |   Rain gauge station file                          |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *GAUGECONVERT*      |  *pathname*     |   Rain gauge data input file                       |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *GAUGEBASENAME*     | *base pathname* |   Rain gauge data file                             |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *RESPOLYGONID*      |  *pathname*     |   Reservoir polygon ID file                        |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *RESDATA*           |  *pathname*     |   Reservoir data table                             |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *OUTFILENAME*       | *base pathname* |   tMesh and variable output                        |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *OUTHYDROFILENAME*  | *base pathname* |   Hydrograph output                                |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OUTHYDROEXTENSION*  |  *extension*    |   Extension for hydrographs                        |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *RIBSHYDOUTPUT*      |   *int*         |   Compatibility with RIBS Output                   |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *NODEOUTPUTLIST*     |   *pathname*    |   Node output list file                            |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *HYDRONODELIST*      |   *pathname*    |   Node runtime output list file                    |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *OUTLETNODELIST*     |   *pathname*    |   Interior node output list                        |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *FORECASTMODE*      |   *int*         |   Forecast mode options                            |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *FORECASTTIME*      |   *int*         |   Time in hours from start                         |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *FORECASTLEADTIME*  |   *int*         |   Total lead time (hrs)                            |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *FORECASTLENGTH*     |   *int*         |   Total forecast length (hrs)                      |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *FORECASTFILE*       |   *pathname*    |   Forecast file directory                          |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *CLIMATOLOGY*       |   *double*      |   Climatology rainfall (mm/hr)                     |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *RAINDISTRIBUTION*  |   *int*         |   Spatial or lumped rainfall                       |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *STOCHASTICMODE*    |   *int*         |   Stochastic model option                          |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *PMEAN*             |   *double*      |   Mean rainfall intensity (mm/hr)                  |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *STDUR*             |   *double*      |   Mean storm duration (hrs)                        |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *ISTDUR*            |   *double*      |   Mean time interval between storms (hrs)          |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *SEED*              |   *int*         |   Random seed                                      |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *PERIOD*            |   *double*      |   Period of variation (hrs)                        |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *MAXPMEAN*          |   *double*      |   Maximum value of mean rainfall intensity (mm/hr) |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *MAXSTDURMN*        |   *double*      |   Maximum value of mean storm duration (hrs)       |
            +-----------------------+-----------------+----------------------------------------------------+
            |   *MAXISTDURMN*       |   *double*      |   Maximum value of mean interstorm duration (hrs)  |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *WEATHERTABLENAME*   |   *filename*    |   Stochastic weather file name                     |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *TLINKE*             |   *double*      |   Atmospheric turbidity parameter                  |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *MINSNTEMP*          |   *double*      |   Minimum snow temperature allowed (Celsius)       |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *TEMPLAPSE*          |   *double*      |   Temperature lapse rate                           |
            +-----------------------+-----------------+----------------------------------------------------+
            |  *PRECLAPSE*          |   *double*      |   Precipitation lapse rate                         |
            +-----------------------+-----------------+----------------------------------------------------+

        The tRIBS Model Input File provides an appropriate means for summarizing the various modeling options and capabilities in the tRIBS Release. An exhaustive explanation of each item is avoided in this document for brevity and the user is referred to the more complete description tRIBS and CHILD descriptions:

        Parallel Model Inputs
        """"""""""""""""""""""""
        The use of the parallel version of the model led to the development of the following new keywords:

            Table 4.3 List of Additional Inputs for Parallel Model Runs

            .. tabularcolumns:: |l|l|l|

            +---------------------+--------------+-----------------------------------------------------+
            |   Keyword           |   Units      |   Description                                       |
            +=====================+==============+=====================================================+
            |   *PARALLELMODE*    |   *int*      |   Option to run as serial (0) or parallel (1) mode  |
            +---------------------+--------------+-----------------------------------------------------+
            |   *GRAPHOPTION*     |   *int*      |   Option for graph file type (0, 1 or 2)            |
            +---------------------+--------------+-----------------------------------------------------+
            |   *GRAPHFILE*       |   *filename* |   Reach connectivity (graph) filename               |
            +---------------------+--------------+-----------------------------------------------------+
            |   *RESTARTMODE*     |   *int*      |   Option for restart mode (0, 1, 2 or 3)            |
            +---------------------+--------------+-----------------------------------------------------+
            |   *RESTARTINTRVL*   |   *hours*    |   Time set for restart output                       |
            +---------------------+--------------+-----------------------------------------------------+
            |   *RESTARTDIR*      |   *pathname* |   Path of directory for restart output              |
            +---------------------+--------------+-----------------------------------------------------+
            |   *RESTARTFILE*     |   *filename* |   Filename of restart file                          |
            +---------------------+--------------+-----------------------------------------------------+
            |   *OPTVIZ*          |   *int*      |   Option to write viz binary files                  |
            +---------------------+--------------+-----------------------------------------------------+
            |   *OUTVIZFILENAME*  |   *filename* |   Filename for viz binary files                     |
            +---------------------+--------------+-----------------------------------------------------+


            Model Run Parameters: Restart Mode
            """""""""""""""""""""""""""""""""""

            The tRIBS model can now output binary files corresponding to the entire set of model states at a particular time interval. This capability was created to facilitate model runs beginning in the middle of a simulation. This may be necessary for recovering from a system crash and can be very useful in data assimilation and forecasting schemes. The restart mechanism is invoked by using the *RESTARTMODE* keyword which has four options: No restart mechanism (option 0), Write files only (option 1), Read files only (option 2), Read and write files (option 3). In this context, writing implies making model output states at a specified interval defined by the keyword *RESTARTINTRVL* (in hours); while reading implies using a previously generated restart file as your initial state. The restart output is written to a directory specified by the keyword *RESTARTDIR* (pathname of directory); while the restart reading is from a file specified by the keyword *RESTARTFILE* (filename). The restart mechanism should be utilized with caution with respect to file space as the restart files can be large.

            Model Run Parameters: Parallel Mode
            """""""""""""""""""""""""""""""""""""

            The tRIBS model can now be run in either serial or parallel mode, depending on user selection. The keyword *PARALLELMODE* is used to specify either serial (option 0) or parallel (option 1) computation. If the parallel mode is used, then attention needs to be paid to the graph file partitioning option. Three methods for graph partitioning can be selected utilizing the keyword *GRAPHOPTION*: (a) A default partitioning of the graph (option 0); (b) A reach-based partitioning (option 1); and (c) An inlet/outlet-based partitioning (option 2). If either option 1 or 2 are selected, the keyword *GRAPHFILE* needs to be specified with the name of the graph file to be used (either reach or inlet/outlet based). Otherwise, no filename is required.

            Model Run Parameters: Visualization Mode
            """""""""""""""""""""""""""""""""""""""""

            The tRIBS model can now save model output in binary format for visualization purposes (using ParaView and Ensight). The keyword *OPTVIZ* is used to specify either no writing of visualization output (option 0) or writing of binary output files (option 1). If the visualization output is used, *OUTVIZFILENAME* is used to specify the name of the binary file to be written.

            Model Run Parameters: Time Variables
            """"""""""""""""""""""""""""""""""""""

            The first ten item codes or parameters in the tRIBS Model Input File are related to the various timesteps used within the model.  The *STARTDATE* keyword is used to indicate the starting time of the model simulation in the following format: ``MM/DD/YYYY/HH/MM`` (Month/Day/Year/Hour/Minutes). Values of the rainfall and meteorological inputs must exist for this starting date for the model to execute properly. The *RUNTIME* keyword is used to specify the number of hours in the total length of the simulation. Similarly, there must be hydrometeorologic data that span the period between the start date and the end date. The *TIMESTEP* parameter is used to specify the Unsaturated Zone computational time step in minutes. For proper execution, the unsaturated time step should be on the order of minutes. The *GWSTEP* represents the groundwater or Saturated Zone computational time step, also in minutes. Typically, the groundwater time step can be on the order of tens of minutes for most applications. *METSTEP* specifies the time step of hydrometeorological data input from weather stations, in minutes. This time step is usually set to 60 minutes since the weather parameters are available at this temporal resolution. *ETISTEP* specifies the interval for the evapotranspiration, interception and snow model calculations in hours. The *RAININTRVL* keyword is used for the input time interval of rainfall data, either from radar rainfall grids or from raingauges, in hours. This interval will depend on the resolution of the radar data which is available from 15 minute up to daily intervals. *OPTINTVL* notifies the model at what interval the model output will be produced. Finally, the last two parameters are related to the rainfall scheme. *INTSTORMMAX* is the amount of hours without rainfall that the model considers to be sufficient for an interstorm period to begin, while *RAINSEARCH* is the amount of hours that the model will search for the next rainfall file without producing an error message and exiting the program.

            Model Run Parameters: Routing Variables
            """"""""""""""""""""""""""""""""""""""""""

            The tRIBS currently possesses a simplified hydrologic routing scheme inherited from the raster-based RIBS model (Garrote and Bras, 1995) for hillslope routing and a finite-element channel routing scheme. The model allows for non-linear routing based on the discharge at a single watershed outlet and two parameter values, the stream velocity and the hillslope velocity, shared by all TIN nodes of that particular type. The hydrologic routing scheme utilizes the discharge at the closest stream node to determine the hillslope velocity. Six routing parameters are specified to the model: *BASEFLOW*, *VELOCITYCOEF*, *FLOWEXP*, *VELOCITYRATIO*, *CHANNELROUGHNESS*, and *CHANNELWIDTH*. The first of these is used to specify the minimum flow in the stream network in cubic meters per second, a required parameter since the flow network velocities depend on the outlet discharge in some linear or nonlinear fashion. If the *BASEFLOW* parameter is not specified, a value of 0.001 cubic meters per second is assigned as default. *VELOCITYCOEF* is used to specify the coefficient in the relationship between the stream velocity and the outlet discharge, while the *FLOWEXP* is the exponent on the discharge in this relationship. Specifying *FLOWEXP = 0* implies a linear relationship between the stream velocity and the outlet discharge. The *VELOCITYRATIO* keyword is the ratio between the calculated stream velocity and the hillslope velocity assigned to non-stream nodes. The last two parameters: *CHANNELROUGHNESS* and *CHANNELWIDTH* are both uniform parameters for the entire stream network in this model version. The roughness parameters refers to a non-dimensional Manning's coefficient while the width is a channel width in meters.

            Model Run Options
            """"""""""""""""""

            A major section in the tRIBS Model Input File is dedicated to the model run options used to specify which hydrological processes are chosen for a particular model run. A brief discussion of each model run option is presented here for the sake of brevity. More details concerning the hydrologic processes themselves are available from other tRIBS documentation sources.

            The *OPTMESHINPUT* keyword is used to indicate the option for inputting the topographic data into the model. It controls the sort of mesh data that is read by the model and necessary input data files related to the model mesh. Seven options currently are implemented within the tRIBS Model: 1 = tMesh files from a prior run are used to recreate the mesh (``*.nodes``, ``*.edges``, ``*.tri``, ``*.z``); 2 = Point file used to create a new mesh (``*.points``); 3 = Arc/Info Grid file is read and sampled randomly to create the mesh; 4 = Arc/Info Grid file is read and sampled hexagonally to create the mesh; 5 = Arc/Info Ungenerated TIN file used to create a Points File (``*.net``); 6 = Arc/Info Ungenerated TIN files used to create a Points File (``*.pnt`` and ``*.lin``); 7 = Mesh constructed from scratch; 8 = Point File used with Tipper Triangulation procedure (``*.points``); 9 = Meshbuilder routines to deal with very large TIN domains (>200,000 to 10s of millions of nodes). The Meshbuilder is a separate executable that operates with an input file (``*.in``) and a points file (``*.points``). It is available for distribution through the code repository and should be compiled separately (a short readme describes its usage).

            The *RAINSOURCE* keyword is used to indicate the rainfall data source given to the model. Two types of radar rainfall data, as well as raingauge measurements are considered in the tRIBS Model: 1 = NEXRAD Stage III Radar (*cm/hr*); 2 = WSI Precipitation Radar (*mm/hr*); 3 = Rain Gauge station data (*mm/hr*).

             The *OPTEVAPOTRANS* parameter indicates the evapotranspiration option selected during the model run. The choice of the particular option will set the required parameter values used from the land use reclassification table and the meteorological data file. Five options are available for evapotranspiration: 0 = Inactive; 1 = Penman-Monteith method; 2 = Deardorff method; 3 = Priestley-Taylor method; 4 = Pan Evaporation measurements. The *OPTINTERCEPT* option allows the user to choose between three particular interception routines: 0 = Inactive, 1 = Canopy storage method; 2 = Canopy water balance method. The choice of the particular option will set the required parameter values used from the land use reclassification table. The *GFLUXOPTION* keyword allows two types of ground heat flux calculations to be performed: 0 = Inactive; 1 = Temperature gradient method, 2 = Force-restore method. The choice of the particular option will set the required parameter values used from the soil reclassification table.

             The *OPTSNOW* parameter indicates the snow pack option used. Currently, either the single-layer energy balance module is on (*OPTSNOW = 1*) or off (*OPTSNOW = 0*). With the single-layer EB model, it has been found necessary to also input a minimum allowable temperature in Celsius (*MINSNTEMP*) in order to allow numerical stability. *OPTRADSHELT*  tells what radiation sheltering scheme is used: 0 = local; 1 = remote controls on diffuse shortwave radiation; 2 = remote controls on entire shortwave radiation; 3 = no sheltering.

             The *OPTLANDUSE* parameter is used to indicate if static or dynamic land cover maps will be used in the simulation. Two options exist: *OPTLANDUSE = 0* (static representation read in at the initial time period) and *OPTLANDUSE = 1* (dynamic updating of the land cover at times specified by the available grids). If the dynamic updating is specified, then the user must indicate the pathname of the file containing the filenames of the ASCII grids to be read. This is specified using the keyword LUGRID (pathname to a Grid Data File, ``*.gdf``). This file should contain the pathnames to the dynamic land cover grids. File naming convention only uses up to the hourly time stamp (no minutes, for example). The files need to be within the time boundaries of the simulation period. The keyword *OPTLUINTERP* allows for two types of interpolations between available land cover maps (at different time periods). *OPTLUINTERP = 0* assigns the current gridded time step value to all model time steps up until the next available file. *OPTLUINTERP = 1* linearly interpolates the land cover parameter values between two different grid time steps.

             The *OPTSOILTYPE* keyword is used to activate the use of gridded soil parameter data input into the model. This option replaces the use of a soil grid index map and a soil parameter table. Two options exist: *OPTSOILTYPE = 0*, uses the traditional tabular soil data associated with a soil map of soil type numbers; *OPTSOILTYPE =1*, activates the use of gridded soil data, a new functionality. If *OPTSOILTYPE = 1* then an additional folder named *SoilTexture* must be created in the main tRIBS directory where folders like *Input* and *Output* are located. This new folder should contain a database (``*.gdf``) file indicating the paths to all the grid files for each soil parameter. The format is similar to that used for the dynamic land cover maps. The directory path to the new folder is indicated under *SCGRID* keyword in the Input File.

             The *METDATAOPTION* is used to indicated the input format for the meteorological data: 0 = Inactive; 1 = Weather station point data; 2 = Grid meteorological data. The particular choice determines which type of text files, grid or point data files are required during model execution. The *CONVERTDATA* option is used to indicate whether or not meteorological pre-processing is activated: 0 = Inactive pre-processing; 1 = Activated pre-processing of meteorological data from RFC Point Data; 2 = Activated pre-processing of meteorological data from gridded observations provided by University of Washington (DMIP).

             The *OPTBEDROCK* keyword is used to specify the format of the bedrock depth data: 0 = Uniform bedrock depth over the basin; 1 = Grid bedrock file. If *OPTBEDROCK = 0*, then the *DEPTHTOBEDROCK* keyword is required (input is a double), otherwise the *BEDROCKFILE* keyword is required (input is a path and filename with extension ``*.brd``).

             The *OPTGWFILE* keyword is used to specify the format of the initial groundwater input file. 0 = Resample ASCII grid file indicated in *GWATERFILE*; 1 = Read in Voronoi polygon file with groundwater levels output from previous run. *GWATERFILE* keyword only used for *OPTGWFILE* option 0, otherwise, the Voronoi GW file is read in through user interaction with model run (e.g. through screen).

             The *WIDTHINTERPOLATION* keyword is used to specify whether or not channel widths will be interpolated between the measured and observed widths (= 0) or only between the measured channel widths (= 1), inputted to the model through the file name specified using the keyword *CHANNELWIDTHFILE*.

             The *STOCHASTICMODE* keyword is used to specify whether or not stochastic rainfall forcing is used as an alternative to providing observed data from radar (grid field) or rain gauge (point). The stochastic mode is off (= 0) or on in various ways: Mean forcing (= 1), random forcing (= 2), sinusoidal forcing (= 3), mean and sinusoidal forcing (= 4) and random and sinusoidal forcing (= 5). The parameters of the stochastic mode include a random seed, a periodicity, a mean/max storm duration, a mean/max interstorm duration, a mean/max rainfall intensity. A complete stochastic weather generator for all climatic variables can also be utilized by specifying *STOCHASTICMODE = 6* and a filename for *WEATHERTABLENAME*.

             The *OPTRESERVOIR* keyword is used to activate the use of the linear reservoir module (``tReservoir``) in the model. 0 = Disable the use of Reservoirs. 1 = Activate the use of Reservoirs. If *OPTRESERVOIR = 1* then additional information is required by specifying the path to the file containing the TIN nodes (or Voronoi polygons) to be used as reservoirs in *RESPOLYGONID* (``*.res``) and the path to the file containing the elevation-discharge-storage information for each type of reservoir in *RESDATA* (``*.eds``).

             The *OPTPERCOLATION* keyword allows the user to select from several options for channel percolation losses. 0 = No channel percolation. 1 = Constant loss method where the infiltration rate is equal to the channel saturated hydraulic conductivity specified under *CHANNELCONDUCTIVITY*. 2 = Constant loss method with a transient period applied with the transient hydraulic conductivity specified as *TRANSIENTCONDUCTIVITY* and the transient time period specified as *TRANSIENTTIME*. 3 = Green-Ampt infiltration equation with the parameters specified as *CHANNELPOROSITY*, *CHANPOREINDEX* and *CHANPSIB*.

             Model Input Files and Pathnames: Mesh Generation
             """"""""""""""""""""""""""""""""""""""""""""""""""

             When specifying the *OPTMESHINPUT* option, the model will require that the pathname of the input files be included within the Mesh Generation section of the Model Input File. The *INPUTDATAFILE* option is used to input the basename for the Mesh input files produced during a previous run (*OPTMESHINPUT = 1*), while the *INPUTTIME* keyword specifies the time slice for mesh input (for tRIBS, *INPUTTIME* should always be set to zero). If using *OPTMESHINPUT = 2*, then the *POINTFILENAME* keyword must be used to specify the pathname and filename of the Points File (``*.points``). If using *OPTMESHINPUT = 2* through *6*, then the keyword *ARCINFOFILENAME* specifies the pathname and basename for the Arc/Info grids or output files (``*.asc``, ``*.net``, ``*.lin``, ``*.pnt``).

             Mesh Input Files and Pathnames: Resampling Grids
             """""""""""""""""""""""""""""""""""""""""""""""""""

             The path and filenames of the grid input and the reclassification tables for the soil and land use data are grouped together within this section of the Model Input File. The soil grid (``*.soi``), land use grid (``*.lan``) and initial groundwater table position (``*.iwt``) are specified using the *SOILMAPNAME*, *LANDMAPNAME* and *GWATERFILE* keywords. The DEM used to derive the remote sheltering grids is specified by the *DEMFILE* keyword. The DEM used should encompass the study area and all significant surrounding topographic features, possibly outside the study area. The radar rainfall grid (for *RAINSOURCE = 1* or *2*) base name is specified using the *RAINFILE* keyword and the extension is inputted by using the *RAINEXTENSION* keyword.

             Mesh Input Files and Pathnames: Meteorological Data
             """"""""""""""""""""""""""""""""""""""""""""""""""""""

             The path and filenames of the meteorological data are grouped together in this section of the Model Input File. If *METDATAOPTION = 1*, then the Station Data File (``*.sdf``) must be specified in *HYDROMETSTATIONS* and the Meteorological Data File (``*.mdf``) basename in *HYDROMETBASENAME*. Otherwise, if *METDATAOPTION = 2*, then the *HYDROMETGRID* keyword must contain the Grid Data File (``*.gdf``). If *CONVERTDATA = 1*, then the *HYDROMETCONVERT* parameter must specify the path and filename of the Meteorological Data Input (``*.mdi``) File.  If *CONVERTDATA = 2*, then the *GAUGECONVERT* parameter must be specify the path and filename of the rain gauge conversion file (``*.mdi``). The *GAUGEBASENAME* keyword is used to specify the base pathname of the MDF raingauge files. Finally, if *RAINSOURCE = 3*, then the *GAUGESTATIONS* keyword is used to specify the rain gauge SDF file. If *CONVERTDATA = 3*, then preprocessing of DMIP formatted observed energy forcings is performed. This results in an MDF file particular to the basin of interest (1992-2000 period) with somewhat altered list of meteorological parameters that can be ingested into the model. A separate SDF file must be prepared to correspond with this data.

             Lapse rates have been implemented in the model for precipitation and temperature. The temperature lapse rate is assigned from *TEMPLAPSERATE*. The precipitation lapse rate is specified by *PRECLAPSE* in *mm/m*. Scattered light from opposing hillslopes can be a significant component of incoming radiations in snowy environments. *HILLALBOPT = 0* uses the snow albedo for the hillslope albedo, *HILLALBOPT = 1* uses the land-use albedo for the hillslope albedo, and *HILLALBOPT = 2* uses a dynamic representation of albedo, where the snow albedo is used if there is snow in the canopy and a vegetative fraction weighted average of snow and land-use albedo is used otherwise.

             Mesh Input Files and Pathnames: Output Data
             """""""""""""""""""""""""""""""""""""""""""""

             The path and basenames of the output data are grouped in this section of the Model Input File. The keyword *OUTFILENAME* is used to specify the location and basename of the output mesh and the voronoi file (``*.nodes``, ``*.tri``, ``*.edges``, ``*.z`` and ``*.voi``) as well as the dynamic variable output (``*.pixel`` and ``*.dat``). The keyword *OUTHYDROFILENAME* specifies the path and basename of the outputted hydrograph and hyetograph time series. The format of the hydrograph and hyetograph file (``*.mdf``) depends on the value of *RIBSHYDOUTPUT*: *= 0*, not compatible with RIBS output; or *= 1*, compatible with RIBS output. This distinction is necessary if the ``*.mrf`` files are to be used with the RIBS graphical user interface. The *NODEOUTPUTLIST* specifies the path and filename of the Node Output List (``*.nol``) file used to input the node IDs for dynamic variable output. The *OUTLETNODELIST* keyword specifies the interior stream nodes to be used for output of the interior hydrographs (``*.nol``) file.

             Model Modes: Rainfall Forecasting Mode
             """""""""""""""""""""""""""""""""""""""""

             The tRIBS model can be used for real-time flood forecasting given predicted rainfall data from any number of sources (radar extrapolation, numerical weather prediction). Currently, the Rainfall Forecasting Mode allows the user to specify the forecast time, lead time and forecasting interval using the *FORECASTTIME*, *FORECASTLEADTIME* and *FORECASTLENGTH* keywords. The Forecasting mode is turned on using *FORECASTMODE*. Three options are available: Single or Updating forecast, Persistence Forecast or Climatological Forecasting. They differ in the product used after the forecast time. For single or updating, the *FORECASTFILE* directory is read for the forecast product. Otherwise, a persistence of the last available rainfall or the climatological value are used. The *RAINDISTRIBUTION* enables the inputted rainfall to be spatially-averaged within tRIBS.

             Model Modes: Stochastic Rainfall Mode
             """"""""""""""""""""""""""""""""""""""""

             The tRIBS model can be forced with real rainfall data or stochastic rainfall input using the Eagleson or Rodriguez-Iturbe type Poisson storm process at a point. The Stochastic Rainfall Mode is invoked with the keyword *STOCHASTICMODE* specified as other than zero. The keywords *PMEAN*, *STDUR*, *ISTDUR* are used alone (option 1: mean forcing), in conjunction with the random seed *SEED* (option 2: random forcing), in conjunction with periodic forcing using the *PERIOD*, *MAXPMEAN*, *MAXSTDURMN* and *MAXISTDURMN* (option 3: sinusoidal forcing), in combination of both mean and sinusoidal (option 4: mean and sinusoidal forcing) or in combination of both mean and random forcing (option 5: mean and random forcing). The user should carefully review implications of selection with the ``tStorm`` class definition. A complete stochastic weather generator (Ivanov, 2004) for all climatic variables can also be utilized by specifying *STOCHASTICMODE = 6* and a filename for *WEATHERTABLENAME*. See Ivanov (2004) chapter on stochastic climate forcing for more details.

             4.4.2 Soil and Land Use Input
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

             The description of the land surface characteristics in the tRIBS Model is achieved through the input of soil textural and land use/land cover data in the form of ASCII grids of a particular soil or land use code. The soil (``*.soi``) and land use (``*.lan``) grids are specified in the Model Input File by using the keywords *SOILMAPNAME* and *LANDMAPNAME*, respectively. **Figure 4.3** presents an example of a soil or land use grid similar to the rainfall ASCII grid presented in **Figure 3.1** for the Peacheater Creek basin at a resolution of 2 kilometers by 2 kilometers. As with other grid input, care should be taken to specify the grids in the same coordinate system as the topographic TIN data. The resampling routines included in the tResample class are designed to read the land use and soil cover grids and assign the appropriate codes to the TIN mesh nodes according to the geographic overlap of the two coverages. The geographic assignment is performed in one of two possible fashions, depending on the relative size of the grid input as compared to the Voronoi cell scale. For large input grids, such as those available from radar rainfall input, the resampling is performed by a nearest neighbor approach. For grid inputs at the scale of the Voronoi polygons, an aerial weighting is used to determine the dominant cover type. Further details are available elsewhere in the tRIBS documentation

                  Figure 4.3 Example of Soil or Land Use Class ASCII grid (``*.soi`` and ``*.lan``)

                  .. tabularcolumns:: |l|l|l|l|l|l|

                  +----------------+---------+---------+---------+---------+---------+
                  |  nrows         |    6    |                                       |
                  +----------------+---------+---------+---------+---------+---------+
                  |  ncols         |    6    |                                       |
                  +----------------+---------+---------+---------+---------+---------+
                  |  xllcorner     | 346035  |                                       |
                  +----------------+---------+---------+---------+---------+---------+
                  |  yllcorner     | 3979905 |                                       |
                  +----------------+---------+---------+---------+---------+---------+
                  |  cellsize      |   2000  |                                       |
                  +----------------+---------+---------+---------+---------+---------+
                  |  NODATA_value  |  -9999  |                                       |
                  +----------------+---------+---------+---------+---------+---------+
                  |  -9999         |  -9999  |  -9999  |   1     |    0    |  -9999  |
                  +----------------+---------+---------+---------+---------+---------+
                  |  -9999         |  -9999  |    1    |    0    |    0    |  -9999  |
                  +----------------+---------+---------+---------+---------+---------+
                  |  -9999         |  -9999  |    1    |    0    |    1    |    1    |
                  +----------------+---------+---------+---------+---------+---------+
                  |  -9999         |    0    |    1    |    1    |  -9999  |  -9999  |
                  +----------------+---------+---------+---------+---------+---------+
                  |    0           |    0    |  -9999  |  -9999  |  -9999  |  -9999  |
                  +----------------+---------+---------+---------+---------+---------+


            Once the land use and soil class codes are assigned to each Voronoi or TIN node, a set of procedures in tInvariant take care of constructing objects for each cover type. These cover type objects are referenced by the codes in the input grids and include references to the various parameter values associated with each cover type. The parameter values for the soil and land use grids are read from a reclassification table inputted separately into the model by using the keywords *SOILTABLENAME* and *LANDTABLENAME*. The format of these soil reclassification (``*.sdt``) and land use reclassification (``*.ldt``) tables are straightforward. They include a small header which specifies the number of cover types (*#Types*) and the number of variables for each type (*#Params*), as shown in **Figure 4.4a** and **Figure 4.5a**. The header is followed by a matrix of parameter values where each row represents one cover type and each column represents one parameter. Currently, there **must** be 12 parameters in the soil and land use reclassification tables. Without this appropriate number of parameters in the file, erroneous calculations may take place. In addition, the order and units of these parameters are **fixed**. Since parameter values outside the appropriate range may results in inaccurate calculations, the user should be careful to select realistic values from literature sources prior to model use.

                  **Figure 4.4a** Soil Reclassification Table Structure (``*.sdt``)

            


------------------------------------------------
          *Last Update: 02-08-2021  C. Lizarraga
