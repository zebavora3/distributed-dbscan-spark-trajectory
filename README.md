# Optimising Distributed DBSCAN for Large-Scale Trajectory Clustering in Apache Spark

## Project Overview
Trajectory clustering of large-scale GPS data is computationally challenging due to the pairwise nature of density-based methods. This project implements and evaluates a **Distributed DBSCAN architecture in Apache Spark (PySpark) on Databricks** using the massive **T-Drive trajectory dataset**. 

Specifically, this repository highlights the design and execution of the **Global Pipeline**, which utilizes unbuffered spatial partitioning combined with a custom cross-partition border-point exchange mechanism to merge clusters seamlessly across distributed nodes.

## Tech Stack & Architecture
* **Environment:** Databricks Cloud Cluster
* **Framework:** Apache Spark / PySpark
* **Core Algorithms:** Distributed DBSCAN, Spatial Grid Partitioning, Coordinate-to-Cell Mapping
* **Languages:** Python / PySpark SQL

## My Contribution: The Global Pipeline
In a distributed environment, standard DBSCAN breaks down because it cannot natively look across partition boundaries. To solve this, I built the global pipeline stage:
1. **Unbuffered Partitioning:** Distributed the massive T-Drive GPS trajectory dataset across Spark executors using spatial grid bounds without overlapping margins to minimize initial shuffle sizes.
2. **Local Clustering:** Ran localized DBSCAN operations in parallel on each spatial cell chunk.
3. **Border-Point Exchange & Merging:** Designed a global merge step that identifies data points sitting right on the grid edges, exchanges border points across the network, and dynamically restitches fragmented clusters into unified global trajectories.

## Key Findings & Performance Metrics
* **Scalability:** Successfully scaled the processing pipeline across subsets of data, balancing execution workloads against the inherent spatial skew of urban taxi trajectories.
* **Clustering Accuracy:** Achieved an **Adjusted Rand Index (ARI) of 0.8533** against a sequential scikit-learn baseline, validating that the distributed optimization maintained high clustering quality while processing millions of records.
