

Introduction
==================

    A number of distributed hydrologic models of varying degrees of complexity have been developed to simulate the land phase of the hydrologic cycle. Physically-based, distributed models solve parameterized equations for fluid flow in both space and time, leading to the implementation of numeric codes. Most models of this type, however, have large execution times, diminishing their capacity for model calibration and validation and ensemble forecasting. One of the major factors leading to the large execution times is the inefficiency in the representation of terrain inherent in a raster grid, the preferred mesh structure in most distributed hydrologic models. This document elaborates on the use and application of a distributed model resolves these difficulties by representing the terrain through triangulated irregular networks (TINs).

    The TIN-based Real-time Integrated Basin Simulator (tRIBS) is a collection of C++ classes designed for distributed hydrologic modeling at small to mid-size catchment scales. The object-oriented software design offers several advantages. In particular, by grouping data and functions operating on these variables into distinct classes, it becomes possible to separate the various hydrologic processes operating on the TIN mesh from the procedures for creating the mesh itself (Tucker *et. al*, 2001). The object-oriented approach also allows for code modularity in such a way as to facilitate model development through code reuse and integration or substitution of new process modules. Such a strategy permitted the development of the tRIBS model from the CHILD modeling framework (Tucker *et. al*, 1999). Hydrologic modules from the RIBS model (Garrote and Bras, 1995) and new hydrologic process models were incorporated into the CHILD framework as separate classes.

    The tRIBS documentation is intended to serve as an introduction for new users. It covers topic areas related to the design, setup, execution and proper usage of the model, in addition to discussing limitations. An exhaustive explanation of each topic area is avoided to maintain this document as a short reference manual. tRIBS is provided here as an open source tool for the use in the resesarch and practitioner communities. It is the responsibility of the user to make sure that the input file formats are correct and that parameter values are reasonable. This document provides sufficient information for the proper construction and execution of tRIBS.

----------------------------------------------------

    *Last update:* 03/11/2024 E. Vivoni
