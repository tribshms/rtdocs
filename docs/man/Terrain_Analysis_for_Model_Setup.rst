
Terrain Analysis for Model Setup
======================================

        An important task in the application of TIN-based distributed hydrologic models is the automatic creation of a TIN Terrain Model that is tailored specifically for watershed hydrology. Although the creation of a TIN terrain model through existing software packages is common, ensuring that a TIN model's resolution, features and representation are adequate for the purpose of TIN-based hydrologic modeling is a critical exercise that is often ignored in the modeling process. For example, various TIN-based watershed models apply a set constant resolution triangles to represent the watershed, a feature which does not take advantage of the topographic diversity in the watershed terrain and the potential for multiple resolutions. In addition, the creation of the TIN must also conform to various linear features present in the topography, which may include the stream network and the watershed boundary. These features ensure that the generation of the TIN Terrain model is "hydrologically" significant for distributed modeling purposes. Although various software packages exist for the creation of a TIN terrain model (*e.g.* Arc/Info, IDL and other finite element mesh generators), the use of Arc/Info is recommended due to its easier integration with the data sources required to create the hydrologic TIN model. For tRIBS, a set of Arc/Info scripts written in AML (Arc Macro Language) have been developed to take the various inputs describing the watershed and create a TIN which can ultimately be inputted into the tRIBS Model using options 5 or 6 of the keyword *OPTMESHINPUT*.

Digital Elevation Models and Stream Networks
--------------------------------------------------

        The basic building block of the TIN mesh developed for watersheds with complex topography is the raster Digital Elevation Model (DEM). Topographic representation through Digital Elevation Models (DEMs) has increased our capability of modeling the surface and subsurface hydrologic processes that govern the rainfall-runoff conversion. High resolution DEMs are readily available from the US Geological Survey at various spatial resolutions. Typical data sets are derived from printed contour maps after procedures for digitizing and interpolating onto a regular grid. The highest resolution product from the USGS are the 0.25, 1.25, 2.5 and 7.5 minute DEM products (1, 5, 10 and 30 meters ground resolution respectively). Higher resolution DEMs would require field surveying techniques, aerial photogrammetric analysis or LIDAR measurements, which there is a number of limited datasets.

        Recommended sources of DEM data include:

            * `USDA National Resources Conservation Service: Geospatial Data Gateway <https://datagateway.nrcs.usda.gov/>`_
            * `GeoPlatform.gov - Federal Geographic Data Committee (FGDC) <https://www.geoplatform.gov/>`_
            * `USGS 3D Elevation Program (LIDAR) <https://www.usgs.gov/core-science-systems/ngp/3dep/>`_

        From the `USDA NRCS Geospatial Data Gateway <https://datagateway.nrcs.usda.gov/>`_ you can download the following datasets and more:

            * Watershed Boundary Datasets (8, 10 and 12 Digit)
            * National Land Cover Datasets by State
            * Gridded Soil Survey Geographic (gSSURGO) by State
            * Climate (Monthly and Annual Normals for Precipitation and Max & Min Temperatures)

        Each data distributor will provide the topographic data is a slightly different format or projection. ArcView GIS or Arc/Info can be used to visualize the elevation data and perform many of the required hydrologic computations based on the topography. The DEM should be hydrologically corrected by removing pits or sinks, thus ensuring a continuous water flow over the watershed. Further DEM processing includes: (1) defining the cell slopes, (2) defining the flow directions, and (3) calculating the flow accumulations. Although the tRIBS model does not require any of these inputs directly, they are an important part of deriving the watershed boundary and the stream network. The stream network is computed from the flow accumulations grid by setting a threshold value on the definition of a stream cell. By comparing the drainage density between the DEM-based stream network to the drainage density of the blue-line vectors from a USGS Quad Sheet, one can determine an appropriate threshold parameter. Although not directly used in the modeling process, the hydrography vector data is an important source of information for obtaining the correct DEM-based stream network. Recommended sources of hydrography data include:

            * `USDA National Resources Conservation Service: Geospatial Data Gateway <https://datagateway.nrcs.usda.gov/>`_
            * `GeoPlatform.gov - Federal Geographic Data Committee (FGDC) <https://www.geoplatform.gov/>`_
            * `USGS National Hydrography <https://www.usgs.gov/core-science-systems/ngp/national-hydrography/>`_

        Having the appropriate stream network, the derivation of the watershed boundary can be performed again within the ArcView GIS or Arc/Info environments. Specific details on the methods used to derive watershed descriptors is omitted from this document for the sake of brevity. The user is referred to a set of AML DEM processing scripts included in the TIAP software page as a starting point for watershed data analysis using DEMs.

Triangulated Irregular Network Model
-------------------------------------------

        A "hydrologically" significant TIN terrain representation consists of a set of triangular elements that together represent the important hydrologic features within a complex watershed. The multiple resolution capability of TIN models is a principal advantage to using a TIN representation. The use of multiple resolutions allows flat terrain to be represented with a coarser resolution as compared to a rugged landscape, thus having significant computational savings in regions of low-gradients within the watershed. Obtaining a multiple resolution TIN is performed by sampling the Digital Elevation Model at points considered to be topographically important at a global scale. Without entering into details, this implies that only points whose elevation is critical to the proper TIN representation, as compared to the original DEM, are kept within the TIN Terrain Model. All other points are discarded. This DEM sampling procedure permits high resolution to be used for rugged terrain and lower resolution for flat landscapes. Details concerning this procedure and the objective measures used to determine the appropriate sampling are presented elsewhere in the tRIBS documentation.

        In addition to sampling the DEM to obtain a multiple resolution TIN, the generation of an appropriate terrain model for hydrological purposes should include the representation of linear features within the watershed. Two types of linear features are of critical importance: the watershed boundary and the watershed stream network. Ensuring that the TIN conforms to these linear features will result in capturing the appropriate watershed area and converging the terrain to the water bodies present inside the watershed. Techniques for incorporating the watershed boundary and the stream network have been developed and applied to a variety of watersheds. Various considerations that arise when computing the dual diagram or Voronoi Polygon Network (VPN) from the TIN have also been incorporated into the creation of the mesh. Again, more details are presented elsewhere in the tRIBS documentation.

        The generation of a multiple resolution TIN mesh using the linear watershed features and the DEM sampling does not necessarily take into account the fact that a "hydrologically" significant TIN should have increased resolution near the stream network. Additional resolution near the channel network and within the floodplain is important when a hydrologic model considers the contraction and expansion of variable source areas for runoff. With poor spatial discretization within a floodplain, the runoff producing areas during a storm event are not well captured. For this reason, a set of procedures for identifying a floodplain and deriving a floodplain-scale, constant resolution TIN mesh from a watershed DEM have been developed. The floodplain TIN is then used as an input to the watershed-scale TIN to obtain a finer discretization of the near-channel source areas.

        A manuscript describing the procedures for the generation of a TIN Terrain model with the hydrologic considerations in mind is in preparation with a set of example case studies and objective measures to guide the user in the proper selection of procedure parameters. The user is also referred to a set of AML TIN generation scripts included in the TIAP software page as a starting point for understanding the methods used to arrive at a "hydrologically" significant TIN Terrain Model.

--------------------------------------------------------

    *Last Update:* 02/25/2021  C. Lizarraga
