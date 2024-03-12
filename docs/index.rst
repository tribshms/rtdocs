.. tRIBS Docs documentation master file, created by
   sphinx-quickstart on Thu Apr  9 15:33:28 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.


tRIBS Documentation
======================

**Foreword**


    The **tRIBS (TIN-based Real-Time Integrated Basin Simulator)
    Distributed Hydrologic Model** is a set of object-oriented,
    *C++* programs that allow for the construction and simulation
    of watershed hydrologic processes on a *Triangulated
    Irregular Network (TIN)*. This document is intended
    to serve as a user manual for the tRIBS model, including instructions
    on how to obtain, compile, set up and run tRIBS. In addition,
    an effort has been made to document the model software design using
    class and workflow diagrams. This guide assumes that the reader is
    familiar with the capabilities of the tRIBS model and its intended
    purposes. More information concerning the model can be obtained
    through the tRIBS Publication List.

    tRIBS is copyrighted 2024 by the tRIBS Developers.


        .. figure::  images/hydrocycle.png
          :width: 400
          :alt: tRIBS Distributed Hydrologic Model System
          :align:   center


    The **TIN-based Real-time Integrated Basin Simulator (tRIBS)**, is fully
    distributed model of hydrologic processes.


**What type of processes does tRIBS model?**


    The *tRIBS* model description can be found in [Ivanov_2004]_ and [Vivoni_2004]_.

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


.. [Ivanov_2004] Ivanov, V.Y., Vivoni, E.R., Bras, R.L. and Entekhabi, D. 2004. Catchment Hydrologic
   Response with a Fully-Distributed Triangulated Irregular Network Model. Water Resources Research. 40(11): W11102. https://doi.org/10.1029/2004WR003218
.. [Vivoni_2004] Vivoni, E.R., Ivanov, V.Y., Bras, R.L. and Entekhabi, D. 2004. Generation of Triangulated
   Irregular Networks based on Hydrological Similarity. Journal of Hydrologic Engineering, 9(4), 288–302. https://doi.org/10.1061/(ASCE)1084-0699(2004)9:4(288)

------------------------------------------------------------------------------------

..  toctree::
    :maxdepth: 3
    :numbered:
    :caption: TABLE OF CONTENTS

    man/Introduction.rst
    man/References.rst
    man/Model_Design.rst
    man/Model_Input_Formats.rst
    man/Model_Execution.rst
    man/Terrain_Analysis_for_Model_Setup.rst
    man/Hydrometeorological_Data_Processing.rst
    man/Contacts_Further_Readings.rst



..  toctree:
    :maxdepth:  2

  * :ref:`genindex`
  * :ref:`modindex`
  * :ref:`search`


----------------------------------------------------------

    *Last update:* E. Vivoni, 02/14/2024
