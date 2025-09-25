# Tree_Census_EDA_Project

## 📝 Description

This project focuses on the data cleaning and preprocessing of the **2015 Street Tree Census dataset for New York City**. The primary objective is to address inconsistencies, handle missing values, and manage outliers to prepare the dataset for further exploratory data analysis and modeling. The notebook systematically identifies and resolves common data quality issues, ensuring the final dataset is clean, reliable, and consistent.

-----

## 🌳 Dataset

The dataset used is the **2015 Street Tree Census - Tree Data**, which contains detailed information on individual trees surveyed across New York City's five boroughs.

  - **Source:** NYC Open Data
  - **Rows:** 683,788
  - **Columns:** 45 (initial load), subsetted to 14 for cleaning.

-----

## 🧹 Data Cleaning Steps

The data cleaning process involved the following key steps:

### 1\. **Initial Data Preparation**

  * The raw CSV file was loaded into a Pandas DataFrame.
  * The dataset was subsetted to focus on 14 key columns relevant to the tree's identity, status, health, and observed problems: `tree_id`, `block_id`, `created_at`, `tree_dbh`, `stump_diam`, `curb_loc`, `status`, `health`, `spc_latin`, `spc_common`, `steward`, `sidewalk`, `user_type`, and `problems`.

### 2\. **Handling Missing Values (NaN)**

A systematic approach was taken to handle `NaN` values based on the tree's `status`.

  * **For Trees with 'Stump' or 'Dead' Status:**

      * It was observed that columns like `health`, `spc_latin`, `spc_common`, `steward`, `sidewalk`, and `problems` had a large number of missing values for these trees.
      * This is logical, as attributes like health or species are not applicable to stumps or dead trees.
      * These `NaN` values were appropriately filled with the string **"Not Applicable"** to make this explicit.

  * **For 'Alive' Trees:**

      * After handling the 'Stump' and 'Dead' trees, a small number of `NaN` values remained in other columns.
      * `health`, `sidewalk`, and `problems`: Missing values were imputed with the most frequent or a logical default value ('Good', 'NoDamage', and 'No', respectively).
      * `steward`: Missing values were filled with "None" to indicate no steward was assigned.
      * `spc_latin` & `spc_common`: A few (5) remaining `NaN` values were left as is, as imputing a tree species without more information would be inaccurate.

### 3\. **Outlier Treatment for Tree Diameter (`tree_dbh`)**

The `tree_dbh` (diameter at breast height) column contained significant outliers, with some trees having diameters of over 50 inches, which could skew analysis.

  * **Visualization:** An initial scatter plot of `tree_id` vs. `tree_dbh` was created to visualize the distribution and confirm the presence of outliers.
  * **Capping Method:** A percentile-based capping method was applied *per tree species* (`spc_latin`) to normalize the diameters of exceptionally large or small trees relative to their own species.
    1.  The 25th and 75th percentile values for `tree_dbh` were calculated for each unique tree species.
    2.  For each tree, if its `tree_dbh` was below its species' 25th percentile, it was capped at the 25th percentile value.
    3.  Subsequently, if its `tree_dbh` was above its species' 75th percentile, it was capped at the 75th percentile value.
  * **Result:** A final scatter plot shows the data after outlier capping, resulting in a much more uniform distribution of `tree_dbh` values, clustered around the percentile lines for each species.

-----

## 📊 Visualizations

The project utilizes scatter plots to visualize the `tree_dbh` column:

1.  **Before Outlier Handling:** Shows the raw distribution with clear outliers.
2.  **After Outlier Handling:** Shows the capped distribution, demonstrating the effect of the percentile-based cleaning.

-----

## 🛠️ Technologies Used

  * **Python 3**
  * **Pandas:** For data manipulation and analysis.
  * **NumPy:** For numerical operations.
  * **Matplotlib & Seaborn:** For data visualization.
  * **Jupyter Notebook:** As the development environment.

-----

## 🚀 How to Use

1.  Clone the repository to your local machine.
2.  Ensure you have the required Python libraries installed:
    ```bash
    pip install pandas numpy matplotlib seaborn jupyter
    ```
3.  Place the dataset `2015_Street_Tree_Census_-_Tree_Data_20250924.csv` in the correct directory as referenced in the notebook.
4.  Launch Jupyter Notebook and open `Tree_Census_Data_Cleaning_Project.ipynb`.
5.  Run the cells sequentially to execute the data cleaning pipeline. 
