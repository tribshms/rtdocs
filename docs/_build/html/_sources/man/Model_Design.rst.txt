
Model Design
=================

    The software design of the tRIBS Model is based on object-oriented C++ programming. The model classes support and use various object oriented methods including inheritance, polymorphism and virtual functions. In addition, the use of linked list and class templates is particularly important within the tRIBS code. As an object-oriented code, tRIBS constructs as set of objects that encapsulate variables and functions and declares their accessibility to other model objects. These objects are then used to carry out the various modeling functions in the hydrologic simulation. In order to best describe the software architecture of the model, it is important to first understand the file structure. **Tables 2.1** and **2.2** list the directories and files that form part of tRIBS. For the new user, this is a starting point to begin to form a mental picture of how the model operates. After knowing the various class names, the user should inspect the Class Diagrams to understand the variables and functions within each class. Class Diagrams summarize the class properties in a visual format without recurring to the actual code. Work Flow Diagrams present how the model procedures flow from one to another, thus allowing a new user to grasp how various objects depend upon and call other objects. Again, these diagrams are tools for quick inspection and a more detailed understanding requires interpretation of the actual code. Since the total code for tRIBS includes approximately 60,000 lines of code within the various files, these tools for quick inspection should be very useful for the new tRIBS user. Note that some new classes have been added to the tRIBS model to handle parallelization routines.


Model File Structure
--------------------------

    The tRIBS Model is organized into a single directory (called ``tRIBS``) with various sub directories that contain the model C++ classes. Each sub directory encapsulates classes with similar functionality or behavior. **Table 2.1** demonstrates the various sub directories found within the tRIBS directory, as a user would see upon downloading the source code. Various of these sub directories deal specifically with the hydrologic processes (``tHydro``, ``tFlowNet``, ``tRasTin``), others create the mesh architecture (``tMesh``, ``tMeshElements``, ``tMeshList``), while others are general purpose classes used for model execution (``tSimulator``, ``tInOut``, ``tCNode``) or within other classes (``tArray``, ``tList``, ``tPtrList``).  The ``Headers`` and ``Mathutil`` directories contain global header files and mathematical utilities for the model, respectively. Two subdirectories have been added for parallelization (``tGraph``, ``tParallel``).

        **Table 2.1** tRIBS Model Subdirectories

        .. tabularcolumns:: |c|c|c|

        +--------------------+--------------------+--------------------+
        |  Headers           |  tInOut            |  tMeshList         |
        +--------------------+--------------------+--------------------+
        |  Mathutil          |  tList             |  tPtrList          |
        +--------------------+--------------------+--------------------+
        |  Utilities         |  tListInputData    |  tRasTin           |
        +--------------------+--------------------+--------------------+
        |  tArray            |  tMesh             |  tSimulator        |
        +--------------------+--------------------+--------------------+
        |  tCNode            |  tMeshElements     |  tStorm            |
        +--------------------+--------------------+--------------------+
        |  tHydro            |  tFlowNet          |  tGraph            |
        +--------------------+--------------------+--------------------+
        |  tParallel         |                    |                    |
        +--------------------+--------------------+--------------------+


    In addition to the sub directories, the ``tRIBS`` directory contains a main function (``main.cpp``) and a makefile for each particular UNIX platform (``makeSUN``, ``makeLINUX``, ``makeG5``, ``makeIBM``, ``makeSGI``, ``makeALPHA``, ``makeLAMPI``, ``makeOPENMPI``) and makefiles for the parallel model compilation (``makeLINUX_PAR``, ``makeMAC_PAR``). Running the make file properly will create a directory to store the object files for each class (``*.o``) and the platform-specific executable (called ``tribs``). Each sub directory will include the C++ class files (``*.cpp`` used as convention) and the C++ Header Files (``*.h``). The reader is referred to various textbooks on C++ programming for more information on the structure for these files (Deitel and Deitel, 2001, Lippman and Lajole, 1998) [Deitel_Deitel_2001]_, [Lippman_Lajoie_1998]_ .  **Table 2.2** shows a list of the code files in the tRIBS model for further reference.


        **Table 2.2** tRIBS Model Class and Header Files

        .. tabularcolumns:: |c|l|

        +--------------------+-------------------------------------------------------------------+
        |  tRIBS             |  main.cpp, makefile                                               |
        +--------------------+-------------------------------------------------------------------+
        |  /Headers          |  Classes.h, Definitions.h, Inclusions.h, globalFns.h,             |
        +--------------------+-------------------------------------------------------------------+
        |                    |  globalFns.cpp, TemplDefinitions.h, tribs_os.h,                   |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tribs_os_ALPHA64.h, tribs_os_LINUX32.h                           |
        +--------------------+-------------------------------------------------------------------+
        |  /Mathutil         |  geometry.h , mathutil.h, mathutil.cpp, predicates.h,             |
        +--------------------+-------------------------------------------------------------------+
        |                    |  predicates.cpp                                                   |
        +--------------------+-------------------------------------------------------------------+
        |  /Utilities        |  InitialGW.cpp, RunTracker.cpp, RainInputCheck.cpp                |
        +--------------------+-------------------------------------------------------------------+
        |  /tArray           |  tArray.h, tArray.cpp, tMatrix.h, tMatrix.cpp                     |
        +--------------------+-------------------------------------------------------------------+
        |  /tCNode           |  tCNode.h, tCNode.cpp                                             |
        +--------------------+-------------------------------------------------------------------+
        |  /tFlowNet         |  tFlowNet.h, tFlowNet.cpp, tFlowResults.h, tFlowResults.cpp,      |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tKinemat.h, tKinemat.cpp, tReservoir.cpp, tReservoir.h,          |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tResData.cpp, tResData.h                                         |
        +--------------------+-------------------------------------------------------------------+
        |  /tGraph           |  tGraph.h, tGraph.cpp, tGraphNode.h, tGraphNode.cpp               |
        +--------------------+-------------------------------------------------------------------+
        |  /tHydro           |  tEvapoTrans.h, tEvapoTrans.cpp, tHydroMet.h, tHydroMet.cpp,      |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tHydroMetConvert.h, tHydroMetConvert.cpp, tHydroMetStoch.h,      |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tHydroMetStoch.cpp tHydroModel.h, tHydroModel.cpp,               |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tIntercept.h, tIntercept.cpp, tWaterBalance.h, tWaterBalance.cpp,|
        +--------------------+-------------------------------------------------------------------+
        |                    |  tSnowPack.h, tSnowPack.cpp,                                      |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tSnowIntercept.h, tSnowIntercept.cpp                             |
        +--------------------+-------------------------------------------------------------------+
        |  /tInOut           |  tInputFile.h, tInputFile.cpp, tOutput.h, tOutput.cpp,            |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tOstream.h, tOstream.h                                           |
        +--------------------+-------------------------------------------------------------------+
        |  /tList            |  tList.h, tList.cpp                                               |
        +--------------------+-------------------------------------------------------------------+
        |  /tListInputData   |  tListInputData.h, tListInputData.cpp                             |
        +--------------------+-------------------------------------------------------------------+
        |  /tMesh            |  tMesh.h, tMesh.cpp, tTriangulator.h, tTriangulator.cpp,          |
        |                    |  heapsort.h                                                       |
        +--------------------+-------------------------------------------------------------------+
        |  /tMeshElements    |  meshElements.h, meshElements.cpp                                 |
        +--------------------+-------------------------------------------------------------------+
        |  /tMeshList        |  tMeshList.h, tMeshList.cpp                                       |
        +--------------------+-------------------------------------------------------------------+
        |  /tParallel        |  tTimer.h, tTimer.cpp, tTimings.h, tTimings.cpp, tParallel.h,     |
        |                    |  tParallel.cpp                                                    |
        +--------------------+-------------------------------------------------------------------+
        |  /tPtrList         |  tPtrList.h, tPtrList.cpp                                         |
        +--------------------+-------------------------------------------------------------------+
        |  /tRasTin          |  tInvariant.h, tInvariant.cpp, tRainfall.h, tRainfall.cpp,        |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tResample.h, tResample.cpp, tVariant.h, tVariant.cpp,            |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tRainGauge.h, tRainGauge.cpp,                                    |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tShelter.h, tShelter.cpp                                         |
        +--------------------+-------------------------------------------------------------------+
        |  /tSimulator       |  tRunTimer.h, tRunTimer.cpp, tSimul.h, tRestart.h, tRestart.cpp,  |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tSimul.cpp, tControl.h, tControl.cpp, tPreProcess.cpp,           |
        +--------------------+-------------------------------------------------------------------+
        |                    |  tPreProcess.h                                                    |
        +--------------------+-------------------------------------------------------------------+
        |  /tStorm           |  tStorm.h, tStorm.cpp                                             |
        +--------------------+-------------------------------------------------------------------+


    The class names are indicative of the functionality for that particular class. Most files contain a single class that encapsulate the data and functions operating on the data within a single object. In some occasions, it has been convenient to include several interrelated classes within the same file. A list of all non-derived tRIBS Classes can be found in ``tRIBS/Headers/Classes.h``. The main function is exclusively used in tRIBS to construct the various objects, while the simulation control itself is performed by the SimulationControl class. Further details on the classes and the flow of data in the tRIBS model are presented in concise, graphical format using diagrams.


Model Class Diagrams
-------------------------

    Model class diagrams are a useful tool for summarizing the class properties, in terms of variables and functions, in a visual format without recurring to the actual code. Function and variable declarations are presented as they are implemented within the code, including knowledge of the accessibility of each object property and the use of other model objects. For the tRIBS model, the UML (Universal Modeling Language) has been used to create class diagrams through Microsoft Visio, part of the Microsoft Visual Studio development framework. The UML format is a standard diagramming language used by software engineers and architects to document model code. **Table 2.3** presents a list of the model classes and references to the class diagram for each.

        **Table 2.3** tRIBS Class Diagrams

        .. tabularcolumns:: |c|c|c|c|

        +------------------------+------------------------+------------------------+------------------------+
        |**Templated**           |**Control and Storage** |**Hydrological**        |                        |
        |**Classes**             |**Classes**             |**Classes**             |                        |
        +========================+========================+========================+========================+
        |  tMesh                 |  tTriangle             |  tHydroModel           |  SoilType              |
        +------------------------+------------------------+------------------------+------------------------+
        |  tMeshList             |  tNode                 |  tEvapoTrans           |  GenericSoilData       |
        +------------------------+------------------------+------------------------+------------------------+
        |  tMeshListIter         |  tEdge                 |  tIntercept            |  tStorm                |
        +------------------------+------------------------+------------------------+------------------------+
        |  tList                 |  tCNode                |  tRainfall             |  tHydroMetStoch        |
        +------------------------+------------------------+------------------------+------------------------+
        |  tListNode             |  Point2D               |  tRainGauge            |  tSnowPack             |
        +------------------------+------------------------+------------------------+------------------------+
        |  tListIter             |  Point3D               |  tHydroMet             |  tSnowIntercept        |
        +------------------------+------------------------+------------------------+------------------------+
        |  tPtrList              |  vcell                 |  tHydroMetConvert      |  tShelter              |
        +------------------------+------------------------+------------------------+------------------------+
        |  tPtrListNode          |  Predicates            |  tResample             |  tResData              |
        +------------------------+------------------------+------------------------+------------------------+
        |  tPtrListIter          |  Simulator             |  tVariant              |  tReservoir            |
        +------------------------+------------------------+------------------------+------------------------+
        |  tArray                |  SimulationControl     |  tFlowNet              |                        |
        +------------------------+------------------------+------------------------+------------------------+
        |  tMatrix               |  tRunTimer             |  tFlowResults          |                        |
        +------------------------+------------------------+------------------------+------------------------+
        |  tOutput               |  tPreprocess           |  tKinemat              |                        |
        +------------------------+------------------------+------------------------+------------------------+
        |  tCOutput              |  tControl              |  tWaterBalance         |                        |
        +------------------------+------------------------+------------------------+------------------------+
        |  tListInputData        |                        |  LandType              |                        |
        +------------------------+------------------------+------------------------+------------------------+
        |  tIdArray              |                        |  GenericLandData       |                        |
        +------------------------+------------------------+------------------------+------------------------+



Model Workflow Diagrams
-----------------------------

    Model workflow diagrams present the steps followed during model execution in a graphical manner that facilitates understanding of the model procedures. The workflow could be documented at various levels of complexity (at the model level, at the class level and at the function level). Here, the model level is chosen as an appropriate representation and the details of the workflow within classes or functions are not shown for brevity. The tRIBS Model Workflow Diagram presents the model procedure at the coarsest level possible. For more information, the user is referred to the ``main.cpp`` and ``tSimul.cpp`` classes which encapsulate the model execution procedures.

Computational Mesh
------------------------

    The tRIBS Model inherited the Triangulated Irregular Network (TIN) mesh architecture directly from the CHILD model framework (Tucker *et. al*, 1999) [Tucker_1999a]_ . As such, the model has the same capabilities as CHILD in constructing TIN meshes using the various options available in the ``tMesh`` class. In addition, some new input capabilities have been added that take advantage of the TIN creation capabilities of Arc/Info TIN (ESRI, 1996) [ArcInfoMethod_1996]_ . These new input capabilities e| end the mesh framework to the more complicated topography present in real world watersheds and also allow us to input "hydrologically" significant TIN terrain representations. The existing options for creating the computational mesh include:

      - Generating a synthetic rectangular mesh with random or hexagonal node arrangements.
      - Read in an existing tRIBS Mesh files from a previous run.
      - Generate a mesh from a given set of (*x* , *y* , *z*, *b*) points.
      - Generate a mesh from a Digital Elevation Model (DEM) Arc/Info ascii grid
      - Generate a set of points from an Arc/Info TIN ungenerate file (``*.net``)
      - Generate a set of points from an Arc/Info TIN ungenerate files (``*.pnt``, ``*.lin``)


    Additional details concerning the generation of the TIN input for the tRIBS Model will be discussed further in this document. It is important, however, to briefly describe the concept behind the TIN computational mesh for the two distributed hydrologic and geomorphologic models (tRIBS and CHILD). A TIN within these models can be described as a set of highly interconnected triangle objects that each possesses three edge and three node objects (as defined in ``MeshElements.cpp``). The TIN mesh allows for flow and transport from TIN node to TIN node, along a triangle edge, using a finite difference approach. Hydrologic computations made at each TIN node (e.g. infiltration, evaporation, groundwater table elevation) are assumed valid over a region consisting of the Voronoi polygon associated with the node. In this way the Voronoi polygon is used as the control volume for mass conservation in the tRIBS model. The Voronoi polygon (or Thiessen polygon) is the dual diagram of the TIN mesh and can be computed by the intersection of perpendicular bisectors to each TIN edge. Since a unique relation exists between a TIN Mesh and its Voronoi Polygon Network (VPN), it is convenient to use both representations interchangeably within the model to simulate hydrological processes. For more details, the reader is referred to Tucker *et. al* (2001) [Tucker_2001]_ .

----------------------------------------------------

    *Last update:* 02/05/2021 C. Lizarraga
