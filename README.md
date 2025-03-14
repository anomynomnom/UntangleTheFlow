# Untangle the Flow

## Overview
This project consists of **two independent parts**, which can be **considered separately**:

1. **Instance Generation & Clustering** (`01_*` to `03_*` files)  
   Enables users to generate OD flow datasets, apply clustering methods, and visualize results interactively.

2. **Reproducibility of Experimental Results** (`exp_*` files)  
   Provides scripts to reproduce the results from the research paper.


---

## 1. Instance Generation & Clustering

### Instance Generation (`01_instance_generator.R`)
This script enables users to generate synthetic **origin-destination (OD) flow datasets** for a specific section of Dresden. It relies on the following input files:

- `sf_network.rds`: An `sf` object containing the street network.
- `local_node_dist_mat.rds`: A  `data.table` containing network distances between all nodes in the street network.

Key configurable parameters:

- `num_min_length` & `num_max_length`: Defines the boundaries for flow lengths within each cluster, depending on the approximate radius of the convex hull of the street network section.
- `num_min_flows` & `num_max_flows`: Specifies the range for the number of flows per cluster.
- `num_min_clusters` & `num_max_clusters`: Determines the overall number of clusters.

Generated OD flow datasets are stored under unique UUID-based directories, each containing an `rds` file with:
- The generated **flows** as an `sf` object with the associated `cluster_id`

---

### HDBSCAN & Distance Computation (`02_hdbscan.R`)
For a selected synthetic OD dataset, this script computes OD flow distance matrices for three different distance measures:
- `manhattan_euclid`
- `chebyshev_euclid`
- `manhattan_network` (network-based Manhattan distance)

Based on the selected metric, the distance matrix is processed using **PaCMAP**, producing:
- A `dist()` object containing the original OD flow distance matrix.
- A `matrix` file storing the **PaCMAP 4D embedding result**.

### Clustering Experiment
A mini-experiment evaluates clustering performance using **HDBSCAN**:
- `minpts` (minimum cluster size) is varied between **2 and 50**.
- Clustering results for both **original** and **PaCMAP-transformed** data are evaluated using the **Adjusted Rand Index (ARI)**.
- The results are visualized with `ggplot2`, displaying a line chart comparing ARI values across different `minpts` values.

---

## Running the Scripts

### Prerequisites
Ensure the required R packages are installed:
```r
install.packages(c("sf", "hdbscan", "ggplot2", "pacmap"))
