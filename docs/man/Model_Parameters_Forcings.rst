Model Parameters and Forcings
==============================

A tRIBS model application consists of a series of Voronoi polygons that discretely represent a watershed. At each TIN node associated with a Voronoi polygon, a set of governing equations describing the hydrologic and energy processes are computed. To solve these equations for each location, spatially distributed data describing surface topography, soils, vegetation and hydrometeorology are required, which can be obtained from field measurements, remote sensing data interpretation or model output such as numerical weather models. 

Model Parameters
------------------

In the case of soil and vegetation cover, different sources of data can be used to assign uniform or spatially-explicit parameter values required for the governing equations. The soils parameters listed in **Table 3.1** are necessary for carrying out the vertical infiltration and lateral flow redistribution, as well computing the soil heat budget within the sloped, heterogeneous, anisotropic soil columns assumed at each Voronoi polygon. Note that the parameter requirements vary with the specified hydrologic processes selected during a model run. For example, the surface heat parameters are only required if the radiation balance is computed.

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
        |  *Cs*              |   Soil Heat Capacity                          |  [J/m3K]           |
        +--------------------+-----------------------------------------------+--------------------+

Model Forcings
----------------
