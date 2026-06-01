# Voronoi Percolation Analysis

This code is designed to run in **Jupyter Lab** or **Jupyter Notebook** and analyzes the geometric connectivity of particle configurations using **Voronoi tessellation**.

The script reads the final frame of a **LAMMPS dump file**, constructs a two-dimensional Voronoi diagram under periodic boundary conditions, identifies connected Voronoi edges based on an interparticle-distance criterion, and computes percolation-related quantities such as the fraction of connected edges and the mean finite cluster size.

This analysis is useful for studying structural connectivity, percolation transitions, and clustering behavior in active matter and molecular simulation systems.

---

## Features

* Read particle positions and diameters from a LAMMPS dump file
* Analyze a selected subset of particle IDs
* Construct two-dimensional Voronoi tessellations
* Apply periodic boundary conditions using a 3×3 box replication method
* Classify Voronoi edges as connected or disconnected based on particle spacing
* Visualize Voronoi networks with color-coded edge types
* Compute connectivity and cluster statistics
* Identify and exclude percolating clusters from finite-cluster analysis

---

## Analysis Performed

### 1. Voronoi Tessellation

A Voronoi diagram is generated from particle positions projected onto the x–y plane.

Each particle is assigned a Voronoi cell whose boundaries are determined by neighboring particles.

---

### 2. Edge Classification

Neighboring particles are connected through a Voronoi ridge.

For each neighboring particle pair, the center-to-center distance is compared to a threshold:

```text
distance ≥ average_particle_diameter + passage_offset
```

where:

* average_particle_diameter = mean diameter of the neighboring particles
* passage_offset = user-defined threshold (default: 0.701)

Edges satisfying this condition are classified as:

* Connected edges (blue)

Edges failing the condition are classified as:

* Disconnected edges (red dashed)

---

### 3. Connectivity Fraction

The code computes:

```text
p = Number of connected edges / Total number of finite Voronoi edges
```

which serves as a measure of network connectivity.

---

### 4. Cluster Identification

Connected Voronoi edges are converted into a graph representation.

Connected components are identified using graph traversal algorithms.

For each cluster, the code determines:

* Number of vertices
* Number of edges
* Cluster membership

---

### 5. Percolation Detection

Clusters spanning opposite sides of the simulation box are identified as percolating clusters.

A cluster is considered percolating if it touches:

* Left and right boundaries

or

* Bottom and top boundaries

under periodic boundary conditions.

Percolating clusters are excluded from finite-cluster statistics.

---

### 6. Mean Cluster Size

The mean finite cluster size is calculated as:

```text
S = (Σ s²) / E
```

where:

* s = number of edges in a finite cluster
* E = total number of connected edges

This quantity is commonly used in percolation theory to characterize connectivity fluctuations near the percolation threshold.

---

## User-Adjustable Parameters

### Input Settings

* LAMMPS dump file name
* Particle ID range
* Number of particles included in the analysis

### Voronoi Analysis

* Voronoi dimensionality (currently 2D)
* Periodic image replication size
* Edge classification criterion

### Connectivity Settings

* passage_offset threshold
* Percolation detection criteria

### Visualization

* Figure size
* Output filename
* Axis limits
* Particle marker size scaling

---

## Outputs

### Voronoi Visualization

The code generates a figure showing:

* Voronoi tessellation
* Connected edges (blue)
* Disconnected edges (red dashed)
* Particle positions scaled by diameter

Example output:

```text
voroni.pdf
```

---

### Percolation Statistics

The script reports:

```text
p = fraction of connected edges

S = mean finite cluster size

Number of connected edges

Number of connected components

Number of finite clusters

Percolating cluster indices
```

---

## Applications

* Percolation analysis
* Active Brownian particle systems
* Motility-induced phase separation (MIPS)
* Voronoi network analysis
* Structural connectivity studies
* Porous media characterization
* Cluster-size distribution analysis
* Geometric phase transitions

---

## Workflow

```text
LAMMPS Dump File
        ↓
Extract Final Frame
        ↓
Particle Coordinates & Diameters
        ↓
3×3 PBC Replication
        ↓
Voronoi Tessellation
        ↓
Edge Classification
        ↓
Connected Voronoi Network
        ↓
Cluster Identification
        ↓
Percolation Detection
        ↓
p and S Calculation
        ↓
Voronoi Visualization & Statistics
```

---

## Notes

* The current implementation analyzes the final simulation frame.
* Periodic boundary conditions are handled through coordinate replication.
* Percolating clusters are automatically excluded from finite-cluster-size calculations.
* The method is based on Voronoi-network connectivity rather than direct particle-contact clustering.
* Although developed for LAMMPS simulations, the code can be adapted to other particle-based trajectory formats with minimal modification.
