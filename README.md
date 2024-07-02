# Satellite Position Calculation with Distributed Computing
the last attempt was using google colab
## Overview

This project demonstrates how to efficiently calculate satellite positions using Python, Dask, and the `sgp4` library, leveraging distributed computing to reduce computation time.

## Setup

### Requirements

Ensure you have the following installed:

- Python (version 3.7 or higher recommended)
- Dask (`pip install dask`)
- `sgp4` (for satellite orbit computations, `pip install sgp4`)

### Input Data

The input data consists of Two-Line Element (TLE) sets stored in a text file (`30sats.txt`). Each TLE set provides orbital parameters required for satellite position computation.

## Approach

### 1. Reading TLE Data

- Read TLE data from the file (`30sats.txt, 30000sats.txt`).
- Parse the data into a list of dictionaries where each dictionary contains two lines (`line1` and `line2`) from the TLE.

### 2. Parallel Processing with Dask

- Use Dask to parallelize the computation across multiple TLE entries.
- Convert the list of TLE dictionaries into a Dask bag (`tle_bag`) for efficient parallel processing.
- Map the `calculate_positions` function over the Dask bag to compute satellite positions for each TLE set concurrently.

### 3. Satellite Position Calculation

- Define the `calculate_positions` function which:
  - Converts TLE parameters into a `Satrec` object using `sgp4.api.Satrec.twoline2rv()`.
  - Iterates through 1440 time steps (one per minute) starting from the current time.
  - Computes satellite position (`r`) and velocity (`v`) using `sgp4.api.Satrec.sgp4()`.
  - Stores the computed positions along with timestamp in a list of dictionaries.

### 4. Triggering Computation

- Use `dask.compute()` to trigger the computation of satellite positions across all TLE entries.
- Retrieve the computed positions as a list of lists (`computed_positions`).

### 5. Output

- Print or process the `computed_positions` list to analyze or further use the satellite positions as needed.

## Results

Distributed computing with Dask allows parallel execution of the `calculate_positions` function across multiple CPU cores or even a cluster of machines. This approach significantly reduces computation time compared to sequential processing. The parallelization benefits are particularly noticeable when dealing with large datasets or computationally intensive tasks like satellite orbit calculations.

