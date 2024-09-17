### Tech Stack:

- think about what a recuriter from Earthmover would like?
- use:
  - Dask
  - Numba (?)
  - Xarray
  - Zarr
  - SciPy
  - some diff-eq solver
  - something for random walks

### Project Ideas:

#### 1. Random walk in 3D lattice:

Objective: Simulate the random walk of particles in a 3D lattice, where each particle’s movement can be influenced by a percolation model, and use xarray, dask, and zarr to manage and analyze the data.

Steps:

3D Lattice Setup: Create a 3D grid where particles can move from one voxel to another.
Percolation Influence: Introduce a percolation model that dictates the probability of a particle moving to an adjacent voxel (e.g., based on whether the voxel is part of a percolated path).
Random Walk Simulation: Simulate the random walk of multiple particles over time, using dask to parallelize the simulation for a large number of particles.
Data Management: Use xarray to organize the positions of particles over time and zarr to store this data efficiently.
Analysis: Analyze how the random walk behavior changes with different percolation thresholds and visualize the paths taken by the particles.
Learning Outcomes:

Basic implementation of a random walk simulation.
Integration of a percolation model to influence particle movement.
Use of xarray and zarr for managing and storing simulation data.
Parallel computation using dask.

#### 2. 3D percolation in a porus medium:

Objective: Simulate percolation through a 3D porous medium and analyze the flow of fluid through it using xarray, dask, and zarr for efficient data handling and processing.

Steps:

Generate a 3D Grid: Create a 3D grid where each voxel (3D pixel) represents a part of a porous medium, with some voxels being solid and others being voids (pores).
Percolation Simulation: Simulate the percolation process by filling the grid with fluid starting from one side and moving through connected pores.
Data Handling: Store the 3D grid data using xarray and save intermediate states of the simulation to a zarr store.
Parallel Processing: Use dask to parallelize the percolation process, especially when simulating large grids or multiple time steps.
Analysis: Analyze the percolation threshold (the point at which the fluid reaches the opposite side) and visualize the fluid distribution using 3D plots.
Learning Outcomes:

How to handle multi-dimensional data with xarray.
Efficient storage and access using zarr.
Parallelizing computations with dask.
Basic implementation of a 3D percolation model.
