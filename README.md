# Untangle the Flows
This repository provides supplementary code and data for the paper **"Untangle the Flows: Enhanced Clustering of Origin-Destination Flows using PaCMAP"**, 
supporting the implementation and analysis presented in the study.  

The project consists of two independent parts:

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

The main parameters include `num_min_length` & `num_max_length` for flow length boundaries, `int_min_cl_size` & `int_max_cl_size` for the number of flows per cluster, and `int_min_n_cl` & `int_max_n_cl` for the possible number of clusters. Generated OD flow datasets are stored under unique UUID-based directories as subfolders of `/data/synthetic`, each containing an `rds` file with the generated **OD flows** as an `sf` object with the associated `cluster_id`, representing the ground truth.


### Clustering (`02_hdbscan.R`)
For a selected synthetic OD dataset, this script computes OD flow distance matrices using `manhattan_euclid`, `chebyshev_euclid`, and `manhattan_network`. The chosen metric is then processed with **PaCMAP**, generating the following outputs:


- A `dist` object containing the original OD flow distance matrix.
- A `matrix` file storing the PaCMAP 4D embedding result.

The `dbscan::hdbscan` function is applied with varying `minpts` values (ranging from 2 to 50) to evaluate clustering performance. The results are visualized using `ggplot2`, displaying a line chart that compares ARI (Adjusted Rand Index) values across different `minpts` settings.

### Shiny-App (`03_shiny_app.R`)
This `shiny` dashboard enables users to interactively set the `minpts` parameter for `hdbscan`, dynamically updating four `leaflet` maps visualizing the corresponding cluster results for one specific synthetic dataset, based on the original OD flow distance matrix and the PaCMAP 4D embedding. Users can explore individual clusters and compare their assignments across both representations.

---

## Reproducibility of Experimental Results
Unfortunately, due to data protection and copyright reasons, the data used for the qualitative real-world analysis cannot be provided. To enable the reproducability of the synthetic experiments conducted, the 100 synthetic OD flow datasets used can be downloaded from [Zenodo](https://zenodo.org/records/15025399?preview=1&token=eyJhbGciOiJIUzUxMiJ9.eyJpZCI6IjIwYzY5NzgwLTRlN2EtNGI3Yy04OGJkLWU2NzNmMTdlMWEyYiIsImRhdGEiOnt9LCJyYW5kb20iOiIzMDY0NGZhN2ViNzkwZmExMDE1NzQxZDA0NmRmN2E2MiJ9.FZxejH2itkRpWa-T4MRH62ZDfkAu8QtiQksBd0pzQ7rlRiXnLTkzo3kZbblljaiGSUUBA7leOD-utgRJ_PrUXw). 