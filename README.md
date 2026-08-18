# Mining Disturbance Detection Using Multi-Temporal Sentinel-2 Imagery

A geospatial AI project for detecting mining-related land-cover disturbance using
multi-temporal Sentinel-2 satellite imagery, official mining-site boundaries,
spectral features, and machine learning.

## Project Status

🟢 Data acquisition  
🟢 Mining-site identification  
🟢 Mine-boundary extraction  
🟢 Multi-temporal dataset generation  
🟢 Random Forest baseline  
🟡 Cross-site evaluation  
🟡 CNN development  
🔴 Change-detection system  
🔴 Interactive monitoring dashboard  

## Study Area

Sikar, Rajasthan, India.

## Current Dataset

The current dataset contains two officially documented masonry-stone mining
sites:

- Hirwas
- Rela / Sharda Sharma

For each site, Sentinel-2 imagery has been collected for four observation years
(2023–2026).

Current image dataset:

- 2 mining sites
- 4 years per site
- 50 mining patches per year
- 50 non-mining patches per year
- 800 multispectral patches in total
- 6 Sentinel-2 spectral bands per patch

## Data Sources

- Rajasthan mining / lease records
- Government environmental-clearance records
- Official mine-boundary KML files
- Sentinel-2 Level-2A Surface Reflectance imagery through Google Earth Engine

## Current Baseline

A preliminary Random Forest baseline was developed using Sentinel-2 spectral
bands and derived spectral indices.

Current single-site pixel-level baseline:

- Accuracy: 84.62%
- F1-score: 0.85

This is a preliminary result and should not yet be interpreted as cross-site
generalization performance.

## Planned System

The final system is intended to combine:

1. Multi-temporal satellite imagery
2. Spectral feature engineering
3. Classical machine-learning baselines
4. Deep-learning image classification / segmentation
5. Temporal change detection
6. Spatial comparison with authorized mining boundaries
7. Interactive geospatial visualization

## Repository Structure

```text
docs/        Project documentation
notebooks/   Research and experiment notebooks
src/         Reusable Python source code
data/        Metadata and dataset references
results/     Figures, metrics and maps
app/         Final dashboard
