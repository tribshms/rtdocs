Model Outputs
==================================

The tRIBS Model produces a number of output files that represent the time series or the spatial distribution of model state or output variables. Output variables include the position of moisture fronts in the unsaturated zone, water table elevation, surface runoff, subsurface flux, rainfall rate, interception loss, evapotranspiration, and information on the mesh triangulation. **Table 5.1**, **Table 5.2**, and **Table 5.3** summarize: (1) mesh output files, (2) time series outputs, and (3) spatial outputs. More detailed descriptions of the individual files are provided in the following sections.

    **Table 5.1** tRIBS Mesh Output Files

            .. tabularcolumns::  |c|c|l|

            +------------------------------+------------------+----------------------------------------------------------------+
            | Model Mesh Files             |  Extension       |  Description                                                   |
            +==============================+==================+================================================================+
            |*Mesh Node File*              |  ``*.nodes``     |  Node (x,y), ID of spoke, boundary code.                       |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Mesh Edge File*              |  ``*.edges``     |  ID of origin and destination node, ID of CCW edge.            |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Mesh Triangle File*          |  ``*.tri``       |  ID of vertex nodes, ID of neighboring triangles opposite the  |
            +------------------------------+------------------+----------------------------------------------------------------+
            |                              |                  | vertex node, ID of CCW edge originating with the vertex node.  |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Mesh Node Elevation File*    | ``*.z``          |  Node elevation (meters).                                      |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Mesh Voronoi Geometry*       | ``*_voi``        |  File containing individual Voronoi polygon geometry.          |
            +------------------------------+------------------+----------------------------------------------------------------+

    **Table 5.2** tRIBS Model Time Series Files

            .. tabularcolumns::  |c|c|l|

            +------------------------------+------------------+----------------------------------------------------------------+
            | Time Series                  |  Extension       | Description                                                    |
            +==============================+==================+================================================================+
            |*Discharge Time Series*.      |``_Outlet.qout``  | Time series of outlet hydrograph (m³/s).                       |
            |                              |  ``or *qout``.   |                                                                |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Basin Averaged File*         |  ``*.mrf``       | Time series of basin-averaged model variables.                 |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Hydrograph Runoff Types File*|  ``*.rft``       | Time series of outlet hydrograph by runoff type (m³/s).        |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Node Dynamic Output File*    |  ``*.pixel``     | Time series of dynamic variables for a specific node.          |
            +------------------------------+------------------+----------------------------------------------------------------+

    **Table 5.3** tRIBS Model Spatial Output Files

            .. tabularcolumns::  |c|c|l|

            +------------------------------+------------------+----------------------------------------------------------------+
            |Model Spatial Output Files    |  Extension       |  Description                                                   |
            +==============================+==================+================================================================+
            |*Mesh Dynamic Output File*    |``*timestamp_00d``|  Dynamic variable output for all mesh nodes at specific time.  |
            +------------------------------+------------------+----------------------------------------------------------------+
            |*Mesh Integrated Output File* |``*timestamp_00i``|  Time-integrated variable output for all mesh nodes.           |
            +------------------------------+------------------+----------------------------------------------------------------+

    The location of the output files is specified in the tRIBS Model Input File by using the keywords *OUTFILENAME* and *OUTHYDROFILENAME*. An important note to make is that the ``*.mrf``, ``*.rft`` and ``*.dat`` files produced by the model are labeled with additional identifiers before the extension that relate to the time of the output. For each *OPINTRVL* time step, the model will produce output of the ``*.mrf`` type, while the ``*.rft`` file is produced only after completion of the entire run. The spatial output (``*timestamp_00d``) are determined by the time step specified in the *SPOPINTRVL* keyword. Time-integrated spatial output (``*timestamp_00i``) is produced only at the end of the simulation. The model also produces various files with a ``*.pixel`` extension followed by a node ID number at the end of the run. The ``*.pixel#`` files contain the dynamic variable output for a single node for all model times. The number of ``*.pixel#`` files produced is specified through a Node Output List (``*.nol``) File described below.

    **Table 5.4** Node Output List File Structure

            .. tabularcolumns:: |c|c|c|

            +-----------+-----------+-----------+
            | *#Nodes*  |                       |
            +-----------+-----------+-----------+
            | *NodeID1* | *NodeID2* | *NodeID3* |
            +-----------+-----------+-----------+

    A similar structure and file is used for the keyword *HYDRONODELIST* and *OUTLETNODELIST*. Using this file, allows the user to obtain the runtime hydrologic information in the unsaturated and saturated model for each time step as output to the screen, a useful tool for debugging. No filename suppresses the debugging information.

Time Series
-----------

Basin Outlet Discharge Time Series
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 5.5** Content of *_Outlet.qout file or *qout file if Voronoi IDs are provided via OUTLETNODELIST

        .. tabularcolumns:: |c|c|c|

        +-------+-------------------+--------+
        | Column| Description       | Units  |
        +=======+===================+========+
        | 1     | Time              | [hr]   |
        +-------+-------------------+--------+
        | 2     | Discharge, Qstrm | [m3/s]  |
        +-------+-------------------+--------+
        | 3     | Channel stage,    | [m]    |
        |       | HLevel            |        |
        +-------+-------------------+--------+

Hydrologic Time Series at Selected TIN nodes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 5.6** Content of *.pixel files

        .. tabularcolumns:: |c|c|c|

        +-------+--------------------------------------------+--------+
        | Column| Description                                | Units  |
        +=======+============================================+========+
        | 1     | Node Identification, ID                    | [id]   |
        +-------+--------------------------------------------+--------+
        | 2     | Time                                       | [hr]   |
        +-------+--------------------------------------------+--------+
        | 3     | Depth to groundwater table, Nwt            | [mm]   |
        +-------+--------------------------------------------+--------+
        | 4     | Wetting front depth, Nf                    | [mm]   |
        +-------+--------------------------------------------+--------+
        | 5     | Top front depth, Nt                        | [mm]   |
        +-------+--------------------------------------------+--------+
        | 6     | Total moisture above the water table, Mu   | [mm]   |
        +-------+--------------------------------------------+--------+
        | 7     | Moisture content in the initialization     | [mm]   |
        |       | profile, Mi                                |        |
        +-------+--------------------------------------------+--------+
        | 8     | Unsaturated lateral flow out from cell,    | [mm/hr]|
        |       | Qpout                                      |        |
        +-------+--------------------------------------------+--------+
        | 9     | Unsaturated lateral flow into cell, Qpin   | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 10    | Transmissivity, Trnsm                      | [m²/hr]|
        +-------+--------------------------------------------+--------+
        | 11    | Groundwater flux, GWflx                    | [m³/hr]|
        +-------+--------------------------------------------+--------+
        | 12    | Surface Runoff, Srf                        | [mm]   |
        +-------+--------------------------------------------+--------+
        | 13    | Rainfall, Rain                             | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 14    | Soil Moisture, top 10 cm, SoilMoist        | [ ]    |
        +-------+--------------------------------------------+--------+
        | 15    | Root Zone Moisture, top 1 m, RootMoist     | [ ]    |
        +-------+--------------------------------------------+--------+
        | 16    | Air Temperature, AirT                      | [°C]   |
        +-------+--------------------------------------------+--------+
        | 17    | Dew Point Temperature, DewT                | [°C]   |
        +-------+--------------------------------------------+--------+
        | 18    | Surface Temperature, SurfT                 | [°C]   |
        +-------+--------------------------------------------+--------+
        | 19    | Soil Temperature, SoilT                    | [°C]   |
        +-------+--------------------------------------------+--------+
        | 20    | Atmospheric Pressure, Press                | [Pa]   |
        +-------+--------------------------------------------+--------+
        | 21    | Relative Humidity, RelHum                  | [ ]    |
        +-------+--------------------------------------------+--------+
        | 22    | Sky Cover, SkyCov                          | [ ]    |
        +-------+--------------------------------------------+--------+
        | 23    | Wind Speed, Wind                           | [m/s]  |
        +-------+--------------------------------------------+--------+
        | 24    | Net Radiation, NetRad                      | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 25    | Incoming Shortwave Radiation, ShrtRadIn    | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 26    | Incoming Direct Shortwave Radiation,       | [W/m²] |
        |       | ShrtRadIn_dir                              |        |
        +-------+--------------------------------------------+--------+
        | 27    | Incoming Diffuse Shortwave Radiation,      | [W/m²] |
        |       | ShrtRadIn_dif                              |        |
        +-------+--------------------------------------------+--------+
        | 28    | Shortwave Absorbed Radiation, Vegetation,  | [W/m²] |
        |       | ShortAbsbVeg                               |        |
        +-------+--------------------------------------------+--------+
        | 29    | Shortwave Absorbed Radiation, Soil,        | [W/m²] |
        |       | ShortAbsbSoi                               |        |
        +-------+--------------------------------------------+--------+
        | 30    | Incoming Longwave Radiation, LngRadIn      | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 31    | Outgoing Longwave Radiation, LngRadOut     | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 32    | Potential Evaporation, PotEvp              | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 33    | Actual Evaporation, ActEvp                 | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 34    | Total Evapotranspiration, EvpTtrs          | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 35    | Evaporation from Wet Canopy, EvpWetCan     | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 36    | Evaporation from Dry Canopy,               | [mm/hr]|
        |       | EvpDryCan                                  |        |
        +-------+--------------------------------------------+--------+
        | 37    | Evaporation from Bare Soil, EvpSoil        | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 38    | Ground Heat Flux, Gflux                    | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 39    | Sensible Heat Flux, Hflux                  | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 40    | Latent Heat Flux, Lflux                    | [W/m²] |
        +-------+--------------------------------------------+--------+
        | 41    | Net Precipitation, NetPrecip               | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 42    | Liquid Water Equivalent, LiqWE             | [cm]   |
        +-------+--------------------------------------------+--------+
        | 43    | Ice Water Equivalent, IceWE                | [cm]   |
        +-------+--------------------------------------------+--------+
        | 44    | Snow Water Equivalent, SnWE                | [cm]   |
        +-------+--------------------------------------------+--------+
        | 45    | Internal Energy of Snow Pack, U            | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 46    | Routed Melt Water Equivalent, RouteWE      | [cm]   |
        +-------+--------------------------------------------+--------+
        | 47    | Snow Temperature, SnTemp                   | [°C]   |
        +-------+--------------------------------------------+--------+
        | 48    | Snow Surface Age, SurfAge                  | [hr]   |
        +-------+--------------------------------------------+--------+
        | 49    | Change in Snow Pack Internal Energy, DU    | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 50    | Latent Heat Flux from Snow Cover, snLHF    | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 51    | Sensible Heat Flux from Snow Cover, snSHF  | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 52.   | Ground Heat Flux from Snow Cover, snGHF    | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 53    | Precip Heat Flux from Snow Cover, snPHF    | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 54    | Outgoing Longw. Rad. from Snow, snRLout    | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 55    | Incom. Longw. Radn. from Snow, snRLin      | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 56    | Incom. Shortw. Radn. from Snow, snRSin     | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 57    | Error in Energy Balance, Uerror            | [kJ/m²]|
        +-------+--------------------------------------------+--------+
        | 58    | Intercepted Snow Water Equivalent, intSWEq | [cm]   |
        +-------+--------------------------------------------+--------+
        | 59    | Sublim. Snow Water Equiv. from Canopy,     | [cm]   |
        |       | intSub                                     |        |
        +-------+--------------------------------------------+--------+
        | 60    | Unloaded SWE from Canopy, intSnUnload      | [cm]   |
        +-------+--------------------------------------------+--------+
        | 61    | Canopy Storage, CanStorage                 | [mm]   |
        +-------+--------------------------------------------+--------+
        | 62    | Cumulative Interception, CumIntercept      | [mm]   |
        +-------+--------------------------------------------+--------+
        | 63    | Interception, Interception                 | [mm]   |
        +-------+--------------------------------------------+--------+
        | 64    | Recharge, Recharge                         | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 65    | Runon, RunOn                               | [mm]   |
        +-------+--------------------------------------------+--------+
        | 66    | Surface Runoff in Hour, srf_Hour           | [mm]   |
        +-------+--------------------------------------------+--------+
        | 67    | Discharge, Qstrm                           | [m³/s] |
        +-------+--------------------------------------------+--------+
        | 68    | Channel Stage, Hlevel                      | [m]    |
        +-------+--------------------------------------------+--------+
        | 69    | Canopy Storage Parameter, CanStorParam     | [mm]   |
        +-------+--------------------------------------------+--------+
        | 70    | Interception Coefficient, IntercepCoeff    | [ ]    |
        +-------+--------------------------------------------+--------+
        | 71    | Free Throughfall Coeff.- Rutter,           | [ ]    |
        |       | ThroughFall                                |        |
        +-------+--------------------------------------------+--------+
        | 72    | Canopy Field Capacity – Rutter, CanFieldCap| [mm]   |
        +-------+--------------------------------------------+--------+
        | 73    | Drainage coefficient – Rutter, DrainCoeff  | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 74    | Drainage Expon. Param. – Rutter,           | [mm⁻¹] |
        |       | DrainExpPar                                |        |
        +-------+--------------------------------------------+--------+
        | 75    | Albedo, LandUseAlb                         | [ ]    |
        +-------+--------------------------------------------+--------+
        | 76    | Vegetation Height , VegHeight              | [m]    |
        +-------+--------------------------------------------+--------+
        | 77    | Optical Transmission Coeff., OptTransmCoeff| [ ]    |
        +-------+--------------------------------------------+--------+
        | 78    | Canopy- Average Stomatal Resistance,       | [s/m]  |
        |       | StomRes                                    |        |
        +-------+--------------------------------------------+--------+
        | 79    | Vegetation Fraction, VegFraction           | [ ]    |
        +-------+--------------------------------------------+--------+
        | 80    | Canopy Leaf Area Index, LeafAI             | [ ]    |
        +-------+--------------------------------------------+--------+

Basin-averaged Hydrological Time Series
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 5.7** Content of *.mrf file

        .. tabularcolumns:: |c|c|c|

        +-------+--------------------------------------------+--------+
        | Column| Description                                | Units  |
        +=======+============================================+========+
        | 1     | Time                                       | [hr]   |
        +-------+--------------------------------------------+--------+
        | 2     | Surface Runoff from Hydrologic Routing, Srf| [m³/s] |
        +-------+--------------------------------------------+--------+
        | 3     | Mean Areal Precipitation, MAP              | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 4     | Maximum Rainfall Rate, Max                 | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 5     | Minimum Rainfall Rate, Min                 | [mm/hr]|
        +-------+--------------------------------------------+--------+
        | 6     | Forecast State, Fstate                     | [ ]    |
        +-------+--------------------------------------------+--------+
        | 7     | Mean Surface Soil Moisture (in top 10 cm), | [ ]    |
        |       | MSM100                                     |        |
        +-------+--------------------------------------------+--------+
        | 8     | Mean Soil Moisture in Root Zone (in top 1  | [ ]    |
        |       | m), MSMRt                                  |        |
        +-------+--------------------------------------------+--------+
        | 9     | Mean Soil Moisture in Unsaturated Zone     | [ ]    |
        |       | (above water table), MSMU                  |        |
        +-------+--------------------------------------------+--------+
        | 10    | Mean Depth to Groundwater, MGW             | [mm]   |
        +-------+--------------------------------------------+--------+
        | 11    | Mean Evapotranspiration, MET               | [mm]   |
        +-------+--------------------------------------------+--------+
        | 12    | Areal Fraction of Surface Saturation, Sat  | [ ]    |
        +-------+--------------------------------------------+--------+
        | 13    | Areal Fraction of Rainfall, Rain           | [ ]    |
        +-------+--------------------------------------------+--------+
        | 14    | Average Snow Water Equivalent, AvSWE       | [cm]   |
        +-------+--------------------------------------------+--------+
        | 15    | Average Amount of Snow Melt, AvMelt        | [cm]   |
        +-------+--------------------------------------------+--------+
        | 16    | Average Snow Temperature, AvSTC            | [°C]   |
        +-------+--------------------------------------------+--------+
        | 17    | Average Change in Snow Pack Internal       | [kJ/m²]|
        |       | Energy, AvDUint                            |        |
        +-------+--------------------------------------------+--------+
        | 18    | Average Latent Heat Flux from Snow         | [kJ/m²]|
        |       | Covered Areas, AvSLHF                      |        |
        +-------+--------------------------------------------+--------+
        | 19    | Average Sensible Heat Flux from Snow       | [kJ/m²]|
        |       | Covered Areas, AvSSHF                      |        |
        +-------+--------------------------------------------+--------+
        | 20    | Average Precipitation Heat Flux from Snow  | [kJ/m²]|
        |       | Covered Areas, AvSPHF                      |        |
        +-------+--------------------------------------------+--------+
        | 21    | Average Ground Heat Flux from Snow         | [kJ/m²]|
        |       | Covered Areas, AvSGHF                      |        |
        +-------+--------------------------------------------+--------+
        | 22    | Average Incoming Longwave Radiation from   | [kJ/m²]|
        |       | Snow Covered Areas, AvSRLI                 |        |
        +-------+--------------------------------------------+--------+
        | 23    | Average Outgoing Longwave Radiation from   | [kJ/m²]|
        |       | Snow Covered Areas, AvSRLO                 |        |
        +-------+--------------------------------------------+--------+
        | 24    | Average Incoming Shortwave Radiation from  | [kJ/m²]|
        |       | Snow Covered Areas, AvSRSI                 |        |
        +-------+--------------------------------------------+--------+
        | 25    | Mean Intercepted Snow Water Equivalent,    | [cm]   |
        |       | AvInSn                                     |        |
        +-------+--------------------------------------------+--------+
        | 26    | Mean Sublimation from Intercepted Snow,    | [cm]   |
        |       | AvInSu                                     |        |
        +-------+--------------------------------------------+--------+
        | 27    | Mean Unloaded Snow from Canopy, AvInUn     | [cm]   |
        +-------+--------------------------------------------+--------+
        | 28    | Fraction Snow Covered Area, SCA            | [ ]    |
        +-------+--------------------------------------------+--------+
        | 29    | Channel percolation, ChanP                 | [m³]   |
        +-------+--------------------------------------------+--------+

Basin-averaged Hydrological Time Series
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 5.8** Content for *.mrf files

        .. tabularcolumns:: |c|c|c|

        +-------+-----------------------------------+--------+
        | Column| Description                       | Units  |
        +=======+===================================+========+
        | 1     | Time                              | [hr]   |
        +-------+-----------------------------------+--------+
        | 2     | Infiltration-excess Runoff, Hsrf  | [m³/s] |
        +-------+-----------------------------------+--------+
        | 3     | Saturation-excess Runoff, Sbsrf   | [m³/s] |
        +-------+-----------------------------------+--------+
        | 4     | Perched Return Flow, Psrf         | [m³/s] |
        +-------+-----------------------------------+--------+
        | 5     | Groundwater Exfiltration, Satsrf  | [m³/s] |
        +-------+-----------------------------------+--------+

Spatial Output
----------------

Dynamic Spatial Output Tables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 5.9** Content of *timestamp_00d files

        .. tabularcolumns:: |c|c|c|

        +-------+---------------------------------------+----------+
        | Column| Description                           | Units    |
        +=======+=======================================+==========+
        | 1     | Node Identification, ID               | [id]     |
        +-------+---------------------------------------+----------+
        | 2     | Elevation, Z                          | [m]      |
        +-------+---------------------------------------+----------+
        | 3     | Slope, S                              | [radian] |
        +-------+---------------------------------------+----------+
        | 4     | Contributing Area, CAr                | [m²]     |
        +-------+---------------------------------------+----------+
        | 5     | Depth to groundwater table, Nwt       | [mm]     |
        +-------+---------------------------------------+----------+
        | 6     | Total moisture above the water table, | [mm]     |
        |       | Mu                                    |          |
        +-------+---------------------------------------+----------+
        | 7     | Moisture content in the initialization| [mm]     |
        |       | profile, Mi                           |          |
        +-------+---------------------------------------+----------+
        | 8     | Wetting front depth, Nf               | [mm]     |
        +-------+---------------------------------------+----------+
        | 9     | Top front depth, Nt                   | [mm]     |
        +-------+---------------------------------------+----------+
        | 10    | Unsaturated lateral flow out from     | [mm/hr]  |
        |       | cell, Qpout                           |          |
        +-------+---------------------------------------+----------+
        | 11    | Unsaturated lateral flow into cell,   | [mm/hr]  |
        |       | Qpin                                  |          |
        +-------+---------------------------------------+----------+
        | 12    | Surface Runoff, Srf                   | [mm]     |
        +-------+---------------------------------------+----------+
        | 13    | Rainfall, Rain                        | [mm/hr]  |
        +-------+---------------------------------------+----------+
        | 14    | Snow Water Equivalent, SWE            | [cm]     |
        +-------+---------------------------------------+----------+
        | 15    | Snow Temperature, ST                  | [°C]     |
        +-------+---------------------------------------+----------+
        | 16    | Ice Part of Water Equivalent, IWE     | [cm]     |
        +-------+---------------------------------------+----------+
        | 17    | Liquid part of Water Equivalent, LWE  | [cm]     |
        +-------+---------------------------------------+----------+
        | 18    | Change in Internal Energy of Snow Pack| [kJ/m²]  |
        |       | DU                                    |          |
        +-------+---------------------------------------+----------+
        | 19    | Internal Energy of Snow Pack, Upack   | [kJ/m²]  |
        +-------+---------------------------------------+----------+
        | 20    | Latent Heat Flux from Snow Cover, sLHF| [kJ/m²]  |
        +-------+---------------------------------------+----------+
        | 21    | Sensible Heat Flux from Snow Cover,   | [kJ/m²]  |
        |       | sSHF                                  |          |
        +-------+---------------------------------------+----------+
        | 22    | Ground Heat Flux from Snow Cover, sGHF| [kJ/m²]  |
        +-------+---------------------------------------+----------+
        | 23    | Precipitation Heat Flux from Snow     | [kJ/m²]  |
        |       | Cover, sPHF                           |          |
        +-------+---------------------------------------+----------+
        | 24    | Outgoing Longwave Radiation from Snow | [kJ/m²]  |
        |       | Cover, sRLo                           |          |
        +-------+---------------------------------------+----------+
        | 25    | Incoming Longwave Radation from Snow  | [kJ/m²]  |
        |       | Cover, sRLi                           |          |
        +-------+---------------------------------------+----------+
        | 26    | Incoming Shortwave Radiation from Snow| [kJ/m²]  |
        |       | Cover, sRSi                           |          |
        +-------+---------------------------------------+----------+
        | 27    | Error in Energy Balance, Uerr         | [J/m²]   |
        +-------+---------------------------------------+----------+
        | 28    | Intercepted SWE, IntSWE               | [cm]     |
        +-------+---------------------------------------+----------+
        | 29    | Sublimated Snow from Canopy, IntSub   | [cm]     |
        +-------+---------------------------------------+----------+
        | 30    | Unloaded Snow from Canopy, IntUnl     | [cm]     |
        +-------+---------------------------------------+----------+
        | 31    | Soil Moisture, top 10 cm, SoilMoist   | [ ]      |
        +-------+---------------------------------------+----------+
        | 32    | Root Zone Moisture, top 1 m, RootMoist| [ ]      |
        +-------+---------------------------------------+----------+
        | 33    | Canopy Storage, CanStorage            | [mm]     |
        +-------+---------------------------------------+----------+
        | 34    | Actual Evaporation, ActEvp            | [mm/hr]  |
        +-------+---------------------------------------+----------+
        | 35    | Evaporation from Bare Soil, EvpSoil   | [mm/hr]  |
        +-------+---------------------------------------+----------+
        | 36    | Total Evapotranspiration, ET          | [mm/hr]  |
        +-------+---------------------------------------+----------+
        | 37    | Ground Heat Flux, Gflux               | [W/m²]   |
        +-------+---------------------------------------+----------+
        | 38    | Sensible Heat Flux, Hflux             | [W/m²]   |
        +-------+---------------------------------------+----------+
        | 39    | Latent Heat Flux, Lflux               | [W/m²]   |
        +-------+---------------------------------------+----------+
        | 40    | Discharge, Qstrm                      | [m³/s]   |
        +-------+---------------------------------------+----------+
        | 41    | Channel Stage, Hlev                   | [m]      |
        +-------+---------------------------------------+----------+
        | 42    | Channel Flow Velocity, FlwVlc         | [m/s]    |
        +-------+---------------------------------------+----------+
        | 43    | Canopy Storage Parameter, CanStorParam| [mm]     |
        +-------+---------------------------------------+----------+
        | 44    | Interception Coeff., IntercepCoeff.   | [ ]      |
        +-------+---------------------------------------+----------+
        | 45    | Free Throughfall Coeff.- Rutter,      | [ ]      |
        |       | ThroughFall                           |          |
        +-------+---------------------------------------+----------+
        | 46    | Canopy Field Capacity – Rutter,       | [mm]     |
        |       | CanFieldCap                           |          |
        +-------+---------------------------------------+----------+
        | 47    | Drainage coefficient – Rutter,        | [mm/hr]  |
        |       | DrainCoeff                            |          |
        +-------+---------------------------------------+----------+
        | 48    | Drainage Expon. Param. – Rutter,      | [mm⁻¹]   |
        |       | DrainExpPar                           |          |
        +-------+---------------------------------------+----------+
        | 49    | Albedo, LandUseAlb                    | [ ]      |
        +-------+---------------------------------------+----------+
        | 50    | Vegetation Height , VegHeight         | [m]      |
        +-------+---------------------------------------+----------+
        | 51    | Optical Transmission Coeff.,          | [ ]      |
        |       | OptTransmCoeff                        |          |
        +-------+---------------------------------------+----------+
        | 52    | Canopy- Average Stomatal Resistance,  | [s/m]    |
        |       | StomRes                               |          |
        +-------+---------------------------------------+----------+
        | 53    | Vegetation Fraction, VegFraction      | [ ]      |
        +-------+---------------------------------------+----------+
        | 54    | Canopy Leaf Area Index, LeafAI        | [ ]      |
        +-------+---------------------------------------+----------+


Time-integrated Spatial Output Table
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  **Table 5.10** Content of *timestamp_00i file

        .. tabularcolumns:: |c|c|c|

        +-------+----------------------------------------+-------------+
        | Column| Description                            | Units       |
        +=======+========================================+=============+
        | 1     | Node Identification, ID                | [id]        |
        +-------+----------------------------------------+-------------+
        | 2     | Boundary Flag, BndCd                   | [ ]         |
        +-------+----------------------------------------+-------------+
        | 3     | Elevation, Z                           | [m]         |
        +-------+----------------------------------------+-------------+
        | 4     | Voronoi Area, VAr                      | [m²]        |
        +-------+----------------------------------------+-------------+
        | 5     | Contributing Area, CAr                 | [km²]       |
        +-------+----------------------------------------+-------------+
        | 6     | Curvature, Curv                        | [ ]         |
        +-------+----------------------------------------+-------------+
        | 7     | Flow Edge Length, EdgL                 | [m]         |
        +-------+----------------------------------------+-------------+
        | 8     | Tangent of Flow Edge Slope, tan(Slp)   | [ ]         |
        +-------+----------------------------------------+-------------+
        | 9     | Width of Voronoi Flow Window, FWidth   | [m]         |
        +-------+----------------------------------------+-------------+
        | 10    | Site Aspect as Angle from North, Aspect| [radian]    |
        +-------+----------------------------------------+-------------+
        | 11    | Sky View Factor, SV                    | [ ]         |
        +-------+----------------------------------------+-------------+
        | 12    | Land View Factor, LV                   | [ ]         |
        +-------+----------------------------------------+-------------+
        | 13    | Average Soil Moisture, top 10 cm, AvSM | [ ]         |
        +-------+----------------------------------------+-------------+
        | 14    | Average Root Zone Moisture, top 1 m,   | [ ]         |
        |       | AvRtM                                  |             |
        +-------+----------------------------------------+-------------+
        | 15    | Infiltration-excess Runoff Occurences, | [# of       |
        |       | HOccr                                  | TIMESTEP]   |
        +-------+----------------------------------------+-------------+
        | 16    | Infiltration-excess Runoff Average     | [mm/hr]     |
        |       | Rate, HRt                              |             |
        +-------+----------------------------------------+-------------+
        | 17    | Saturation-excess Runoff Occurences,   | [# of       |
        |       | SbOccr                                 | TIMESTEP]   |
        +-------+----------------------------------------+-------------+
        | 18    | Saturation-excess Runoff Average Rate, | [mm/hr]     |
        |       | SbRt                                   |             |
        +-------+----------------------------------------+-------------+
        | 19    | Perched Return Runoff Occurences,      | [# of       |
        |       | POccr                                  | TIMESTEP]   |
        +-------+----------------------------------------+-------------+
        | 20    | Perched Return Runoff Average Rate,    | [mm/hr]     |
        |       | PRt                                    |             |
        +-------+----------------------------------------+-------------+
        | 21    | Groundwater Exfiltration Runoff        | [# of       |
        |       | Occurences, SatOccr                    | GWSTEP]     |
        +-------+----------------------------------------+-------------+
        | 22    | Groundwater Exfiltration Runoff        | [mm/hr]     |
        |       | Average Rate, SatRt                    |             |
        +-------+----------------------------------------+-------------+
        | 23    | Soil Saturation Occurences, SoiSatOccr | [# of       |
        |       |                                        | TIMESTEP]   |
        +-------+----------------------------------------+-------------+
        | 24    | Recharge-Discharge Variable, RchDsch   | [m]         |
        +-------+----------------------------------------+-------------+
        | 25    | Average Evapotranspiration, AveET      | [mm/hr]     |
        +-------+----------------------------------------+-------------+
        | 26    | Evaporative Fraction, EvpFrct          | [ ]         |
        +-------+----------------------------------------+-------------+
        | 27    | Cumulative Latent Heat Flux from Snow  | [kJ/m²]     |
        |       | Cover, cLHF                            |             |
        +-------+----------------------------------------+-------------+
        | 28    | Cumulative Melt, cMelt                 | [cm]        |
        +-------+----------------------------------------+-------------+
        | 29    | Cumulative Sensible Heat Flux from     |  [kJ/m²]    |
        |       | Snow Cover, cSHF                       |             |
        +-------+----------------------------------------+-------------+
        | 30    | Cumulative Precipitation Heat Flux     | [kJ/m²]     |
        |       | from Snow Cover, cPHF                  |             |
        +-------+----------------------------------------+-------------+
        | 31    | Cumulative Incoming Longwave           | [kJ/m²]     |
        |       | Radiation from Snow Cover, cRLIn       |             |
        +-------+----------------------------------------+-------------+
        | 32    | Cumulative Outgoing Longwave           | [kJ/m²]     |
        |       | Radiation from Snow Cover, cRLo        |             |
        +-------+----------------------------------------+-------------+
        | 33    | Cumulative Incoming Shortwave          | [kJ/m²]     |
        |       | Radiation from Snow Cover, cRSIn       |             |
        +-------+----------------------------------------+-------------+
        | 34    | Cumulative Ground Heat Flux from       | [kJ/m²]     |
        |       | Snow Cover, cGHF                       |             |
        +-------+----------------------------------------+-------------+
        | 35    | Cumulative Energy Balance Error, cUErr | [kJ/m²]     |
        +-------+----------------------------------------+-------------+
        | 36    | Cumulative Hrs of Sun exposure,cHrsSun | [hr]        |
        +-------+----------------------------------------+-------------+
        | 37    | Cumulative Hours Snow Covered, cHrsSnow| [hr]        |
        +-------+----------------------------------------+-------------+
        | 38    | Longest Time of Continuous Snow        | [hr]        |
        |       | Cover, persTime                        |             |
        +-------+----------------------------------------+-------------+
        | 39    | Maximum Season SWE, peakWE             | [cm]        |
        +-------+----------------------------------------+-------------+
        | 40    | Simulation Hour of Maximum SWE,        | [hr]        |
        |       | peakTime                               |             |
        +-------+----------------------------------------+-------------+
        | 41    | Simulation Hr of Initial SWE, initTime | [hr]        |
        +-------+----------------------------------------+-------------+
        | 42    | Cumulative Sublimated Snow from        | [cm]        |
        |       | Canopy, cIntSub                                      |
        +-------+----------------------------------------+-------------+
        | 43    | Cumulative Unloaded Snow from Canopy,  | [cm]        |
        |       | cintUnl                                |             |
        +-------+----------------------------------------+-------------+
        | 44    | Av. Canopy Storage Parameter,          | [mm]        |
        |       | AvCanStorParam                         |             |
        +-------+----------------------------------------+-------------+
        | 45    | Av. Intercep. Coeff., AvIntercCoeff    | [ ]         |
        +-------+----------------------------------------+-------------+
        | 46    | Av. Free Throughfall Coeff.- Rutter,   | [ ]         |
        |       | AvTF                                   |             |
        +-------+----------------------------------------+-------------+
        | 47    | Av. Canopy Field Capac. – Rutter,      | [mm]        |
        |       | AvCanFieldCap                          |             |
        +-------+----------------------------------------+-------------+
        | 48    | Av. Drain. Coeff. – Rutter,            | [mm/hr]     |
        |       | AvDrainCoeff                           |             |
        +-------+----------------------------------------+-------------+
        | 49    | Av. Drain. Expon. Param. – Rutter,     | [mm⁻¹]      |
        |       | AvDrainExpPar                          |             |
        +-------+----------------------------------------+-------------+
        | 50    | Av. Albedo,AvLUAlb                     | [ ]         |
        +-------+----------------------------------------+-------------+
        | 51    | Av. Veg. Height , AvVegHeight          | [m]         |
        +-------+----------------------------------------+-------------+
        | 52    | Av. Optical Transm. Coeff., AvOTCoeff  | [ ]         |
        +-------+----------------------------------------+-------------+
        | 53    | Av. Canopy- Average Stom. Resist.,     | [s/m]       |
        |       | AvStomRes                              |             |
        +-------+----------------------------------------+-------------+
        | 54    | Av. Veg. Frac., AvVegFract             | [ ]         |
        +-------+----------------------------------------+-------------+
        | 55    | Av. Canopy Leaf Area Index, AvLeafAI   | [ ]         |
        +-------+----------------------------------------+-------------+

