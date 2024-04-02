Model Parameters and Forcings
==============================

A tRIBS model application consists of a series of Voronoi polygons that discretely represent a watershed. At each TIN node associated with a Voronoi polyong, a set of governing equations describing the hydrologic and energy processes are computed. To solve these equations for each location, spatially distributed data describing surface topography, soils, vegetation and hydrometeorology are required, which can be obtained from field measurements, remote sensing data interpretation or model output such as numerical weather models. 

Model Parameters
------------------

In the case of soil and vegetation cover, different sources of data can be used to assign uniform or spatially-explicit parameter values required for the governing equations. The soils parameters listed in **Table 3.1** are necessary for carrying out the vertical infiltration and lateral flow redistribution, as well computing the soil heat budget within the sloped, heterogeneous, anisotropic soil columns assumed at each Voronoi polygon. Note that the parameter requirements vary with the specified hydrologic processes selected during a model run. For example, the surface heat parameters are only required if the radiation balance is computed.

        **Table 3.1** tRIBS Model Subdirectories

        .. tabularcolumns:: |c|c|c|

        +--------------------+--------------------+--------------------+
        |  Headers           |  tInOut            |  tMeshList         |
        +--------------------+--------------------+--------------------+
        |  Mathutil          |  tList             |  tPtrList          |
        +--------------------+--------------------+--------------------+
        |  utilities         |  tListInputData    |  tRasTin           |
        +--------------------+--------------------+--------------------+
        |  tArray            |  tMesh             |  tSimulator        |
        +--------------------+--------------------+--------------------+
        |  tCNode            |  tMeshElements     |  tStorm            |
        +--------------------+--------------------+--------------------+
        |  tHydro            |  tFlowNet          |  tGraph            |
        +--------------------+--------------------+--------------------+
        |  tParallel         |                    |                    |
        +--------------------+--------------------+--------------------+


Model Forcings
----------------
