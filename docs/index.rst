tRIBS Distributed Hydrologic Modeling System
=============================================

The TIN-based Real-Time Integrated Basin Simulator is a fully-distributed, continuous hydrologic model operating on a Triangulated Irregular Network (TIN). This document is intended to serve as a user manual for the tRIBS model, including instructions on how to obtain, compile, set up and run tRIBS using GitHub. In addition, an effort has been made to document the model software design using class and workflow diagrams. Additional information can be obtained through the `tRIBS References <https://tribshms.readthedocs.io/en/latest/man/References.html/>`_.

        .. figure::  images/hydrocycle.png
          :width: 600
          :alt: tRIBS Distributed Hydrologic Modeling System
          :align:   center

The tRIBS model in this release under the `MIT license <https://opensource.org/license/mit>`_ includes:

Hydrologic Processes:

        - Soil moisture infiltration fronts and their vertical redistribution.
        - Coupled dynamics of the vadose and saturated zones with a dynamic water table.
        - Topography-driven lateral fluxes in the vadose and saturated zones.
        - Radiation and energy balance components on complex terrain. 
        - Single layer snowpack accumulation, ablation and melt on complex terrain.
        - Rainfall and snow interception on vegetation canopies.
        - Evaporation of intercepted rainfall, soil evaporation and plant transpiration.
        - Hydrologic hillslope routing, hydraulic routing in channels, level-pool reservoir routing.

Computational Processes:

        - Soil moisture infiltration fronts and their vertical redistribution.
        - Coupled dynamics of the vadose and saturated zones with a dynamic water table.
        - Topography-driven lateral fluxes in the vadose and saturated zones.
        - Radiation and energy balance components on complex terrain. 
        - Single layer snowpack accumulation, ablation and melt on complex terrain.
        - Rainfall and snow interception on vegetation canopies.
        - Evaporation of intercepted rainfall, soil evaporation and plant transpiration.
        - Hydrologic hillslope routing, hydraulic routing in channels, level-pool reservoir routing.

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
