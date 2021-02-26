Model Input Formats
========================

  .. figure::  ../images/tRIBSFlowchart.jpg
      :align:   center


      A schematic of the tRIBS Model Framework illustrates the various components and input types within the model. The tRIBS model is designed to accept input from various types of data formats: grid data, TIN data, point data and text tables. The grid data supplied to the model can be time-invariant (soils and land use) or time-varying (rainfall or weather) grids. The TIN data are inputted into the model in a variety of methods that represent the nodes within the mesh or represent the topography that is intended to be modeled. The method chosen to input the TIN data depends upon the particular application. The point data represent the values of time-varying parameters, such as meteorological data, that are available at specified points within the watershed. Resampling routines are available for geographically overlaying the grid or point data onto the Voronoi polygon mesh. Finally, text tables are used within the model for inputting parameter values associated with the soil and land use maps or the hydrometeorological data.


Grid Input
---------------

    A standard ASCII grid format is used for all grid input to the model, which may include soil and land use reclassification fields, initial groundwater table depth, depth to bedrock and radar rainfall grids. The ASCII grid format used is a standard created by ESRI for the exchange of grid data for GIS applications. It consists of a small, 6-line header that describes the matrix data presented in the text file. This format is a convenient method for data exchange into GIS since the format is easily read by all ESRI software. In other software programs for visualization of matrix data (*i.e.* MATLAB), care must be taken when reading in the header information. Any extension name for the grid data can be used within tRIBS as long the filename is specified in the Model Input File. However, for visualization of the grid within GIS, an extension ``*.asc`` is required for the grid to be read properly (ESRI, 1996).

    As mentioned previously, the grid input to the tRIBS model can consist of time-invariant watershed descriptors, such as soil and land use coverage grids, or time-varying, spatially-distributed hydrometeorologic forcings, such as radar rainfall estimates or grid meteorological data from numerical weather models. Although each grid input is treated differently within the model, the user input should be identical. It consists mostly of specifying the grid pathname within the Model Input File, as described in a later section. As illustrated in the tRIBS Model Framework, all grid input are resampled onto the TIN terrain representation created within the tRIBS model, and specifically onto the Voronoi Polygon Network (VPN).

        .. tabularcolumns:: |c|c|c|c|c|c|

        +-----------------+-----------+-----------+-----------+----------+----------+
        | ncols           | 6         |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | nrows           | 6         |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | xllcorner       | 346035    |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | yllcorner       | 3979905   |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | cellsize        | 2000      |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | NODATA_value    | -9999     |           |           |          |          |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | -9999           | -9999     | -9999     | 0.511     | 0.111    | -9999    |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | -9999           | -9999     | 0.951     | 1.873     | 0.239    | -9999    |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | -9999           | -9999     | 0.824     | 0.412     | 0.444    | 1.051    |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | -9999           | 3.123     | 0.154     | 0.853     | -9999    | -9999    |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | 1.090           | 2.541     | 0.288     | -9999     | -9999    | -9999    |
        +-----------------+-----------+-----------+-----------+----------+----------+
        | 2.241           | 0.312     | -9999     | -9999     | -9999    | -9999    |
        +-----------------+-----------+-----------+-----------+----------+----------+

        **Figure 3.1.** Example of a standard ASCII grid file.


    **Figure 3.1** illustrates an example of a grid ASCII input to the model with the appropriate header information and the matrix of values. These values may be any particular type of input that needs to be resampled onto the tRIBS mesh. In this particular case, it represents NEXRAD radar rainfall values (in *cm/hr*) at a resolution of 2 kilometer by 2 kilometers within the perimeter of the Peacheater Creek Watershed (see Sample Application in tRIBS Watershed Downloads Page). Illustrated in the figure are various features to note:

        1) the grid input must be composed of square grid cells (*i.e. dx = dy = cellsize* (in meters)),
        2) the coordinate system is specified by the values of x and y at the lower left corner (in this case, UTM zone 15 coordinates are shown),
        3) the entire grid can be rectangular, such that *nrows* and *ncols* can differ and
        4) the NODATA_value can be used to represent cells outside of the domain of interest.


TIN Input
--------------

    Topographic data is inputted into the tRIBS Model through a variety of methods that are implemented in the tMesh class.  For applications in real watersheds with complex topography and stream networks, the method of choice is to generate the TIN mesh within Arc/Info GIS and export it into a format that tRIBS can convert to a Points File (``*.points``). This Points File is then read during a subsequent model run. For tRIBS, a set of Arc/Info scripts have been created that manipulate the watershed Digital Elevation Model (DEM), the stream network, the watershed boundary and the bottom-valley floodplain to created a "hydrologically" significant TIN Terrain Model. Further details of these scripts are presented in the Terrain Analysis Section of this document. Exporting the TIN mesh from Arc/Info is performed through a procedure that ungenerates the TIN into a set of points (``*.pnt``) and lines (``*.lin``). tRIBS can read these files and construct from them a Points (``*.points``) file, an example of which is shown in **Figure 3.2**.

        .. tabularcolumns:: |c|c|c|c|

        +---------+----------+----------+----------+
        | 7       |          |          |          |
        +---------+----------+----------+----------+
        | 405     | 0        | 0        | 1        |
        +---------+----------+----------+----------+
        | 495     | 0        | 0        | 2        |
        +---------+----------+----------+----------+
        | 585     | 0        | 0        | 1        |
        +---------+----------+----------+----------+
        | 360     | 90       | 1        | 1        |
        +---------+----------+----------+----------+
        | 450     | 90       | 1        | 0        |
        +---------+----------+----------+----------+
        | 540     | 90       | 1        | 0        |
        +---------+----------+----------+----------+
        | 630     | 90       | 1        | 1        |
        +---------+----------+----------+----------+

        **Figure 3.2.** Example of a tRIBS Points File (``*.points``)

    A Points File is a simple text file that contains a listing of point coordinates (*x, y, z*) and the boundary code (*b*) for each point (in that specific order) with a small header that indicates the total number of points in the file. The boundary code is indicative of the node position within the watershed: 0 (mesh interior node), 1 (closed mesh boundary node), 2 (open mesh boundary or outlet node), 3 (stream network node). Proper creation and consistency of the Points File is very important for the tRIBS Model and the ``*.points`` file should be carefully inspected using ArcView GIS or a similar environment. The Points File can be the appropriate method of TIN input for points obtained from a field survey, from a GIS point coverage, from a sampled DEM or from a ungenerated TIN mesh.

    The Points File is the recommended TIN input for the tRIBS Model during the initial model construction, usually necessary when a new basin is modeled for the first time. After a successful tRIBS model run, the model outputs a set of files that describe the TIN mesh properties in greater detail, including the connectivity between nodes and the triangles within the mesh. The set of files includes: ``*.nodes``, ``*.edges``, ``*.tri`` and ``*.z``. These files can be read directly into the model during subsequent model runs, thus avoiding the use of the ``*.points`` file and speeding up the process of mesh construction. Further details on both of these options will be discussed in subsequent sections and are also available in Tucker (1999) [Tucker_1999]_ .


Point Station Input
-------------------------

    Hydrometeorological data can be inputted into the tRIBS model through methods for Point Station Input implemented in the ``tEvapoTrans`` and ``tRainfall`` classes and the ``tHydroMet`` and ``tRainGauge`` storage classes. Point Station Input is useful for providing meteorological data from a sparse set of weather stations or for providing rain gauge rainfall data, instead for radar rainfall maps, to the model. The data from these sparse stations or points is resampled onto the Voronoi Polygon Network (VPN) by using a Thiessen polygon method at the point coordinates. The station properties, including coordinates, are specified through an SDF file (Station Descriptor File), while the station data are provided in an MDF file (Meteorological Data File). Both file types are discussed in the section on Meteorological Point Data Input in this document.


Text File Inputs
----------------------

    Various types of text files are used in the tRIBS Model to specify model options, hydrologic parameters or control commands. The most important of the text files is the Model Input File (``*.in``). This file contains various required and optional parameters organized by keywords. The format for each parameter consists of a line of descriptive text followed by the value of the parameter itself on a second line. There are over 40 different keyword inputs in a typical Model Input File. These can be classified into various groupings: Model Run Parameters, Model Run Options and Model Input Files and Pathnames. Subgroupings include: Time Variables, Routing Variables, Mesh Generation, Resampling Grids, Meteorological Data and Output Data. More details concerning the Model Input File will be presented in the section on Model Input File in this document.

    Another important use of text files is for the reclassification of soil and land use grids into meaningful hydrologic parameters assigned to each Voronoi polygon. A simple text file is used to relate each cover class to the particular hydrologic parameter required for the model equations. It consists of a small header followed by a matrix of parameter values for each cover class. In the case of the soil reclassification table (``*.sdt``), the parameters are used to specify the soil hydraulic and thermal properties. In the case of the land reclassification table (``*.ldt``), the parameters are used to relate the cover type to the interception and evapotranspiration properties of the vegetation and land cover. Both types of files will be explain in greater detail in the section on Soil and Land Use Input.

    A text file can also be used to run the model and specify the command line options desired during the run by using a Model Run File (``*_run``). This file consists of a single line that specifies the pathname of the tRIBS executable followed by the name of the Model Input File and the desired command line options.


Special Parallel Model Inputs
-----------------------------------

    The tRIBS model utilizes the same model input formats (``*.points`` file for TIN input, ASCII grids for vegetation and soils input, etc.) as in the tRIBS model. The parallel mode can be toggled on/off using the keyword *PARALLELMODE* in the tRIBS Model Input file (``*.in``). In this section, we will only provide details on the input of the graph partitioning files (``*.graph``). The graph files are utilized to specify how a large watershed domain is partitioned into subbasins and on which computer processor each subbasin is run on. There are currently three methods implemented to partition a domain:

        1. A default partitioning of the graph;
        2. A reach-based partitioning; and
        3. An inlet/outlet-based partitioning.

    The various options can be selected utilizing the keyword *GRAPHOPTION*. The default graph partitioning is based on an automatic splitting of the internal node list. It is a simple method that does not permit user control or interaction. As a result, it may not be an optimal way for subdividing a domain into a well-balanced computational effort among different processors. The reach-based and inlet/outlet-based methods require user input of a file into tRIBS by specifying the filename using the keyword *GRAPHFILE*. The file structure varies for each type of domain decomposition. The following tables indicate the file structure for the reach-based and inlet/outlet-based approaches.

          **Table 3.1** Reach-based Graph Input File (``*.graph``)

          .. tabularcolumns:: |c|c|

          +-------------------------+-------------------------+
          | Processor ID (#)        | Reach ID (#)            |
          +-------------------------+-------------------------+
          | Processor ID (#)        | Reach ID (#)            |
          +-------------------------+-------------------------+
          | Processor ID (#)        | Reach ID (#)            |
          +-------------------------+-------------------------+
          | Processor ID (#)        | Reach ID (#)            |
          +-------------------------+-------------------------+
          | ...                     | ...                     |
          +-------------------------+-------------------------+


    The reach-based graph input (**Table 3.1**) is essentially a two-column text file with no header. Column 1 holds the numerical IDs of the computer processors to be used (labeled from 0 to N) while Column 2 holds the numerical IDs (labeled from 0 to M) of the reaches to be run on the corresponding computer processors. The number of available computer processors will depend on the cluster in use. The number of reaches will depend on the size of the problem treated. For large domains, manual construction of the graph input file may become cumbersome. The reach IDs need to be determined from the ``*.reach`` file generated by the tRIBS model after mesh construction. This file is typically imported as a line coverage into a GIS package to identify the spatial location of each reach and their corresponding reach ID. The user will need to determine the most appropriate method for distributing the various reaches onto the available processors. Proper load balancing needs to be considered to distribute effort among different subbasins. Vivoni *et al.* (2006) [Vivoni_2006]_ presents a discussion of this issue with respect to some test cases.

    The inlet/outlet-based graph input (**Table 3.2**) is essentially a three-column text file with no header. Column 1 holds the numerical IDs of the computer processors to be used (labeled from 0 to N), Column 2 holds the numerical IDs of the channel nodes that form the inlet (upstream) segment of a reach and Column 3 holds the numerical IDs of the channel nodes that form the outlet (downstream) segment of a reach. Inlet nodes are typically inside sub-basins along the headwater areas, while outlet nodes are typically the closest downstream location along the main channel. The inlet/outlet-based graph partitioning provides for flexibility to the user, but may be more complicated to set up. The inlet/outlet IDs need to be determined from the ``*.voi`` file generated by the tRIBS model after mesh construction. This file is typically imported as a polygon coverage into a GIS package to identify the spatial location of each node and their corresponding ID. As with the above case, the user will need to experiment with the inlet/outlet partitioning in order to obtain proper load balancing and performance.

        **Table 3.2** Inlet/Outlet-based Graph Input File (``*.graph``)

        .. tabularcolumns:: |c|c|c|

        +-------------------------+-------------------------+--------------------------+
        | Processor ID (#)        | Inlet ID (#)            | Outlet ID (#)            |
        +-------------------------+-------------------------+--------------------------+
        | Processor ID (#)        | Inlet ID (#)            | Outlet ID (#)            |
        +-------------------------+-------------------------+--------------------------+
        | Processor ID (#)        | Inlet ID (#)            | Outlet ID (#)            |
        +-------------------------+-------------------------+--------------------------+
        | Processor ID (#)        | Inlet ID (#)            | Outlet ID (#)            |
        +-------------------------+-------------------------+--------------------------+
        | ...                     | ...                     | ...                      |
        +-------------------------+-------------------------+--------------------------+


-------------------------------------------------

    *Last update:* 02/06/2021 C. Lizarraga
