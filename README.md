# TAsk-specific Data Quality Assessment Framework
Repository for the python implementation of task-specific quality assessment of global open building datasets (OBDs) using health accessibility modelling. This project evaluates the fitness of four major open building datasets—Google, Microsoft, OSM, and GBA—for spatial analysis healthcare and pipe water access services.
```
data/
├── raw/                      # Original downloaded datasets
│   ├── google_buildings/    # Google Open Buildings
│   ├── microsoft_buildings/ # Microsoft Building Footprints  
│   ├── osm_buildings/       # OpenStreetMap data
│   └── googlebuildingatlas/ # TUM
├── processed/                # Harmonized/cleaned datasets
├── reference/                # Administrative boundaries, study areas
│   └── study_area.geojson   # Your AOI polygon
└── ancillary/                # Supporting data
    ├── health_facilities/    # Hospital/clinic locations
    ├── road_network/         # Transportation networks
    └── population/           # Gridded population data
```

# Dataset Specifications

<table>
  <tr>
    <th>Dataset</th>
    <th>Source</th>
    <th>Format</th>
    <th>Resolution/Scale</th>
    <th>For comparison across cities(Not needed for the paper!)</th>
  </tr>
  <tr>
    <td><strong>Google Open Buildings</strong></td>
    <td><a href="https://sites.research.google/gr/open-buildings/">Google Open Buildings</a></td>
    <td>GeoJSON/Parquet</td>
    <td>Building footprints</td>
    <td><code>google_buildings_[city].geojson</code></td>
  </tr>
  <tr>
    <td><strong>Microsoft Building Footprints</strong></td>
    <td>Microsoft</td>
    <td>GeoJSON</td>
    <td>~1:1000 scale</td>
    <td><code>microsoft_buildings_[city].geojson</code></td>
  </tr>
  <tr>
    <td><strong>OSM Building Data</strong></td>
    <td><a href=" https://download.geofabrik.de/](https://download.geofabrik.de/">Geofabrik</a></td>
    <td>.shp/.pbf</td>
    <td>Varies by region</td>
    <td><code>osm_buildings_[city].shp</code></td>
  </tr>
  <tr>
    <td><strong>GlobalBuildingAtlas</strong></td>
    <td><a href="https://doi.org/10.14459/2025mp1782307">https://doi.org/10.14459/2025mp1782307</a></td>
    <td>GPKG</td>
    <td></td>
    <td><code>esri_buildings_[city].gdb</code></td>
  </tr>
  <tr>
    <td><strong>Health Facilities</strong></td>
    <td><a href="https://data.humdata.org/dataset/hotosm_gha_health_facilities">Ghana Health Facilities (OpenStreetMap)</a></td>
    <td>GeoJSON/CSV</td>
    <td>Point locations</td>
    <td><code>health_facilities_[city].geojson</code></td>
  </tr>
  <tr>
    <td><strong>Road Network</strong></td>
    <td>OSM/OpenStreetMap</td>
    <td>.pbf/.graphml</td>
    <td>Street segments</td>
    <td><code>road_network_[city].graphml</code></td>
  </tr>
  <tr>
    <td><strong>Population</strong></td>
    <td>WorldPop/GHSL</td>
    <td>GeoTIFF</td>
    <td>100x100 meter grids</td>
    <td><code>population_[city]_[year].tif</code></td>
  </tr>
</table>


* Create directory structure
mkdir -p data/{raw/{google_buildings,microsoft_buildings,osm_buildings,esri_buildings},processed,reference,ancillary/{health_facilities,road_network,population}}

* Download study area boundary (example for Nairobi, Accra.etc.)
* Add your study area polygon to data/reference/study_area.geojson

* Download health facilities (example using OSM)
python scripts/download_health_facilities.py --city "Nairobi" --output data/ancillary/health_facilities/

* Extract road network
python scripts/download_road_network.py --city "Nairobi" --output data/ancillary/road_network/


* Example data loading in notebook
google_data = gpd.read_file('data/processed/google_buildings_nairobi.geojson')
microsoft_data = gpd.read_file('data/processed/microsoft_buildings_nairobi.geojson')  
osm_data = gpd.read_file('data/processed/osm_buildings_nairobi.geojson')
esri_data = gpd.read_file('data/processed/esri_buildings_nairobi.geojson')
facilities = gpd.read_file('data/ancillary/health_facilities/nairobi_hospitals.geojson')

* Data Sources & Download Instructions
Google Open Buildings: Download via VTP

Microsoft Footprints: GitHub Releases

OSM Data: Geofabrik Downloads

Esri Footprints: Living Atlas

Health Facilities: healthsites.io or OSM

Population Data: WorldPop or GHSL

# Notes
Clip all datasets to your study area before processing to reduce file sizes

Ensure coordinate reference systems match (use EPSG:4326 for consistency)

For large areas, consider tiling or sampling approaches



