tRIBS Distributed Hydrologic Modeling System
=============================================

The TIN-based Real-Time Integrated Basin Simulator is a fully-distributed, continuous hydrologic model operating on a Triangulated Irregular Network (TIN). This document is intended to serve as a user manual for the tRIBS model, including instructions on how to obtain, compile, set up and run tRIBS using GitHub. In addition, an effort has been made to document the model software design using class and workflow diagrams. Additional information can be obtained through the tRIBS Model References https://tribshms.readthedocs.io/en/latest/man/References.html.

tRIBS is copyrighted 2024 by the tRIBS Developers.


        .. figure::  images/hydrocycle.png
          :width: 600
          :alt: tRIBS Distributed Hydrologic Modeling System
          :align:   center

We mention a few *tRIBS* processes it models:

        - Couple the vadose and saturated zones with a dynamic water table.
        - Soil moisture infiltration fronts and their redistribution.
        - Topography-driven lateral fluxes in the vadose and groundtable zones.
        - Radiation and energy balance components on complex terrain. 
        - Single layer snowpack accumulation, ablation and melt.
        - Rainfall and snow interception on vegetation canopies.
        - Evaporation of intercepted rainfall, soil evaporation and plant transpiration.
        - Hydrologic in hillslopes and hydraulic routing in channels.
        - Level-pool reservoir routing.

------------------------------------------------------------------------------------

.. toctree::
    HOME <self>
    :hidden:

.. toctree::
    :maxdepth: 3
    :numbered:
    :caption: ABOUT

    man/Introduction
    man/References
    man/Contacts

.. toctree::
    :maxdepth: 3
    :numbered:
    :caption: DOCUMENTATION

    man/Model_Design
    man/Model_Input_Formats
    man/Model_Parameters_Forcings
    man/Model_Execution

.. toctree::
    :maxdepth: 3
    :numbered:
    :caption: DEVELOPMENT

    man/Development
    man/Contributing
    man/ReleaseNotes
    man/UsingGit

.. toctree::
    :maxdepth: 3
    :numbered:
    :caption: DOWNLOADS

    man/Docker
    man/BenchMarks

.. toctree::
    :maxdepth:  2
