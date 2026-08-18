# Dataset Card

## Dataset Name

Mining Disturbance Detection — Multi-Temporal Sentinel-2 Dataset

---

## Purpose

The dataset is designed for research into the detection of mining-related
land-cover disturbance from multi-temporal satellite imagery.

It is intended to support:

- classical machine learning;
- image-based deep learning;
- spectral analysis;
- temporal change detection;
- spatial comparison with documented mining boundaries.

---

## Current Study Sites

The current dataset contains two documented masonry-stone mining sites in
Sikar, Rajasthan:

1. Hirwas
2. Rela / Sharda Sharma

---

## Temporal Coverage

Each site currently contains imagery from four observation years:

- 2023
- 2024
- 2025
- 2026

---

## Dataset Size

Each site:

- 200 mining patches
- 200 non-mining patches
- 400 patches total

Current combined dataset:

- 400 mining patches
- 400 non-mining patches
- 800 multispectral patches total

---

## Image Patch Structure

Each multispectral patch contains six Sentinel-2 bands:

- B2 — Blue
- B3 — Green
- B4 — Red
- B8 — Near Infrared
- B11 — SWIR 1
- B12 — SWIR 2

Patches were standardized to approximately:

`16 × 16 × 6`

where:

- 16 × 16 = spatial dimensions
- 6 = spectral channels

---

## Labels

The current binary labels are:

| Label | Class |
|---|---|
| 0 | Non-mining |
| 1 | Mining |

The initial spatial labeling approach used official mine-boundary polygons.

Pixels or sampled locations falling within the documented mining boundary were
assigned to the mining class, while sampled locations outside the mining
boundary were assigned to the non-mining class.

---

## Sampling Strategy

For each mine and observation year:

- 50 mining samples were generated inside the documented mine polygon.
- 50 non-mining samples were generated in the surrounding study area outside
  the mine polygon.

This produces:

`100 patches per year`

and:

`400 patches per site across four years`.

The same spatial sampling seeds/locations were reused across years where
possible to support temporal comparison.

---

## Spatial Context

The patch-generation workflow uses Sentinel-2 imagery at a requested output
scale of 10 m.

The patch footprint is approximately 160 m × 160 m before rasterization and
standardization.

The resulting arrays were standardized to approximately 16 × 16 spatial
pixels.

---

## Raw Multispectral Data

The quantitative image dataset is stored as numerical arrays rather than
ordinary RGB photographs.

The six channels preserve spectral information required by the machine-learning
pipeline.

RGB PNG files are generated separately as human-readable previews.

---

## Derived Tabular Dataset

A separate pixel-level dataset was constructed for the initial classical
machine-learning baseline.

The feature set contains:

- B2
- B3
- B4
- B8
- B11
- B12
- NDVI
- NDBI
- BSI
- latitude
- longitude

An initial Hirwas extraction produced:

- 1,987 sampled pixels
- 97 mining pixels
- 1,890 non-mining pixels

A balanced subset of 194 samples was subsequently used for the preliminary
Random Forest experiment:

- 97 mining
- 97 non-mining

---

## Dataset Provenance

The dataset is derived from:

1. Rajasthan mining / lease records;
2. Government environmental-clearance documentation;
3. Official mine-boundary KML files;
4. Sentinel-2 Level-2A surface-reflectance imagery accessed through Google
   Earth Engine.

The processing pipeline is documented in the project notebooks and
documentation files.

---

## Known Limitations

### 1. Limited number of sites

Only two mining sites are currently included.

Additional sites are required before claiming strong cross-site
generalization.

### 2. Boundary-based labels

The mining/non-mining labels are based on documented mine boundaries.

A boundary label does not by itself establish whether a specific mining action
was legally authorized at a specific time.

### 3. Sentinel-2 spatial resolution

Sentinel-2 may not resolve very small mining features reliably.

### 4. Mixed-resolution bands

B2, B3, B4 and B8 are nominally 10 m bands, while B11 and B12 are nominally
20 m bands and are resampled when used at the requested 10 m patch scale.

### 5. Preliminary evaluation

The first Random Forest result was produced using a single-site pixel-level
split. It should not be interpreted as a final estimate of model
generalization.

---

## Intended Future Expansion

The dataset will be expanded by:

- adding additional mining sites;
- increasing spatial diversity;
- increasing temporal diversity;
- improving patch-label quality;
- creating independent site-level train/validation/test sets;
- evaluating CNN and other deep-learning architectures;
- adding temporal change-detection labels.

---

## Dataset Status

Current status:

**Two sites, four years, 800 multispectral image patches.**

This is an active research dataset and is expected to change as additional
sites and validation data are incorporated.
