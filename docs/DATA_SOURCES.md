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
