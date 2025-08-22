Model Parameters and Forcings
==============================

A tRIBS model application consists of a series of Voronoi polygons that discretely represent a watershed. At each TIN node associated with a Voronoi polygon, a set of governing equations describing the hydrologic and energy processes are computed. To solve these equations for each location, spatially distributed data describing surface topography, soils, vegetation and hydrometeorology are required, which can be obtained from field measurements, remote sensing data interpretation or model output such as numerical weather models. 

Model Parameters
------------------

In the case of soil and vegetation cover, different data sources can be used to assign uniform or spatially-explicit parameter values for the governing equations. The soils parameters listed in **Table 3.1** are necessary for carrying out the vertical infiltration and lateral flow redistribution, as well computing the soil heat budget within the sloped, heterogeneous, anisotropic soil columns assumed at each Voronoi polygon. Note that the parameter requirements vary with the specified hydrologic processes selected during a model run. For example, the surface heat parameters are only required if the radiation balance is computed. The order in which the soil parameters are listed in the tabular input should correspond to the list below and the units should match. Actual parameter names are not important for tabular input, whereas raster-based inputs assume slightly different naming conventions. Similar units need to be used when specifying soil parameters as raster inputs. 

        **Table 3.1** tRIBS Soil Parameter Description

        .. tabularcolumns:: |c|c|c|c|

        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  **Parameter**     |  **GDF Name**      |  **Description**                              |  **Unit**          |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Ks*              |  *KS*              |  Saturated Hydraulic Conductivity             |  [mm/hr]           |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  :math:`\theta_S`  |  *TS*              |  Saturated Soil Moisture                      |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  :math:`\theta_R`  |  *TR*              |  Residual Soil Moisture                       |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *m*               |  *PI*              |  Pore Distribution Index                      |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *PsiB*            |  *PB*              |  Air Entry Bubbling Pressure                  |  [mm] (negative)   |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *f*               |  *FD*              |  Hydraulic Decay Parameter                    |  [1/mm]            |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *As*              |  *AR*              |  Saturated Anisotropy Ratio                   |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Au*              |  *US*              |  Unsaturated Anisotropy Ratio                 |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *n*               |  *PO*              |  Porosity                                     |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *ks*              |  *VH*              |  Volumetric Heat Conductivity                 |  [J/msK]           |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Cs*              |  *SH*              |  Soil Heat Capacity                           |  [J/m3K]           |
        +--------------------+--------------------+-----------------------------------------------+--------------------+

The vegetation or land-use parameters in **Table 3.2** are necessary for carrying out the rainfall interception, bare soil evaporation and evapotranspiration within each Voronoi polygon, as well as determining the radiation and energy balance within the land surface. Note that the parameter requirements vary with the specified processes selected during a model run. For example, rainfall interception can be computed using a simple canopy storage method or the more complex Rutter model. The order in which the vegetation parameters are listed in the tabular input should correspond to the list below and the units should match. Actual parameter names are not important for tabular input, whereas raster-based inputs assume slightly different naming conventions. Similar units need to be used when specifying vegetation parameters as raster inputs. 

        **Table 3.2** tRIBS Vegetation or Land Use Description

        .. tabularcolumns:: |c|c|c|c|

        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  **Parameter**     |  **GDF Name**      |  **Description**                              |  **Unit**          |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *a*               |  None              |  Canopy Storage - Storage Method              |  [mm]              |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *b1*              |  None              |  Interception Coefficient - Storage Method    |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *P*               |  *TF*              |  Free Throughfall Coefficient - Rutter Method |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *S*               |  *CC*              |  Canopy Field Capacity - Rutter Method        |  [mm]              |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *K*               |  *DC*              |  Drainage Coefficient - Rutter Method         |  [mm/hr]           |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *b2*              |  *DE*              |  Drainage Exponent - Rutter Method            |  [1/mm]            |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Al*              |  *AL*              |  Albedo                                       |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *h*               |  *VH*              |  Vegetation Height                            |  [m]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Kt*              |  *OT*              |  Optical Transmission Coefficient             |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *Rs*              |  *SR*              |  Stomata Resistance                           |  [s/m]             |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *V*               |  *VF*              |  Vegetation Fraction                          |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        |  *LAI*             |  *LA*              |  Leaf Area Index                              |  [-]               |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        | :math:`\theta^*_s` |  *SE*              |   Stress Threshold for Evaporation            |  [-]               |
        |                    |                    |   [:math:`\theta_R` to :math:`\theta_S`]      |                    |
        +--------------------+--------------------+-----------------------------------------------+--------------------+
        | :math:`\theta^*_t` |  *ST*              |   Stress Threshold for Transpiration          |  [-]               |
        |                    |                    |   [:math:`\theta_R` to :math:`\theta_S`]      |                    |
        +--------------------+--------------------+-----------------------------------------------+--------------------+

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

            +---------+---------+-----------------+-----------------+-----+--------+-----+------+------+-----+-----+-----+
            |*#Types* |*nParams*|                                                                                        |
            +---------+---------+-----------------+-----------------+-----+--------+-----+------+------+-----+-----+-----+
            |  *ID*   |  *Ks*   |:math:`\theta_S` |:math:`\theta_R` | *m* | *PsiB* | *f* | *As* | *Au* | *n* | *ks*| *Cs*|
            +---------+---------+-----------------+-----------------+-----+--------+-----+------+------+-----+-----+-----+
          
            **Table 3.5** Land Use Reclassification Table Structure (``*.ldt``)

            .. raw:: html

               <div style="overflow-x: auto;">

            .. tabularcolumns:: |c|c|c|c|c|c|c|c|c|c|c|c|c|c|c|

            +---------+---------+------+----+-----+-----+-----+------+-----+------+-----+-----+------+--------------------+--------------------+
            |*#Types* |*nParams*|                                                                                                              |
            +---------+---------+------+----+-----+-----+-----+------+-----+------+-----+-----+------+--------------------+--------------------+
            |  *ID*   |  *A*    | *b1* |*P* | *S* | *K* | *b2*| *Al* | *h* | *Kt* | *Rs*| *V* | *LAI*| :math:`\theta^*_s` | :math:`\theta^*_t` |
            +---------+---------+------+----+-----+-----+-----+------+-----+------+-----+-----+------+--------------------+--------------------+
            
            .. raw:: html

               </div>

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
            | *SE*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+
            | *ST*       |  *Grid File Pathname* | *Grid Extension* |
            +------------+-----------------------+------------------+

In the above ``*.gdf`` files, note that the first line specifies the total number of parameters to be inputted, while the second line is used to input a representative absolute latitude, longitude and GMT values for all the input grids. The next *#Params* lines are used to specify the parameter code, the file pathname of the land cover parameter grid (including the basename of the file) and the extension given to the particular grid. The *NO_DATA* flag is used to specify the grids that are not available for a particular parameter. 

Model Forcings
----------------

In the case of hydrometeorological forcings, model inputs can be achieved in a number of different ways: (1) point input of hydrometeorological observations; (2) grid input of meteorological observations or numerical model results, or (3) point input of stochastic climate simulations. The model can handle the meteorological forcing in the point or grid format and has internal routines to assign this information to Voronoi polygons or TIN nodes via Thiessen resampling or nearest neighbor approaches.

**Table 3.8** lists the hydrometeorological model forcings. The primary hydrometeorological parameter is rainfall at a specified temporal resolution, typically hourly. Sub-hourly forcing can be specified despite having no minute column, by simply providing the data in order using the same hour in the hour column. The requirement of the other meteorological parameters depends on the processes selected for the model run. Some of the parameter information is redundant, for example dew point temperature and relative humidity are interchangeable. When incoming solar radiation is used, sky cover is not neeed. Other information can be input directly or computed within the model, for example net radiation, using the other meteorological measurements. The naming convention for each variable is used when specifying raster-based inputs. Units should be preserved. 

        **Table 3.8** tRIBS Hydrometeorological Parameter Description

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

Meteorological input into tRIBS can from point data or grid data, depending on the data sources available. The two data inputs are treated differently in the model. **Table 3.9** shows the two forms of meteorological data input and storage.

            **Table 3.9** Meteorological Data Input Methods

            .. tabularcolumns:: |c|c|c|

            +--------------+--------------------------------------+-----------------------------------------------+
            |Characteristic|  Point Data                          |  Grid Data                                    |
            +==============+======================================+===============================================+
            |  *Input*     |*Station Descriptor File* (``*.sdf``) |*ASCII grids* (``*.txt``, ``*.lan``, ``*.soi``)|
            +--------------+--------------------------------------+-----------------------------------------------+
            |              |*Meteorological Data File* (``*.mdf``)|                                               |
            +--------------+--------------------------------------+-----------------------------------------------+
            | *Storage*    | *Assignment to storage objects*      | *Direct assignment to* ``tCNode``             |
            +--------------+--------------------------------------+-----------------------------------------------+
            |*Manipulation*|*Thiessen point resampling*           | *Grid resampling*                             |
            +--------------+--------------------------------------+-----------------------------------------------+
            | *Examples*   | ``tHydroMet``, ``tRainGauge``        | ``tRainfall``, ``tVariant``, ``tInvariant``   |
            +--------------+--------------------------------------+-----------------------------------------------+

The format of the Station Descriptor Files (``*.sdf``) and the Meteorological Data Files (``*.mdf``) is modified slightly depending on whether these contain meteorological or rain gauge data. 

        **Table 3.10.** Weather Station SDF Structure

        .. raw:: html

           <div style="overflow-x: auto;">

        .. tabularcolumns::     |c|c|c|c|c|c|c|c|c|c|c|

        +-----------+----------+--------+--------+---------+----------+---------+--------------+----------------+------------------+
        |*#Stations*|*#Params* |        |        |         |          |         |              |                |                  |
        +-----------+----------+--------+--------+---------+----------+---------+--------------+----------------+------------------+
        |*StationID*|*FilePath*|*AbsLat*|*RefLat*|*AbsLong*| *RefLong*| *GMT*   |*RecordLength*|*#WeatherParams*|*StationElevation*|
        +-----------+----------+--------+--------+---------+----------+---------+--------------+----------------+------------------+

        .. raw:: html

           </div>

        **Table 3.11.** Rain Gauge SDF Structure

        .. tabularcolumns::   |c|c|c|c|c|c|c|

        +-------------+------------+----------+-----------+----------------+---------------+--------------------+
        | *#Stations* | *#Params*  |          |           |                |               |                    |
        +-------------+------------+----------+-----------+----------------+---------------+--------------------+
        | *StationID* | *FilePath* | *RefLat* | *RefLong* | *RecordLength* | *#RainParams* | *StationElevation* |
        +-------------+------------+----------+-----------+----------------+---------------+--------------------+

Note the following: *#Stations* is the number of total stations to be read, *#Params* is the number of parameters for each of the subsequent lines, *StationID* must be unique values for each station (starting at 0), the *FilePath* refers to the MDF file for that particular station and must be relative to the location of the executable, the *AbsLong* and *AbsLat* must be in decimal degree (lat/long), the RefLong and RefLat must be in the same coordinate system as the input grids and watershed TIN, Greenwich Mean Time (*GMT*) is difference in hours between the location and the Greenwich Meridian (negative number in Western Hemisphere), the *RecordLength* is the length of the time series in the MDF file, the *#WeatherParams* and *#RainParams* are the number of parameters in the MDF file including the date and time, and *Other* is used for inputting additional station information, such as station elevation, if desired. These keywords are not included in the file, just the parameter value. 

           **Table 3.12** Weather Station MDF Structure

            .. tabularcolumns::  |c|c|c|c|c|c|c|c|c|c|c|

            +-----+-----+-----+-----+------+------------+------+------+------+------+------+
            | *Y* | *M* | *D* | *H* | *PA* | *TD/RH/VP* | *XC* | *US* | *TA* | *TS* | *NR* |
            +-----+-----+-----+-----+------+------------+------+------+------+------+------+
            | ... | ... | ... | ... | ...  | ...        | ...  | ...  | ...  | ...  | ...  |
            +-----+-----+-----+-----+------+------------+------+------+------+------+------+

           **Table 3.13** Rain Gauge MDF Structure

            .. tabularcolumns::  |c|c|c|c|c|

            +-----+-----+-----+-----+--------+
            | *Y* | *M* | *D* | *H* | *R*    |
            +-----+-----+-----+-----+--------+
            | ... | ... | ... | ... | ...    |
            +-----+-----+-----+-----+--------+

Note the following: the parameter names must be a placed in a header for each MDF file, the *TD/RH/VP* imply that either one of these parameters can be inputted into that particular field, there must be *RecordLength* number of lines following after the header in intervals, missing data must be inputted with the *NO_DATA* flag *= 9999.99*, and the units must be retained as indicated, including for *IS*, *NR* and *TS*. Rainfall (*R*) is typically specified in its own MDF file. Notice that the file does not contain a minute column. Nevertheless, sub-hourly data can be inputted into the model at intervals that are multiples of the *TIMESTEP*. For example, for 15-minute data, the user should specify four rows for each hour (same *H*) in order. A similar approach is taken for sub-hourly rain gauge data. 

An alternative input format type for meteorological data is with the use of grid data. This option in the tRIBS model is used with the keyword *METDATAOPTION = 2*, while the more traditional weather station data is specified with *METDATAOPTION = 1*.  The additional information is provided through a text file for reading in meteorological input (``*.gdf``) as specified through the keyword *HYDROMETGRID* in the Input File. The structure of the Grid Data File or GDF is presented in **Table 3.14**.

           **Table 3.14** Meteorological GDF File Structure

            .. tabularcolumns:: |c|c|c|

            +------------+----------------------+------------------+
            | *#Params*                                            |
            +------------+----------------------+------------------+
            | *Latitude* | *Longitude*          |  *GMT*           |
            +------------+----------------------+------------------+
            | *PA*       | *Grid File Pathname* | *Grid Extension* |
            +------------+----------------------+------------------+
            | *TD*       | *Grid File Pathname* | *Grid Extension* |
            +------------+----------------------+------------------+
            | *XC*       | *Grid File Pathname* | *Grid Extension* |
            +------------+----------------------+------------------+
            | *US*       | *Grid File Pathname* | *Grid Extension* |
            +------------+----------------------+------------------+
            | *TA*       | *Grid File Pathname* | *Grid Extension* |
            +------------+----------------------+------------------+
            | *IS*       | *NO_DATA*            | *NO_DATA*        |
            +------------+----------------------+------------------+
            | *TS*       | *NO_DATA*            | *NO_DATA*        |
            +------------+----------------------+------------------+
            | *NR*       | *NO_DATA*            | *NO_DATA*        |
            +------------+----------------------+------------------+
            | *RH*       | *NO_DATA*            | *NO_DATA*        |
            +------------+----------------------+------------------+

Note that the first line specifies the total number of parameters to be inputted, while the second line is used to input a representative absolute latitude, longitude and GMT values for all the input grids. The next *#Params* lines are used to specify the parameter code, the file pathname of the weather grid (including the basename of the file) and the extension given to the particular grid. The *NO_DATA* flag is used to specify that weather grids are not available for a particular parameter. All the keywords used to represent the parameters are fixed as well as the units. 

