### Goals:

1. implement a basic probabilistic fire spread model on a 2d lattice whose points are not encoded with a vector of characteristics
2. once a basic model has been created, define the relationship between the values of the cell's vector and it's probability of being burned
3. (eventually) figure out a way to take a real image, convert it to the lattice required of the model, eventually using ML
    to take an image and infer the values of the vector for each cell in the lattice (image).
    a. this would entail taking an image, subdividing it into cells, having a model predict environmental characteristics from characteristics of the image (vegitation density, moisture, elevation)

### Step-by-Step Process for Wildfire Risk Assessment Using Monte Carlo Simulations

#### **1. Collect 12 Images from a Particular Region of Land**

- **Explanation:** You obtain a series of images (e.g., one per month over a year) covering the area of interest. These images capture the seasonal variations in vegetation density (VD) and moisture levels (M).

#### **2. Use an ML Model to Estimate VD, M, and E for Each Image**

- **Vegetation Density (VD):** Use a machine learning model (such as a Convolutional Neural Network) to estimate the vegetation density for each cell in your grid based on the images.
- **Moisture Levels (M):** The same or a different ML model estimates moisture levels from image data, possibly using spectral information if available.
- **Elevation (E):** Obtain elevation data for each cell, either from the images using photogrammetry or from existing digital elevation models (DEMs). Since elevation doesn't change over time, this step may be done once.

#### **3. Construct Distributions for VD and M from the Time Series**

- **Time Series Data:** For each cell, you have 12 data points for VD and M corresponding to each image.
- **Assuming Gaussian Distribution:** You calculate the mean and standard deviation for VD and M, assuming they follow a normal (Gaussian) distribution.
  - **Note:** Verify that the data fits a Gaussian distribution. If not, consider using a different distribution that better represents your data (e.g., log-normal, beta distribution).
- **Purpose of Distributions:** These distributions represent the variability and uncertainty of VD and M over time and will be used in the Monte Carlo simulations.

#### **4. Find or Create a Model Relating Fire Probability to VD, M, and E**

- **Fire Risk Function:** Develop or use an existing model that estimates the probability of wildfire ignition or spread based on VD, M, and E. This function could be empirical or based on theoretical considerations.
  - **Example Function:** `P_{fire} = f(VD, M, E)`, where `P_{fire}` is the probability of fire.
- **Incorporate Phase Transition:** If using percolation theory, the model might include critical thresholds where the probability of fire changes dramatically, representing a phase transition.

#### **5. Perform Monte Carlo Simulations**

- **Number of Trials:** Decide on the number of simulations (e.g., 1,000 trials). More trials can provide more accurate estimates but require more computational resources.
- **For Each Trial:**
  - **Random Sampling:** Draw random samples of VD and M for each cell from their respective distributions.
    - **Sampling VD:** `VD_sample = RandomSample(μ_{VD}, σ_{VD})`
    - **Sampling M:** `M_sample = RandomSample(μ_{M}, σ_{M})`
  - **Use Fixed E:** Elevation is constant for each cell.
  - **Compute Fire Probability:** Calculate `P_{fire}` for each cell using the sampled values and the fire risk function.
  - **Record the Probability:** Store the computed probability for each cell.

#### **6. Compute Risk Metrics for Each Cell**

- **Aggregate Probabilities:**
  - After all trials, you have a set of probabilities for each cell.
  - **Average Probability:** Calculate the average probability of fire for each cell across all trials.
    - `\overline{P_{fire}} = \frac{1}{N} \sum_{i=1}^{N} P_{fire, i}`
- **Phase Transition Probability:**
  - Identify critical thresholds where the probability of fire increases sharply.
  - **Risk Metric:** Use this value to quantify how 'risky' each cell is concerning wildfires.

#### **7. Visualize Risk Across Different Parts of the Land**

- **Color Encoding:**
  - Map the risk metrics to a color scale (e.g., green for low risk, red for high risk).
- **Create Risk Maps:**
  - Generate visualizations that show the spatial distribution of wildfire risk.
  - **Interpretation:** These maps help stakeholders identify high-risk areas and make informed decisions.

### **Additional Points to Consider**

- **Data Limitations:**
  - **Sample Size:** With only 12 images, the statistical reliability of your distributions may be limited.
  - **Mitigation:** Consider collecting more data if possible or using data augmentation techniques.
- **Validation:**
  - **Model Verification:** Validate your fire risk model with historical fire data if available.
  - **Simulation Accuracy:** Increase the number of Monte Carlo trials to improve accuracy.
- **Computational Resources:**
  - Ensure you have adequate processing power to handle the simulations, especially if you're using a fine grid or a large number of trials.
- **Interdependencies:**
  - **Spatial Correlation:** Fires can spread from one cell to neighboring cells. Consider incorporating spatial dependencies into your model if possible.
  - **Temporal Dynamics:** Fires and environmental conditions change over time; a dynamic model might provide more accurate risk assessments.

### **Summary of the Workflow**

1. **Data Collection:** Gather time-series images of the area.
2. **Feature Extraction:** Use ML models to estimate VD, M, and E for each cell.
3. **Statistical Modeling:** Construct distributions for VD and M based on the time series.
4. **Risk Modeling:** Develop a function that relates VD, M, and E to fire probability.
5. **Simulation:** Run Monte Carlo simulations by sampling from the distributions and computing fire probabilities.
6. **Risk Assessment:** Calculate average probabilities and identify critical thresholds for each cell.
7. **Visualization:** Create risk maps to visualize and communicate the findings.

### **Conclusion**

Your outlined steps form a coherent and viable approach to assessing wildfire risk using a combination of machine learning, statistical analysis, and Monte Carlo simulations. This methodology allows you to account for variability and uncertainty in environmental conditions, providing a robust risk assessment tool.

### **Next Steps**

- **Data Preparation:** Ensure your data is clean and preprocessed appropriately.
- **Model Development:**
  - **ML Models:** Train and validate your ML models for estimating VD and M.
  - **Fire Risk Function:** Develop and test your fire risk model.
- **Simulation Setup:**
  - Decide on the number of trials and ensure computational feasibility.
- **Validation and Testing:**
  - Validate your overall model with any available real-world data.
- **Implementation:**
  - Develop code for the simulations and visualizations.
- **Analysis:**
  - Interpret the results and identify key insights.

--------------------------------------------------------------------------------------------------------------------------------------------

### Future Directions:

### Exploring Random Grid Construction and Varying Grid Sizes for Wildfire Risk Assessment

#### **1. Randomized Grid Construction**
Instead of using a fixed, regular grid, you would create the grid by randomly selecting cell boundaries in a Monte Carlo-like manner. Each trial of the simulation would produce a different grid configuration, and you could compute vegetation density (VD), moisture levels (M), and elevation (E) within each of these randomly constructed cells.

#### **How It Might Work:**
- **Step 1: Random Cell Generation:**
  - For each trial, you randomly generate a grid over the region by choosing the positions of cell boundaries.
  - You could randomly partition the region into non-uniform cells, creating grids with varying shapes and sizes.
  - The number of cells per trial could be fixed or variable, depending on the randomness.

- **Step 2: Compute VD, M, and E for Random Cells:**
  - After generating the random grid for a trial, compute the values of VD, M, and E within each cell.
  - These values are calculated based on the pixel data (for VD and M) and DEM (for E) within the bounds of each randomly generated cell.

- **Step 3: Monte Carlo Simulation on Each Random Grid:**
  - For each trial, after generating the grid and computing VD, M, and E, you run the fire risk model to calculate the probability of fire in each randomly generated cell.
  - Record the fire probabilities for each grid, aggregating results across multiple trials.

#### **2. Benefits of Randomized Grid Construction**
- **Incorporating Spatial Uncertainty:** This method adds an extra layer of randomness to account for spatial uncertainty. Rather than being confined to a fixed grid structure, the random cells reflect the natural variability of the landscape more flexibly.
- **Dynamic Adaptation:** The grid would adapt dynamically with each trial, allowing you to explore how different spatial partitioning of the region affects fire risk. This is especially useful when fire spreads across irregular, natural landscapes where fixed grids might not capture the nuances.
- **Avoiding Bias from Fixed Grids:** Fixed grids can introduce bias by imposing artificial structure onto the data. Randomized grids help avoid overfitting to any particular partition and allow you to analyze the overall behavior of the region without being tied to specific grid boundaries.

#### **3. Considerations for Implementing Random Grid Construction**
- **Cell Size Control:** You need to ensure that the randomly generated cells aren't too large or too small. Large cells could oversmooth important details, while tiny cells might make it hard to capture meaningful patterns. A balance could be achieved by constraining the range of possible cell sizes.
- **Computational Complexity:** Random grid construction could add significant computational overhead. You’ll need to compute VD, M, and E for a new set of cells during each trial, which could slow down the simulation. However, this can be mitigated by optimizing the process or parallelizing it.
- **Sensitivity to Grid Layout:** One potential downside of this approach is that the final results might be sensitive to the specific random grids generated. This could make the interpretation of results more difficult, as fire risk estimates may vary depending on how the region is partitioned in each trial.

#### **4. Aggregating Results from Random Grid Trials**
After running the simulation over many random grid configurations, you’ll need to aggregate the results to get a robust risk estimate:
- **Fire Risk Averages:** For each location (or aggregate region), you could average the fire risk probabilities across all the different grid configurations. This will provide a risk estimate that is less dependent on any single grid layout.
- **Spatial Patterns:** You could look at the consistency of fire risk across random grids to identify areas that consistently show high or low risk, despite the random partitioning.

#### **5. Hybrid Approaches**
You could also combine random grid construction with fixed-grid methods:
- **Fixed Grid with Random Refinement:** Start with a regular grid but allow random refinement within specific areas. For instance, areas of high vegetation density might get further subdivided in some trials, allowing for more detailed analysis in those regions.
- **Adaptive Randomization:** The size or shape of the random grid cells could be influenced by the data itself. For example, areas with higher variability in VD or M could get finer random grids, while homogeneous areas could have coarser random grids.

#### **6. Impact on Phase Transition Analysis**
In percolation theory, phase transitions typically depend on the structure of the grid and the connectivity of cells. By introducing randomness in grid construction, you’re effectively testing how different spatial partitionings influence the conditions for phase transitions. This can reveal more insights about the robustness of the phase transition point under different spatial configurations.

### **Exploring Different Grid Sizes with Random Grid Construction**
If the size of the grid is also randomized, you can combine the benefits of varying granularity with the stochastic flexibility of random grid construction. This would:
- **Explore a Range of Grid Sizes:** You would automatically test different levels of granularity, capturing both fine and coarse spatial details in a stochastic manner.
- **Adapt to Different Regions:** Larger cells could be beneficial for homogeneous areas, while smaller cells might capture local variations in areas with high heterogeneity.
- **Optimize for Balance:** By incorporating random grid sizes, you can achieve a balance between detail and computational complexity. The randomization of both grid size and layout would give a more comprehensive view of fire risk across multiple scales.

#### **Conclusion**
Using a Monte Carlo-like process for grid construction introduces a novel layer of spatial randomness and flexibility into your wildfire risk model. It allows you to explore how different grid layouts and sizes affect fire risk estimates, which is especially useful for natural, irregular landscapes. This approach could provide a richer, more dynamic understanding of how fire risk varies spatially, incorporating uncertainty in both the environmental variables and the grid itself.

--------------------------------------------------------------------------------------------------------------------------------------------

# Aggregating Monte Carlo Simulations Across Random Grid Configurations for a Comprehensive Heatmap Risk Assessment

### **1. Defining the Study Area and Random Grids**
- **Study Area:** First, ensure that the entire region of interest is consistently covered across all Monte Carlo trials. Even though the grid changes from trial to trial, the study area should remain fixed.
- **Random Grid Configurations:** In each trial, a different random grid is constructed over the study area. Each grid has cells of varying sizes and shapes, depending on the randomness in the grid construction process.

### **2. Run Monte Carlo Simulations for Each Random Grid**
- For each Monte Carlo trial:
  1. **Generate a Random Grid:** A new grid with randomly constructed cells is laid over the study area.
  2. **Compute VD, M, and E:** Vegetation density (VD), moisture (M), and elevation (E) values are calculated for each cell in the random grid.
  3. **Calculate Fire Risk:** For each cell, a fire risk probability is computed using your wildfire risk function \( P_{\text{fire}}(VD, M, E) \).
  4. **Record Results:** Save the fire risk values for all cells in this random grid.

### **3. Map the Random Grid Results Back to a Common Framework**
- **Common Framework (Base Grid):** To aggregate results across many random grids, you need a common framework onto which all risk values can be mapped. This could be:
  - **A Fixed Grid:** A predefined grid of fixed-size cells (e.g., a 100x100 grid) covering the study area. All random grid results will be mapped back to this base grid for aggregation.
  - **A Continuous Surface:** Alternatively, you could use a continuous surface where each point in the study area has an estimated risk value based on the surrounding grid cells in each trial.

#### **Mapping Process:**
- **Overlay Random Grids:** For each trial, overlay the random grid onto the base grid or surface.
  - For each cell in the base grid, determine which random grid cells intersect or overlap with it. Each base grid cell may overlap with multiple random grid cells across different trials.
- **Weighting by Overlap:** Assign weights to the random grid cell fire risk values based on their area of overlap with the base grid cells. Larger overlaps contribute more to the base grid cell’s fire risk estimate.
  - **Example:** If a base grid cell overlaps with three random grid cells across trials, the risk for that base grid cell will be the weighted average of the fire risk from those random cells, with the weights proportional to the overlap.

### **4. Aggregate Fire Risk Across Trials**
Once you have mapped the random grid results to the base grid, you can begin aggregating the fire risk estimates across all Monte Carlo trials:
- **Average Fire Risk for Each Base Grid Cell:** For each base grid cell, compute the average fire risk across all Monte Carlo trials. This average provides a comprehensive estimate of the wildfire risk for that cell over the different random grid configurations.
  - **Weighted Average:** If some random grid cells overlap with a base grid cell more than others, you can use a weighted average to give more influence to those with larger overlaps.
  - **Formula:** For base grid cell \( C_i \), with \( N \) overlapping random grid cells \( C_{i,j} \) across different trials, the fire risk \( R(C_i) \) can be calculated as:
  \[
  R(C_i) = \frac{\sum_{j=1}^{N} \text{Overlap}(C_i, C_{i,j}) \cdot P_{\text{fire}}(C_{i,j})}{\sum_{j=1}^{N} \text{Overlap}(C_i, C_{i,j})}
  \]
  where \( \text{Overlap}(C_i, C_{i,j}) \) represents the area of overlap between base grid cell \( C_i \) and random grid cell \( C_{i,j} \).

- **Variance/Uncertainty Estimate:** In addition to the mean fire risk, you can compute the variance or standard deviation of the fire risk across trials for each base grid cell. This will give an indication of how uncertain the risk estimate is for that cell, reflecting variability across different grid configurations.

### **5. Generate the Heatmap**
Once the fire risk for each base grid cell has been aggregated across all Monte Carlo trials, you can create a **heatmap** to visualize the results:
- **Color Coding:**
  - Assign colors to each base grid cell based on the average fire risk.
    - Low-risk areas might be colored green.
    - High-risk areas might be colored red.
  - You can also use additional color gradients to represent intermediate risk levels (e.g., yellow for medium risk).
- **Overlay Uncertainty:** Optionally, you can overlay information about uncertainty (variance or standard deviation) on the heatmap. Cells with higher uncertainty can be marked or shaded differently to indicate where the risk estimates are less certain.

### **6. Visualizing Risk at Multiple Scales**
Since you’ve computed fire risk across different random grid configurations with varying cell sizes, you can visualize risk at multiple scales:
- **Fine-Scale Visualization:** Use smaller grid cells to produce a detailed risk map that shows fire risk at a very localized level.
- **Coarse-Scale Visualization:** Aggregate risk at larger spatial scales by averaging fire risk across several adjacent base grid cells to create a higher-level view of risk patterns.
- **Overlay Both Scales:** You can overlay fine-scale details on top of coarse-scale patterns to provide a comprehensive risk report.

### **7. Comprehensive Risk Assessment Report**
To generate a full risk assessment report, you can include:
- **Heatmaps:** Multiple heatmaps showing fire risk at different scales or for different time periods (if your data covers multiple seasons or years).
- **Statistical Summary:** For each region or base grid cell, provide a statistical summary of the fire risk, including:
  - Mean fire risk across all Monte Carlo trials.
  - Variance or uncertainty in the risk estimate.
  - Comparisons between different areas.
- **Critical Zones Identification:** Highlight regions that consistently show high fire risk across all grid configurations and where intervention may be needed.
- **Risk Trends:** If your Monte Carlo simulations incorporate temporal data, provide insights on how the fire risk evolves over time (e.g., seasonal variations in risk).

### **Conclusion**
By mapping the results of random grid configurations back to a fixed grid and aggregating the fire risk estimates across trials, you can create a detailed, heatmap-like wildfire risk assessment report. This approach captures spatial uncertainty and allows you to visualize risk at multiple scales, providing a comprehensive view of fire risk across the region. With proper weighting, aggregation, and visualization techniques, the final heatmap will offer actionable insights for decision-makers in fire management and prevention.
