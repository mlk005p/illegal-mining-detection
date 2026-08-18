# Data Sources

This document records the primary datasets, government records, spatial
boundaries, and satellite imagery sources used in the mining-disturbance
detection project.

---

## 1. Rajasthan Department of Mines & Geology

**Organization:** Department of Mines & Geology, Government of Rajasthan

**Official website:**

https://mines.rajasthan.gov.in/

### Role in this project

The Rajasthan Department of Mines & Geology was used as the primary government
source for mining and mineral-related information in Rajasthan.

The project initially focused on mining records from **Sikar district** and
used these records to identify candidate mining sites relevant to the study.

### Information used

The mining records were used for:

- identifying mining leases in Sikar;
- identifying the reported mineral associated with each lease;
- identifying candidate masonry-stone mining sites;
- retaining lease/project information for data provenance;
- supporting the selection of sites for spatial verification.

### Candidate-site selection

The project did not treat every lease record as a suitable machine-learning
site.

Candidate sites were filtered and evaluated using:

1. **Geographic relevance** — the site had to be located in Sikar district.
2. **Mineral relevance** — masonry-stone mining was prioritized.
3. **Spatial availability** — a reliable geographic boundary was required or
   preferred.
4. **Satellite suitability** — the site needed to be sufficiently observable
   in Sentinel-2 imagery.
5. **Temporal suitability** — usable imagery had to be available for the
   selected observation years.

---

## 2. Sikar Mining-Lease Dataset

A processed lease-record dataset was used as the initial candidate-site
database.

### Dataset role

The dataset was not used directly as the image-classification dataset.
Instead, it acted as the **source/provenance layer** for identifying and
documenting mining sites.

The workflow was:

```text
Mining-lease records
        ↓
Sikar district records
        ↓
Masonry-stone candidates
        ↓
Spatial/document verification
        ↓
Official mine boundary
        ↓
Sentinel-2 imagery
        ↓
Machine-learning dataset

The processed lease data is retained separately from the satellite-derived
pixel and image-patch datasets.

3. Government Environmental-Clearance Records

Government environmental-clearance records were used to locate project-level
documentation for selected mines.

Portal:

https://environmentclearance.nic.in/

Role in this project

These records were used to obtain or verify:

mining-project identity;
lease/project information;
mineral type;
village/tehsil information;
project area;
geographic information;
official spatial attachments such as KML files.

These records were especially important for obtaining spatial ground truth.

4. Official Mine-Boundary KML Files

Official KML files were used as the spatial ground-truth source for the two
current study sites.

4.1 Hirwas

The Hirwas study site was selected as a documented masonry-stone mining
location in Sikar district.

An official mine-boundary KML was obtained and converted into an
Earth Engine polygon.

The calculated polygon area was approximately:

4.507 hectares

The polygon was used to distinguish pixels and image patches located within
the documented mining area from surrounding areas.

4.2 Rela / Sharda Sharma

The second study site is the Masonry Stone Mining Project, ML No. 254/2010,
near Rela, Neem Ka Thana, Sikar.

The government record identifies the project as a masonry-stone mining project
and provides the associated spatial documentation.

An official KML file was obtained for the site and converted into an Earth
Engine polygon.

The calculated polygon area was:

1.013 hectares

This value was used as a sanity check against the approximately 1-hectare
project/lease scale reported in the government documentation.

5. Sentinel-2 Satellite Imagery

Source: European Space Agency / Copernicus Sentinel-2

Google Earth Engine collection:

COPERNICUS/S2_SR_HARMONIZED

This collection provides harmonized Sentinel-2 surface-reflectance imagery.

Role in this project

Sentinel-2 imagery is the primary remote-sensing input for:

spectral feature extraction;
pixel-level classification;
image-patch generation;
multi-temporal analysis;
future change detection.
6. Sentinel-2 Bands Used

Six bands were selected for the current multispectral image dataset:

Band	Description	Role
B2	Blue	Visible spectral information
B3	Green	Visible spectral information
B4	Red	Visible spectral information
B8	Near Infrared	Vegetation / surface discrimination
B11	SWIR 1	Soil, rock and disturbed-surface information
B12	SWIR 2	Soil, rock and disturbed-surface information
Important spatial-resolution note

Sentinel-2 B2, B3, B4 and B8 have a nominal 10 m spatial resolution.

B11 and B12 have a nominal 20 m spatial resolution and are resampled when
used in the patch-generation workflow at the requested 10 m scale.

This resampling decision should be considered during later model evaluation.

7. Sentinel-2 Scene Selection

For each mining site, imagery was restricted spatially to the study area and
temporally to the required observation period.

The basic selection pipeline was:

Sentinel-2 SR Harmonized
        ↓
Filter by mine study area
        ↓
Filter by date range
        ↓
CLOUDY_PIXEL_PERCENTAGE < 10%
        ↓
Sort by cloud percentage
        ↓
Select clearest suitable scene

For multi-year comparison, a comparable seasonal window was preferred where
sufficient clear imagery was available.

8. Observation Years

The current image dataset uses four observation years:

2023
2024
2025
2026
Hirwas selected scenes
Year	Selected date	Cloud cover
2023	2023-09-04	0%
2024	2024-05-21	0%
2025	2025-05-13	0%
2026	2026-05-16	approximately 0%
Rela selected scenes
Year	Selected date	Cloud cover
2023	2023-05-09	0.001139%
2024	Selected May 2024 scene	Recorded in notebook
2025	2025-05-18	0.000776%
2026	2026-05-18	0.000385%

The exact scene-selection values and processing steps are retained in the
research notebooks.

9. Spectral Feature Engineering

In addition to the raw Sentinel-2 bands, three derived spectral indices were
used in the initial tabular baseline.

NDVI

Normalized Difference Vegetation Index:

NDVI = (NIR - Red) / (NIR + Red)

Implemented using:

B8 and B4
Purpose

Used as an indicator of vegetation presence and to help distinguish vegetated
areas from disturbed or exposed surfaces.

NDBI

Normalized Difference Built-up Index:

NDBI = (SWIR - NIR) / (SWIR + NIR)

Implemented using:

B11 and B8
Purpose

Used as a spectral indicator of built-up or disturbed surface characteristics.

BSI

Bare Soil Index:

BSI = ((SWIR1 + Red) - (NIR + Blue))
      /
      ((SWIR1 + Red) + (NIR + Blue))

Implemented using:

B11, B4, B8 and B2
Purpose

Used to capture exposed soil, bare ground, and disturbed surface
characteristics.

10. Initial Pixel-Level Dataset

For the first Hirwas analysis, a 500 m study area was established around the
mine.

From the selected 2023 Sentinel-2 image, 1,987 pixel samples were
collected.

Each sample contained:

B2
B3
B4
B8
B11
B12
NDVI
NDBI
BSI
latitude
longitude

This formed the initial tabular satellite feature dataset.

11. Ground-Truth Labeling

The official mine-boundary polygon was used as the spatial ground-truth
reference.

For the initial pixel dataset:

inside mining polygon     → label 1
outside mining polygon    → label 0

The initial Hirwas sample distribution was:

Total samples       = 1,987
Mining samples      = 97
Non-mining samples  = 1,890

Because this was highly imbalanced, a balanced subset was created for the
initial Random Forest experiment:

Mining      = 97
Non-mining  = 97
Total       = 194
12. Multispectral Image-Patch Dataset

The project was subsequently expanded from individual tabular pixels to
multispectral image patches.

For each mine and observation year:

50 mining patches
50 non-mining patches

This produced:

100 patches per year

Across four years:

100 × 4 = 400 patches per mine

Current sites:

Hirwas = 400 patches
Rela   = 400 patches
Current total
800 multispectral image patches

Each multispectral patch contains six Sentinel-2 bands:

B2
B3
B4
B8
B11
B12

The patches were standardized for CNN preparation to approximately:

16 × 16 × 6

The corresponding patch metadata records the class and observation year.

13. Image Preview Dataset

To make the multispectral dataset human-readable for inspection and
presentation, RGB preview images were generated from:

B4 = Red
B3 = Green
B2 = Blue

These previews are visualization artifacts.

The multispectral .npz/.npy files remain the quantitative data used for
machine-learning experiments.

14. Current Dataset Organization

Each study site is maintained as an independent dataset.

Example structure:

Site
├── mining
├── non_mining
├── mining_2024
├── non_mining_2024
├── mining_2025
├── non_mining_2025
├── mining_2026
├── non_mining_2026
├── previews
└── metadata.csv

Each completed site dataset is also archived as a ZIP file for backup.

15. Data Provenance Principle

The project distinguishes between:

Source data
Government mining records and environmental-clearance documents.
Spatial ground truth
Official mine-boundary KML files.
Remote-sensing data
Sentinel-2 surface-reflectance imagery.
Derived data
Spectral indices, pixel samples, and image patches.
Machine-learning datasets
Labeled and balanced subsets generated from the derived data.

This separation is maintained so that the origin and transformation of each
dataset component can be traced.

16. Limitations of the Current Dataset

The current dataset has several limitations that must be considered before
final model evaluation:

The current study contains only two mining sites.
Mining labels represent the documented mining boundary and do not by
themselves establish whether an activity is legally authorized or
unauthorized at a particular date.
Sentinel-2 has limited spatial resolution for small mining features.
B11 and B12 are native 20 m bands while the patch pipeline requests a 10 m
output scale.
The initial Random Forest experiment used a single-site pixel-level split,
so its performance should not be interpreted as cross-site generalization.
Additional mining sites are required for robust training and independent
site-level testing.
17. Planned Data Expansion

Future dataset development will focus on:

additional documented mining sites;
additional spatial diversity;
multi-site training and testing;
improved patch-label quality;
additional temporal observations;
independent site-level evaluation.

The intended long-term dataset is therefore:

multiple mining sites
        ×
multiple years
        ×
multiple labeled image patches

rather than a single-mine dataset.
