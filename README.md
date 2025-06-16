# stac2cube 🛰️🧊<br> Spatio-Temporal Asset Catalogs To Analysis-Ready Data Cubes
**stac2cube** converts STAC catalogs into Analysis-Ready Data Cubes for efficient Earth Observation (EO) processing. This tool is designed to work both on any local-machine and HPC system by _terrabyte_. We recommend using *MobaXTerm* for accessing the _terrabyte_ login node and editing files.

UPDATE: The repository will be published once the preprint version of the paper is available. 

## Feature Overview
- **stac2cube.get_stac_layers**
    - Collects images from STAC catalogs for the selected mission based on users parameters.
    - Automatically preprocess spectral/radar values based on specifications of the selected mission.
    - Generates multi-dimensional data cubes, suitable for time-series.
    - The data cubes can be updated anytime without generating them from the scratch.
    - Available missions: **Sentinel-2 L2A, Sentinel-2 L1C, Sentinel-1 RTC, Landsat C2 L2, COP DEM Glo-30 (single time)**
- **stac2cube.get_cloud_layers**
    - Collects images from Sentinel-2 L1C to automatically apply s2cloudless cloud probability algorithm on data cube structure.
    - The result contains cloud probability maps and user defined binary cloud mask layers.
    - When selected, clouds from the generated data cube are automatically masked out.
    - Can be updated anytime.
- **stac2cube.coregister_scenes**
    - Applies coregistration algorithm on Sentinel-2 data cubes.
    - AROSICS package provides the coregistration algorithm<br>
    Daniel Scheffler. (2017, July 3). AROSICS: An Automated and Robust Open-Source Image Co-Registration Software for Multi-Sensor Satellite Data (Version 0.12.1). Zenodo. https://doi.org/10.5281/zenodo.3742909
    - Fix the global X/Y shift between consecutive Sentinel-2 items.
 - **stac2cube.super_resolution**
    - Applies super-resolution algorithm on both 10-meters and 20-meters bands and results in 2.5-meters resolution stacks.
 - **Updating Mechanism**
  - If the computer is not competent enough to produce long-term data cubes (e.g., 8 years), then the user can activate updating mechanism and collect the data with different time ranges.
  - Example: request data for 2018 only, then request 2019 with update mechanism and it will combine the both data. 
    


## Installation

The installation can be done by using either pip or UV. All the dependencies are defined. It's highly recommended to have a either fresh Mamba or Conda environment for the installation.

---



### Example installation

#### a) Set Up Micromamba Environment
    $ module load micromamba
    $ micromamba create -n stac2cube
    $ micromamba shell init --shell bash --root-prefix=~/micromamba
    $ source ~/.bashrc
    $ micromamba activate stac2cube

#### b) Install stac2cube via PIP or UV
Ensure that stac2cube env is activated.

    $ micromamba install pip
You should be on the folder where `pyproject.toml` is located.

    $ cd <path_to_stac2cube_parent_folder>

Stable mode:<br>

    $ pip install .

Editable mode (for development):<br>

    $ pip install -e .

---


## Access and Licensing Details for STAC Catalogs

### Access to STAC Catalogs

- **Important**: _terrabyte_ STAC catalogs can be only computed when working on a _terrabyte_ environment.<br>
- However, stac2cube package is designed to work on both local-machine without _terrabyte_ connection and within _terrabyte_ HPC environment.<br>
- Therefore, a silent parameter will enable _terrabyte_ STAC catalogs when a SLURM job is activated.<br>
- The default set-up (_terrabyte_ disabled) will feature STAC catalogs that provide "open-access data" (**not** open-source).<br>
- Thus, note that stac2cube package **can not** guarantee unlimited access to these open-access data catalogs in the future!

### STAC Catalog Licenses

| Provider   | Service           | STAC API                                        | License                                                                                      | Open-Access | Open-Source |
|------------|-------------------|-------------------------------------------------|----------------------------------------------------------------------------------------------|-------------|-------------|
| DLR        | terrabyte         | https://stac.terrabyte.lrz.de/public/api/       | MIT License Copyright (c) 2024 Deutsches Zentrum für Luft- und Raumfahrt e.V.                | No          | No          |
| Element 84 | Earth Search      | https://earth-search.aws.element84.com/v1/      | Apache License 2.0                                                                           | Yes         | Yes         |
| Microsoft  | Planetary Computer| https://planetarycomputer.microsoft.com/api/    | MIT License Copyright (c) Microsoft Corporation.                                             | Yes         | No          |

### Why use terrabyte then?

Why do _terraybte_ users collect data from _terrabyte_ STAC catalog instead of  open-source Earth Search?

- The data by Element 84 is stored in AWS S3 services. 
- The data by DLR is stored in the servers of The Leibniz Supercomputing Centre (LRZ) in Garching/Munich.
- When working on a _terrabyte_ environment, the data query is returned from same server instead of connecting to AWS. <br><br>

#### **Example**: Query for Sentinel-2 L2A: 
- daterange: ["2017-01-01", "2025-03-28"]
- polygon: Nord Hubland/Würzburg/Germany<br>

| Service          | Returned Date  | Processing Time (s)|
|------------------|----------------|--------------------|
|terrabyte         | 1134           | 24.0               |
|Earth Search      | 1038           | 140.5              |
|Planetary Computer| 1133           | 12.2               |

- Indicates* that queries are faster when working on a _terrabyte_ environment.
- Most importantly, this indicates that Earth Search archive has some missing scenes.
- Also Earth Search STAC definitions are sometimes faulty (especially Sentinel-2 L1C) and as a developer of this package, I prefer working with _terrabyte_ API.

\* Queries are iterated 10 times per each service and the average time per run is calculated (timeit module).

## How to cite:
paper to be published <3

<br><br>
Contact: https://www.geographie.uni-wuerzburg.de/en/earthobservation/staff/baturalp-arisoy/
