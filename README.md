# AI-Powered Site Selection for Solar Farm Installations

Copyright (c) 2026 Shrikara Kaudambady. All rights reserved.

## 1. Introduction

This project provides a Jupyter Notebook that implements an AI-driven workflow for identifying optimal locations for solar farm installations. The solution uses a powerful geospatial analysis technique called **Multi-Criteria Decision Analysis (MCDA)** combined with unsupervised machine learning to pinpoint the most suitable areas for development.

Instead of relying on where farms have been built in the past, this forward-looking approach identifies optimal sites based on a set of weighted criteria, making it a robust tool for strategic planning and investment.

## 2. The Solution Explained

The methodology is based on creating a final "suitability map" where each location (pixel) is scored based on how well it meets a series of weighted criteria.

### 2.1. Data Simulation

The notebook generates five synthetic data layers to model a real-world analysis scenario. Each layer represents a critical factor in solar farm site selection:

1.  **Solar Irradiance:** Simulates the amount of solar energy received, a primary driver of a farm's productivity.
2.  **Terrain Slope:** Represents the steepness of the terrain. Flatter land is cheaper and easier to build on.
3.  **Grid Proximity:** Simulates the distance to the nearest high-voltage power line. Closer proximity reduces infrastructure costs.
4.  **Land Use:** Categorizes the land into types like 'Barren', 'Forest', 'Urban', and 'Water'.
5.  **Protected Areas:** A boolean mask representing areas like national parks where development is prohibited.

### 2.2. Methodology: Multi-Criteria Decision Analysis (MCDA)

The core of the workflow involves four key steps:

1.  **Define Exclusionary Criteria:** First, we identify "deal-breaker" conditions. Areas that are completely unsuitable are removed from consideration. In this notebook, we exclude forests, urban areas, water bodies, and protected zones.

2.  **Normalize Factors:** All remaining data layers (irradiance, slope, grid proximity) are normalized to a common scale of 0 to 1, where a score of **1 is always the most favorable** (e.g., highest irradiance, lowest slope, shortest distance to the grid).

3.  **Assign Weights:** Not all criteria are equally important. We assign a weight to each factor based on its relative importance. These weights are easily adjustable in the notebook, allowing users to tailor the analysis to their specific priorities (e.g., prioritizing energy output over construction cost).
    *   `weight_irradiance = 0.4`
    *   `weight_slope = 0.3`
    *   `weight_grid = 0.3`

4.  **Calculate Suitability Score:** A final suitability score is calculated for each location using a weighted sum:
    `Score = (norm_irradiance * w_irrad) + (norm_slope * w_slope) + (norm_grid * w_grid)`

### 2.3. Unsupervised Learning for Site Identification

After generating the final suitability map, we use the **K-Means clustering algorithm** to automatically identify the top candidate sites. The algorithm groups the highest-scoring locations into distinct, geographically coherent clusters, providing clear targets for further investigation.

## 3. How to Use the Notebook

### 3.1. Prerequisites

You will need Python 3 and Jupyter Notebook/JupyterLab installed. You will also need to install the following libraries:

```bash
pip install numpy pandas scikit-learn matplotlib seaborn
```

### 3.2. Running the Notebook

1.  Clone this repository:
    ```bash
    git clone https://github.com/shrikarak/solar-farm-site-selection-ai.git
    cd solar-farm-site-selection-ai
    ```
2.  Start the Jupyter server:
    ```bash
    jupyter notebook
    ```
3.  Open `solar_site_selection.ipynb` and run the cells sequentially.

## 4. Deployment and Customization

This notebook serves as a powerful template that can be easily adapted for real-world data.

1.  **Using Your Own Data:**
    *   Replace the data simulation cells with code to load your own raster data (e.g., in GeoTIFF format). The `rasterio` library is highly recommended for this.
    *   Ensure all your data layers are co-registered (i.e., they align perfectly with the same coordinate system, extent, and resolution).

2.  **Customizing the Analysis:**
    *   **Adjust Weights:** Modify the `weights` dictionary in the notebook to reflect your project's specific priorities. For example, if grid connection cost is the biggest concern, you could increase the `weight_grid`.
    *   **Modify Exclusions:** Change the `land_use_exclusions` list to include or remove different land use types based on local regulations and constraints.
