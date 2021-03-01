
Hydrometeorological Data Processing
=========================================

        Hydrometeorological data is a requirement in the tRIBS Distributed Hydrologic Model whenever the application calls for continuous hydrologic simulations over the storm-interstorm cycle or even during event-based modeling where the rainfall forcing is required. In either case, the appropriate hydrometeorological data must be obtained from available sources and preprocessed into a format amenable for input into the tRIBS Model. The processing of radar rainfall data and weather station data are briefly described in the following sections. Additional model capabilities, including rain gauge precipitation input and grid meteorological input, are omitted for brevity but have also been incorporated into the tRIBS model. Further details are available upon submitting a request to the authors.

NEXRAD Stage III and WSI Rainfall Radar Data
---------------------------------------------------

        Various sources of rainfall data for hydrologic modeling are available through the National Weather Service (NWS) and other state, local or federal agencies. Rain gauges, the traditional sources of rainfall data, measure the amount of precipitation falling inside a calibrated bucket at a single point. Measurements from rain gauges are typically sparse in their spatial coverage (1 rain gauge for every 1-50 square kilometers) but reasonably accurate a single point. The use of rain gauges for distributed hydrologic modeling requires an interpolation of rainfall values over the watershed (using Thiessen polygons) so that each model computational element is assigned the value of its nearest neighboring gauge. An improvement upon the use of rainfall gauges can be obtained by using the spatially-variable rainfall fields available from radar sensors. In the United States, a significant effort has been made to sample rainfall fields through a nationwide network of weather radars. The NEXRAD system provides rainfall coverage for the continental United States by using approximately 160 individual radars that measure the three dimensional reflectivity field every six minutes at a spatial resolution of 2 kilometers by 2 kilometers.

        For hydrologic modeling purposes, the NEXRAD reflectivity data from the individual radars have to be processed to obtain a mosaiced rainfall product that has been adjusted by raingauge values (mean-field bias adjustment). Various sources of NEXRAD data are available from different public and private sector distributors:

            * NOAA National Centers for Environmental Information (`Radar Data <https://www.ncdc.noaa.gov/data-access/radar-data/>`_)
            * National Weather Service (`Radar <https://radar.weather.gov/#/>`_)
            * `DTN Weather Services <https://www.dtn.com/>`_
            * `IBM Weather <https://www.ibm.com/weather/>`_
            * `Weather Underground <https://www.wunderground.com/>`_
            * `The Weather Channel (IBM) <https://weather.com/>`_

        Each distributor performs a different set of correction and conversion algorithms to obtain accurate rainfall grids over the entire United States. The Stage III data is freely available from the NWS and well documented (e.g. Young et. al, 2000) [Young_2000]_. The NEXRAD data is organized by monthly intervals and by River Forecasting Center (RFC) so that a mosaic for each RFC (e.g. Arkansas-Red River Basin) is available at hourly intervals on a 4 km by 4 km grid. The other two data sources are available from the distributors for a fee. More information about the data distribution and processing algorithms must be obtained from the corporations themselves, as they are proprietary.

        The tRIBS Distributed Hydrologic Model has been used with both the NWS Stage III data and the WSI Corporation radar rainfall data. The NWS Stage III data was downloaded from the archived source as a collection of tar-gzipped binary files. A utility called ``read_xmrg`` is available from the NWS to read the binary file and convert them to ASCII grids (*e.g.* **Figure 3.1**). Our work has used a previous version of this code named ``xmrgtoascster.c``  to convert the binary format into a grid product in Polar Stereographic projection. Once in an amenable ASCII format, a set of Arc/Info Arc Macro Language (AML) scripts have been developed to convert the Polar Stereographic grids for the entire RFC into a grid for the particular watershed in a specific local coordinate system (in *cm/hr*) . The AML routine (``radarconvert.aml``) also perform the unpacking of the tar-gzip ball, calling of the xmrgtoascster program, as well as the projection and clipping of the rainfall grid. A useful paper describing the NEXRAD projections and coordinate system is Reed and Maidment (1998).  The former Weather Services International (WSI), now IBM Weather, radar rainfall data is not freely available to the public. Through our project collaborators, Atmospheric and Environmental Research, Inc. (AER), we obtain rainfall ASCII grids in decimal degrees (in *mm/hr*) for the particular region of interest. The data are obtained at AER through a satellite feed, archived and preprocessed from the binary stream to an ASCII grid file structure. A set of AMLs have been developed for projecting, clipping and preparing the WSI data into the tRIBS grid input format (``wsiconvert.aml``). The WSI data are available at a 15 minute temporal resolution and up to a 2 km by 2 km grid spatial resolution. Comparisons between the WSI and Stage III radar products have shown that the WSI have an improved spatial resolution and remove many of the artifacts in the interpolation scheme used for the Stage III data. A report on this subject is in preparation.

        In terms of the tRIBS Model, the use of the two radar rainfall sources is controlled by the keyword *RAINSOURCE* ( *= 0*, NEXRAD Stage III, *= 1*, WSI Product ) in the tRIBS Model Input File. The *RAINFILE* and *RAINEXTENSION* keywords are used to specify the pathname and basename, and the extension of the rainfall input grids. In addition, the *RAININTRVL* and *RAINSEARCH* keywords are used to specify the temporal spacing of the input rainfall grids and the maximum time over which the model will search for a new input rainfall grid. Finally, the tRIBS Model computes a basin-averaged rainfall hyetograph from the grid input rainfall fields, which is outputted as the second column of the hydrograph (``*.mdf``) file.


Operational RFC Meteorological Data
-----------------------------------------

        Various sources are available for hydrometeorological data through state, federal and local government agencies. In the United States, however, the distribution of weather data is neither standardized or centralized. Weather data, in general, is available in a number of different formats, sampling periods, number of parameters, record lengths, aerial extents and accuracy. Finding an appropriate source for input into a hydrologic model is not straightforward, even more so if you consider that a change in the any of the above properties alters the input code, data checks and calculations possible within the model. In the search for hydrometeorological data, the reader is referred to the following data sources:

            * NCDC Cooperative Summary of the Day (`NOAA NCDC Site <https://www.ncdc.noaa.gov/>`_ )
            * Hourly US Weather Observations (`NOAA NCEI <https://www.ncdc.noaa.gov/data-access/>`_ )
            * NOAA National Centers for Environmental Information (`Local Climatological Data <https://www.ncdc.noaa.gov/cdo-web/datatools/lcd/>`_)
            * National Weather Service Advanced Hydrologic Prediction Service (`River Forecast Centers <https://water.weather.gov/ahps/rfc/rfc.php/>`_)
            * `Synoptic Data <https://download.synopticdata.com/>`_
            * State Mesonet Weather Network Data (e.g. `Oklahoma Mesonet <http://www.mesonet.org/index.php/>`_, `Arizona AZMET <https://cals.arizona.edu/azmet/index.html/>`_, `Colorado CoAgMet <http://www.coagmet.colostate.edu/>`_, `California CIMIS <https://cimis.water.ca.gov/>`_, and others. )


        Other sources of data from meteorological stations and/or interpolations of weather observations are available from other government agencies or research institutions. For the tRIBS, the use of the Operational RFC Point Data was selected as the weather data source of choice. Among the reasons for this selection were the fact that the data is available freely for the entire United States at an hourly interval and includes all the necessary weather parameters necessary for a complete description of the atmospheric state. In addition, the Operational RFC Point Data is a collection of data provided to the River Forecasting Centers (RFC) from different data providers, including state-wide mesonets, for flood forecasting purposes. As such, this data source ensures appropriate compatibility with the weather information available real-time at the RFC centers. Finally, the data availability only lags behind the current date by one year, allowing recent events to be modeled without waiting for data releases that occur 2-3 years after the date of observations (e.g. CD-ROMs).

        A drawback of the Operational RFC Point Data is the format in which it is provided (*i.e.* Informix database tables). The data files are extremely large (> 200 MB) since they provide the hourly weather data for all the stations within an RFC (*i.e.* Arkansas-Red River Basin) for an entire month. In order to deal with this data in a more efficient way within tRIBS, a preprocessor class called ``tHydroMetConvert`` was created. This class reads in the weather data from the RFC Point Data Files and produces the tRIBS HydroMet Station and HydroMet Data files (``*.sdf`` and ``*.mdf``, respectively) necessary for use with the evapotranspiration class (``tEvapoTrans``).

        In order to use ``tHydroMetConvert``, a separate text file must be created (``*.mdi``) that specifies the stations, parameters, file pathnames and options. An ``*.mdi`` file (Meteorological Data Input) has a simple structure, as shown in **Figure 6.1** and an example can be obtained from the Sample Application available from the tRIBS Downloads Page.

            **Figure 6.1 Meteorological Data Input File Structure**

            .. tabularcolumns |l|

            +--------------------------------------+
            | *#Files*  *#Stations*  *#Parameters* |
            +--------------------------------------+
            |   *MERGE/SEPARATE Option*            |
            +--------------------------------------+
            |   *Path Name of Data File 1*         |
            +--------------------------------------+
            |   *Path Name of Location File 1*     |
            +--------------------------------------+
            |  *Path Name of Data File 2*          |
            +--------------------------------------+
            |  *Path Name of Location File 2*      |
            +--------------------------------------+
            |  ...                                 |
            +--------------------------------------+
            |  *Name of Station 1*                 |
            +--------------------------------------+
            |  *Name of Station 2*                 |
            +--------------------------------------+
            |  ...                                 |
            +--------------------------------------+
            |  *Name of Parameter 1*  *Station #*  |
            +--------------------------------------+
            |  *Name of Parameter 1*  *Station #*  |
            +--------------------------------------+
            |  ...                                 |
            +--------------------------------------+

        Some explanation of these various components should shed light upon the use of the ``*.mdi`` files. The first line simply states the number of RFC Point Data Files to be read, each containing a month of data, followed by the number of weather observation stations to be read and the total number of weather parameters to be read. Identifying the appropriate station names and the appropriate parameter names is done by inspecting the RFC Point Data File and locating the stations according to their proximity to the watershed of interest. Information about each station, including the latitude and longitude, is found in the RFC Point Location File. The *MERGE* or *SEPARATE* key word is important since it will specify whether or not the data from various stations will be merged into one tRIBS HydroMet Data File (``*.mdf``) or if the station data will be kept in separate files. This option is useful if the user has identified more than one station near to the watershed of interest that have complementary data (*e.g.* station 1 has data missing in station 2). The pathname lines are used to specify the location of the Data File and the Location Files that need to be downloaded from the Operational RFC Site. The name of the station lines correspond to the actual name given to each site within the RFC file. The proximity of the chosen stations should be ascertained by using the location of station and a watershed map within a GIS program. The name of the parameter lines include both the actual parameter name (*e.g. TA, XC, TD*) and the number of the station from which it will be extracted (if *MERGE* option used). Further details can be obtained from the ``tHydroMetConvert`` class source code. Note that the use of the hydrometeorological data processor is controlled in the tRIBS Model Input File through the keywords *METDATAOPTION* (*= 0*, inactive, *= 1*, point data, *= 2*, grid data) and *CONVERTDATA* (*= 0*, inactive, *= 1*, preprocess weather data).


---------------------------------------------------------------------------------

          *Last Update:* 02/28/2021  C. Lizarraga
