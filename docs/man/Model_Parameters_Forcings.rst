Model Parameters and Forcings
==============================

A tRIBS model application consists of a series of Voronoi polygons that discretely represent a watershed. At each TIN node associated with a Voronoi polygon, a set of governing equations describing the hydrologic and energy processes are computed. To solve these equations for each location, spatially distributed data describing surface topography, soils, vegetation and hydrometeorology are required, which can be obtained from field measurements, remote sensing data interpretation or model output such as numerical weather models. 

Model Parameters
------------------

In the case of soil and vegetation cover, different data sources can be used to assign uniform or spatially-explicit parameter values for the governing equations. The soils parameters listed in **Table 3.1** are necessary for carrying out the vertical infiltration and lateral flow redistribution, as well computing the soil heat budget within the sloped, heterogeneous, anisotropic soil columns assumed at each Voronoi polygon. Note that the parameter requirements vary with the specified hydrologic processes selected during a model run. For example, the surface heat parameters are only required if the radiation balance is computed. The order in which the soil parameters are listed in the tabular input should correspond to the list below and the units should match. Actual parameter names are not important for tabular input, whereas raster-based inputs assume slightly different naming conventions. Similar units need to be used when specifying soil parameters as raster inputs. 

        **Table 3.1** tRIBS Soil Parameter Description

        .. tabularcolumns:: |c|c|c|

        +--------------------+-----------------------------------------------+--------------------+
        |  **Parameter**     |  **Description**                              |  **Unit**          |
        +--------------------+-----------------------------------------------+--------------------+
        |  *Ks*              |  Saturated Hydraulic Conductivity             |  [mm/hr]           |
        +--------------------+-----------------------------------------------+--------------------+
        |  *thetaS*          |  Saturated Soil Moisture                      |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *thetaR*          |  Residual Soil Moisture                       |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *m*               |  Pore Distribution Index                      |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *PsiB*            |  Air Entry Bubbling Pressure                  |  [mm] (negative)   |
        +--------------------+-----------------------------------------------+--------------------+
        |  *f*               |  Hydraulic Decay Parameter                    |  [1/mm]            |
        +--------------------+-----------------------------------------------+--------------------+
        |  *As*              |  Saturated Anisotropy Ratio                   |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *Au*              |  Unsaturated Anisotropy Ratio                 |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *n*               |  Porosity                                     |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *ks*              |  Volumetric Heat Conductivity                 |  [J/msK]           |
        +--------------------+-----------------------------------------------+--------------------+
        |  *Cs*              |  Soil Heat Capacity                           |  [J/m3K]           |
        +--------------------+-----------------------------------------------+--------------------+

The vegetation or land-use parameters in **Table 3.2** are necessary for carrying out the rainfall interception, bare soil evaporation and evapotranspiration within each Voronoi polygon, as well as determining the radiation and energy balance within the land surface. Note that the parameter requirements vary with the specified processes selected during a model run. For example, rainfall interception can be computed using a simple canopy storage method or the more complex Rutter model. The order in which the vegetation parameters are listed in the tabular input should correspond to the list below and the units should match. Actual parameter names are not important for tabular input, whereas raster-based inputs assume slightly different naming conventions. Similar units need to be used when specifying vegetation parameters as raster inputs. 

        **Table 3.2** tRIBS Vegetation or Land Use Description

        .. tabularcolumns:: |c|c|c|

        +--------------------+-----------------------------------------------+--------------------+
        |  **Parameter**     |  **Description**                              |  **Unit**          |
        +--------------------+-----------------------------------------------+--------------------+
        |  *a*               |  Canopy Storage - Storage Method              |  [mm]              |
        +--------------------+-----------------------------------------------+--------------------+
        |  *b1*              |  Interception Coefficient - Storage Method    |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *P*               |  Free Throughfall Coefficient - Rutter Method |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *S*               |  Canopy Field Capacity - Rutter Method        |  [mm]              |
        +--------------------+-----------------------------------------------+--------------------+
        |  *K*               |  Drainage Coefficient - Rutter Method         |  [mm/hr]           |
        +--------------------+-----------------------------------------------+--------------------+
        |  *b2*              |  Drainage Exponent - Rutter Method            |  [1/mm]            |
        +--------------------+-----------------------------------------------+--------------------+
        |  *Al*              |  Albedo                                       |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *h*               |  Vegetation Height                            |  [m]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *Kt*              |  Optical Transmission Coefficient             |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *Rs*              |  Stomata Resistance                           |  [s/m]             |
        +--------------------+-----------------------------------------------+--------------------+
        |  *V*               |  Vegetation Fraction                          |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *LAI*             |  Leaf Area Index                              |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *thetas*          |   Stress Threshold for Evaporation [0 to 1]   |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *thetat*          |   Stress Threshold for Transpiration [0 to 1] |  [-]               |
        +--------------------+-----------------------------------------------+--------------------+

Input Formats
~~~~~~~~~~~~~~

One form of data input for soil textural and vegetation data is through the use of ASCII grids consisting of soil or land use codes or indices. The soil (``*.soi``) and land use (``*.lan``) grids are specified in the Input File by using the keywords *SOILMAPNAME* and *LANDMAPNAME*. **Table 3.3** presents an example of a soil or land use grid. As with other grid input, care should be taken to specify the grids in the same coordinate system as the topographic TIN data. 

            **Table 3.3** Example of Soil or Land Use Class ASCII grid (``*.soi`` and ``*.lan``)

            .. tabularcolumns:: |c|c|c|c|c|c|

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

The parameter values for the soil and land use grids are read from a reclassification table inputted separately using the keywords *SOILTABLENAME* and *LANDTABLENAME*. The format of these soil reclassification (``*.sdt``) and land use reclassification (``*.ldt``) tables include a small header which specifies the number of cover types (*#Types*) and the number of variables for each type (*#Params*), as shown in **Table 3.4** and **Table 3.5**. The header is followed by a matrix of parameter values where each row represents one cover type and each column represents one parameter. The order and units of these parameters are **fixed**. Since parameter values outside the appropriate range may results in inaccurate calculations, the user should be careful to select realistic values from literature sources prior to model use.

            **Table 3.4** Soil Reclassification Table Structure (``*.sdt``)

            .. tabularcolumns:: |c|c|c|c|c|c|c|c|c|c|c|c|

            +---------+---------+---------+---------+-----+--------+-----+------+------+-----+-----+-----+
            |*#Types* |*nParams*|                                                                        |
            +---------+---------+---------+---------+-----+--------+-----+------+------+-----+-----+-----+
            |  *ID*   |  *Ks*   |*thetaS* |*thetaR* | *m* | *PsiB* | *f* | *As* | *Au* | *n* | *ks*| *Cs*|
            +---------+---------+---------+---------+-----+--------+-----+------+------+-----+-----+-----+

            **Table 3.5** Land Use Reclassification Table Structure (``*.ldt``)

            .. tabularcolumns:: |c|c|c|c|c|c|c|c|c|c|c|c|c|

            +---------+---------+------+----+-----+-----+-----+------+-----+------+-----+-----+------+
            |*#Types* |*nParams*|                                                                    |
            +---------+---------+------+----+-----+-----+-----+------+-----+------+-----+-----+------+
            |  *ID*   |  *a*    | *bI* |*P* | *S* | *K* | *b2*| *Al* | *h* | *Kt* | *Rs*| *V* | *LAI*|
            +---------+---------+------+----+-----+-----+-----+------+-----+------+-----+-----+------+

Note that the soil parameters relate to the hydraulic and thermal properties in the upper portions of the soil profile. Most of these can be directly related to the surface soil texture. The first nine parameters are essential for running the Unsaturated Zone Model while the last two are required if the keyword *GFLUXOPTION = 1*. Note that these land use parameters relate to the interception and evaporation properties of the vegetative cover or land use type. The first two parameters are required if the keyword *OPTINTERCEPT = 1*, while the next four are required if *OPTINTERCEPT = 2*. The final five parameters are required for various options of the keyword *OPTEVAPOTRANS*. The last two parameters have been added to specify the soil moisture stress threshold for soil evaporation and plant transpiration in units of relative soil moisture (varying from 0 to 1).

Gridded soil data can be used as an alternative to the tabular soil parameter input. To activate the use of the gridded soil data the user must the keyword *OPTSOILTYPE = 1* in the Input File (``*.in``). If *OPTSOILTYPE = 0* then the use of the tabular data will be selected. The information is provided through the use of a text file for reading soil grid input (``*.gdf``) specified through the keyword *SCGRID*. The structure of the soil grid data file or GDF is shown in **Table 3.6**. 

    **Table 3.6** Soil Parameter GDF File Structure

            .. tabularcolumns::  |c|c|c|

            +------------+-----------------------+------------------+
            | *#Params*                                             |
            +------------+-----------------------+------------------+
            | *Latitude* |  *Longitude*          |  *GMT*           |
            +------------+-----------------------+------------------+
            | *KS*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *TS*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *TR*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *PI*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *PB*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *FD*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *AR*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *UA*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *PO*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *VH*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *SH*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+

An alternative input format type for dynamic land cover data is with the use of grid data. This option in the tRIBS model is used with the keyword *OPTLANDUSE = 1*, while the more static land cover is specified with *OPTLANDUSE = 0*. The use of dynamic land cover variables maybe convenient for inputting remotely sensed vegetation fields. Information is provided through a text file for reading in land cover grid input (``*.gdf``) as specified through the keyword *LUGRID* in the Input File. The structure of the Grid Data File or GDF is presented in **Table 3.7**.

    **Table 3.7** Land Cover GDF File Structure

            .. tabularcolumns::  |c|c|c|

            +------------+-----------------------+------------------+
            | *#Params*                                             |
            +------------+-----------------------+------------------+
            | *Latitude* |  *Longitude*          |  *GMT*           |
            +------------+-----------------------+------------------+
            | *AL*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *TF*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *VH*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *SR*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *VF*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *CS*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *IC*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *CC*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *DC*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *DE*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *OT*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *LA*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+

In the above ``*.gdf`` files, note that the first line specifies the total number of parameters to be inputted, while the second line is used to input a representative absolute latitude, longitude and GMT values for all the input grids. The next *#Params* lines are used to specify the parameter code, the file pathname of the land cover parameter grid (including the basename of the file) and the extension given to the particular grid. The *NO_DATA* flag is used to specify the grids that are not available for a particular parameter. 

Model Forcings
----------------

In the case of hydrometeorological forcings, model inputs can be achieved in a number of different ways: (1) point input of hydrometeorological observations; (2) grid input of meteorological observations or numerical model results, or (3) point input of stochastic climate simulations. The model can handle the meteorological forcing in the point or grid format and has internal routines to assign this information to Voronoi polygons or TIN nodes via Thiessen resampling or nearest neighbor approaches.

**Table 3.6** lists the hydrometeorological model forcings. The primary hydrometeorological parameter is rainfall at a specified temporal resolution, typically hourly. Sub-hourly forcing can be specified despite having no minute column, by simply providing the data in order using the same hour in the hour column. The requirement of the other meteorological parameters depends on the processes selected for the model run. Some of the parameter information is redundant, for example dew point temperature and relative humidity are interchangeable. When incoming solar radiation is used, sky cover is not neeed. Other information can be input directly or computed within the model, for example net radiation, using the other meteorological measurements. The naming convention for each variable is used when specifying raster-based inputs. Units should be preserved. 

        **Table 3.6** tRIBS Hydrometeorological Parameter Description

        .. tabularcolumns:: |c|c|c|

        +--------------------+-----------------------------------------------+--------------------+
        |  **Parameter**     |  **Description**                              |  **Unit**          |
        +--------------------+-----------------------------------------------+--------------------+
        |  *PA*              |  Atmospheric Pressure                         |  [mb]              |
        +--------------------+-----------------------------------------------+--------------------+
        |  *TD*              |  Dew Point Temperature                        |  [C]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *RH*              |  Relative Humidity                            |  [%]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *VP*              |  Vapor Pressure                               |  [mb]              |
        +--------------------+-----------------------------------------------+--------------------+
        |  *XC*              |  Sky Cover                                    |  [tenths] (0 to 10)|
        +--------------------+-----------------------------------------------+--------------------+
        |  *US*              |  Wind Speed                                   |  [m/s]             |
        +--------------------+-----------------------------------------------+--------------------+
        |  *TA*              |  Air Temperature                              |  [C]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *TS*              |  Surface Temperature                          |  [C]               |
        +--------------------+-----------------------------------------------+--------------------+
        |  *NR*              |  Net Radiation                                |  [W/m2]            |
        +--------------------+-----------------------------------------------+--------------------+
        |  *R*               |  Rainfall                                     |  [mm/hr]           |
        +--------------------+-----------------------------------------------+--------------------+
        |  *IS*              |  Incoming Solar Radiation                     |  [W/m2]            |
        +--------------------+-----------------------------------------------+--------------------+

Input Formats
~~~~~~~~~~~~~~
