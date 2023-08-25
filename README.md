# spawning-dataset
Code for the generation and processing of fish spawning regions

## Organization
 - `code/` includes R scripts for both preparation and display.
 - `inputs/` contains data drawn from AquaMaps/FishBase or constructed
   by hand.
 - `outputs/` contains the shapefile dataset and other results.

## Reproduction process

First, clone the repository, and set the working directory in R to the
root of the repository. All R code assumes that this is the working
directory.
   
### To reproduce the Spawning ProCreator spreadsheet

The Spawning ProCreator spreadsheet, `Master Spawning ProCreator.csv`,
describes how each spwning region should be constructed. Improvements
to these constructions can be made by editing the spreadsheet, which
does not require reproducing it. However, if the underlying spawning
information from FishBase and SCRFA is extended, the spreadsheet
should be recreated and new rows should be merged with the existing
file.

To reproduce the spreadsheet, as it was prior to adding the
information that describes how regions should be constructed, follow
these steps:

1. You may optionally regenerate the mapping of EEZs to FAO
   regions. This is produced by `prelim/fao2eez/mapping.R`. After
   running it, move the resulting `outputs/fao2eez.csv` to
   `inputs/fao2eez.csv`.
   
2. Regenerate the `input/specieseez.csv` file if the `inputs/Region
FAO EEZ matching-DO NOT EDIT IN EXCEL.csv` has changed. This second
file describes the FAO regions corresponding to multinational
descriptions in the spawnings dataset. To regenerate it, run
`prelim/fao2eez/species2eez.R`, which produces `output/specieseez.csv`
and move this file to `inputs/specieseez.csv`.

3. Merge the FishBase and SCRFA spawning records: Run the
   `code/prelim/spawning-merge.R` script. This generates a file
   `outputs/spawning-records.csv` which should be moved to
   `inputs/spawning-records.csv` for the next step.

4. Geocode spawning region names: Set `source = 'arcgis'` in
   `code/prelim/geocode.py` and run the script; then set `source =
   'geonames'` and run the script again. This script produces geocoded
   result files names `localities-arcgis.csv` and
   `localities-geonames.csv`. Move these to the `inputs/` directory.

5. Run the `code/prelim/spawning-geoprep.R` script, which constructs
   the raw Spawning ProCreator spreadsheet into
   `outputs/master.csv`. This can then be imported into Excel or
   Google Sheets for filling out the Verdict column.

6. When the Spawning ProCreator spreadsheet is prepared (the Verdict
   and other columns are manually entered), save the result as a CSV
   file at `inputs/Master Spawning ProCreator.csv`.

### To reproduce the GO-FISH shapefile

The `code/generate/read.R` functions translate information from the
`inputs/Master Spawning ProCreator.csv` spreadsheet into shapefile
regions.

The `code/generate/publicdataset.R` script produces a shapefile that
includes all available spawning regions, intersected with suitability
information. It produces `outputs/GO-FISH.shp` and
`outputs/GO-FISH.csv`, the latter of which corresponds to the polygon
attributes in the shapefile.

The `code/generate/stats.R` script generates `spawning-species.csv`
which provides information about each species and spawning region
provided in the spawning regions dataset; `sau-species.csv` which
provides information about each species in the SAU dataset; and
`sumstats.csv` which provides a summary of this information by
continent and fish group.

To regenerate the public dataset, first run
`code/generate/publicdataset.R` and then `code/generate/stats.R`.

### To regnerate the figures

 - `maps.R` generates spawning maps across the whole year, by season,
   or by month.
   
 - `Fig2and3.R` generates the other figures in the paper.

## License

<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

This repository contains data extracted from public datasets, as described below:

 - FishBase: The data under `inputs/spawning` and summarized in `inputs/spawning-records.csv` is dervied from FishBase (CC-BY-NC 3.0).

   Froese, R. and D. Pauly. Editors. 2023. FishBase. World Wide Web electronic publication. www.fishbase.org, version (02/2023).

 - Sea Around Us: The data under `inputs/saudata` is derived from Sea Around Us (CC-BY-NC 4.0)

   Pauly D., Zeller D., Palomares M.L.D. (Editors), 2020. Sea Around Us Concepts, Design and Data (seaaroundus.org).
   
 - AquaMaps: The data under `inputs/ranges` is derived from AquaMaps (CC-BY-NC 3.0)

   Kaschner, K., Kesner-Reyes, K., Garilao, C., Segschneider, J., Rius-Barile, J. Rees, T., & Froese, R. (2019, October). AquaMaps: Predicted range maps for aquatic species. Retrieved from https://www.aquamaps.org.
   
 - Natural Earth: The shapefiles under `inputs/shapefiles/ne_10m_admin_0_countries` and `inputs/shapefiles/ne_50m_coastline` where made by Natural Earth (public domain): https://www.naturalearthdata.com/downloads/
 
 - SCRFA: The file `inputs/scrfa.csv` and the summarized dataset `inputs/spawning-records.csv` contains information derived from the SCRFA Aggregations Database: https://www.scrfa.org/database/

 - Marine Regions: Some shapefiles in `inputs/shapefiles` are extracted from Marine Regions (CC-BY-4.0): https://www.marineregions.org/downloads.php

 - Knolls and seamounts in the world ocean: Some shapefiles in `inputs/shapefiles` are extracted from Yesson et al. (2011) (CC-BY-3.0)
   
   Yesson, Chris; Clark, M R; Taylor, M; Rogers, A D (2011): Knolls and seamounts in the world ocean - links to shape, kml and data files. PANGAEA, https://doi.org/10.1594/PANGAEA.757563,

- FAO Major Fishing Areas: FAO regions are used to interpret international spawning reginos.

   FAO 2023. FAO Major Fishing Areas. Fisheries and Aquaculture Division [online]. Rome. 
https://www.fao.org/fishery/en/collection/area
