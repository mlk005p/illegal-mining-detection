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

```text
Sample point inside official mine polygon
                ↓
             label = 1

Sample point outside official mine polygon
                ↓
             label = 0

