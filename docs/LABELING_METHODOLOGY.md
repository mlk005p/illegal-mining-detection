# Labeling Methodology

## Purpose

This document describes how mining and non-mining labels were assigned to
samples in the satellite-derived datasets.

The objective is to create spatially traceable labels based on documented mine
boundaries rather than assigning labels from arbitrary spectral thresholds.

---

## 1. Source of Ground Truth

The primary spatial ground-truth source is the official mine-boundary KML
associated with the selected mining project.

The KML boundary was converted into an Earth Engine polygon and used as the
reference geometry for spatial labeling.

Current study sites:

- Hirwas
- Rela / Sharda Sharma

---

## 2. Polygon Validation

Before generating training data, the polygon was checked in Earth Engine.

The calculated polygon areas were:

| Site | Calculated area |
|---|---:|
| Hirwas | 4.507 ha |
| Rela / Sharda Sharma | 1.013 ha |

These calculations were used as sanity checks against the approximate project
areas reported in the corresponding government documentation.

---

## 3. Binary Label Definition

The current task uses two classes:

| Label | Class | Meaning |
|---|---|---|
| 1 | Mining | Sample location associated with the documented mining polygon |
| 0 | Non-mining | Sample location outside the documented mining polygon |

The labels therefore represent **spatial association with the documented mine
boundary**.

They should not be interpreted as a legal determination of whether a mining
operation was authorized or unauthorized at the exact observation date.

---

## 4. Pixel-Level Labeling

For the initial Hirwas pixel dataset, approximately 1,987 Sentinel-2 samples
were collected from a study area surrounding the mine.

Each sample contains geographic coordinates and spectral features.

The spatial labeling rule was:

Sample point inside official mine polygon
                ↓
             label = 1

Sample point outside official mine polygon
                ↓
             label = 0
''''text
This produced:

97 mining samples
1,890 non-mining samples

for a total of 1,987 sampled pixels.

5. Class Balancing

The initial pixel dataset was strongly imbalanced.

Therefore, a balanced subset was created for the preliminary Random Forest
experiment.

The balanced subset contained:

97 mining samples
97 non-mining samples
194 samples total

The number of non-mining samples was reduced to match the number of mining
samples.

This balancing step was used for the preliminary baseline experiment and does
not replace the original 1,987-sample dataset.

6. Image-Patch Labeling

The project was later extended from individual pixels to multispectral image
patches.

For each study site and observation year:

50 mining patch locations were sampled from the mine polygon.
50 non-mining patch locations were sampled from the surrounding area
outside the polygon.

Thus:

50 mining
+
50 non-mining
=
100 patches per year

Across four observation years:

100 × 4 = 400 patches per site

The current two-site dataset therefore contains:

Hirwas = 400 patches
Rela   = 400 patches


Total  = 800 patches
7. Spatial Consistency Across Years

For temporal comparison, the same sampling strategy and random seeds were
used across years for each site.

This allows corresponding patch locations to be compared across different
observation years.

Conceptually:

Site A / Patch 001 / 2023
        ↕
Site A / Patch 001 / 2024
        ↕
Site A / Patch 001 / 2025
        ↕
Site A / Patch 001 / 2026

This structure is intended to support future temporal change-detection
experiments.

8. Non-Mining Sampling

Non-mining samples were generated from the surrounding study area after
excluding the official mining polygon.

The intention is to provide examples of surrounding land cover that can be
compared against mining-related surfaces.

The non-mining class may include heterogeneous land-cover types such as
vegetation, agricultural land, exposed soil, and other non-mining surfaces.

This heterogeneity is intentional, but it is also a known source of class
overlap and potential false positives.

9. Image-Patch Dimensions

The multispectral patches are generated from Sentinel-2 imagery at a
requested 10 m scale.

The resulting raster requests produce approximately 17 × 17 pixel arrays
before standardization.

For CNN preparation, these arrays are standardized to:

16 × 16 × 6

where the six channels are:

B2
B3
B4
B8
B11
B12
10. Important Labeling Limitations
Boundary versus actual activity

A mine boundary identifies the documented project/lease area. It does not
necessarily indicate that every pixel inside the polygon represents active
excavation at every observation date.

Mixed pixels

A Sentinel-2 pixel may contain a mixture of surface types, especially near
mine boundaries.

Patch-level ambiguity

A patch can contain both mining and non-mining surfaces even when its sampled
center lies inside the mine polygon.

Temporal changes

A location classified as mining from the boundary may change substantially
between years because of excavation, restoration, vegetation growth, or other
land-cover changes.

Generalization

The current dataset contains only two sites, so additional sites are needed
to evaluate whether the model generalizes beyond the sites used to construct
the current dataset.

11. Future Label Improvements

Future versions of the dataset may introduce:

polygon-overlap thresholds for patch acceptance;
more precise excavation masks;
manually reviewed ambiguous patches;
additional mine sites;
independent validation sites;
temporal change labels;
potential unauthorized/outside-boundary disturbance labels.

These improvements will be considered before the final model is evaluated as an
illegal-mining detection system.

