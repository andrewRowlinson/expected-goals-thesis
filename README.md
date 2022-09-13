# expected-goals-thesis
A repository for analysis on Expected Goals using StatsBomb and Wyscout data.

# StatsBomb data
This repository assumes that the [StatsBomb open-data](https://github.com/statsbomb/open-data) has already been cloned to a local directory.

# Versioning

The original thesis was run from a particular [version](https://github.com/statsbomb/open-data/commit/87a5f02d7f526c4fe92909790999da5f26166328) of the data and mplsoccer (my football plotting library).
The original code is here: https://github.com/andrewRowlinson/expected-goals-thesis/tree/c6945f2919666933bd5e35692f838f85a82073e0

I have updated the code on 2021-09-13 to use the latest available data and mplsoccer 1.1.6. However, I have not updated the thesis document.

# To run the notebooks
All of the notebooks can be run from an [Anaconda](https://www.anaconda.com/products/individual) environment.

First install mamba, which is faster than Anaconda for these large environments:
```
conda install mamba -n base -c conda-forge
```

Then clone the repository to your local computer. Navigate to the directory and install the dependencies:
```
mamba env create -f environment.yml
```

You can use this environment like standard Anaconda environments:

```
conda activate expected-goals
```

Load Jupyter Notebook from the prompt:

```
jupyter notebook
```

# 1. Data Loading
The data are from [StatsBomb open-data](https://github.com/statsbomb/open-data) and [Wyscout](https://figshare.com/collections/Soccer_match_event_dataset/4415000/2).

To run the analysis you need to first create the datasets. By running the notebooks in the notebooks/create-data directory (in numerical order):
- 00_statsbomb_data_to_parquet.ipynb: creates the StatsBomb data in the data/statsbomb folder
- 01_wyscout_data_to_parquet.ipynb: creates the Wyscout data in the data/wyscout folder
- 02_remove_overlap_wyscout_statsbomb.ipynb: removes 100 overlapping games from the Wyscout data
- 03_wyscout_shot_dataset.ipynb: creates a Wyscout shot dataset: data/wyscout/shots.parquet
- 04_statsbomb_freeze_frame_features.ipynb: creates some features from the StatsBomb freeze-frame data: data/statsbomb/freeze_features.parquet
- 05_statsbomb_shot_dataset.ipynb: creates a StatsBomb shot dataset: data/statsbomb/shots.parquet
- 06_combine_shots_dataset.ipynb: creates an overall shot dataset: data/shots.parquet
- 07_add_synthetic_shots_remove_outliers.ipynb: removes 227 outliers from the data/shots.parquet generates 1000 fake shots (data/fake_shots.parquet)

# 2. Modelling
- 00-explore-data-quality-overlap.ipynb: explores the 100 overlapping Wyscout/StatsBomb games and their data quality
- 01-expected-goals-model.ipynb: builds two expected goals models: logistic regression and light gradient boosting machines
- 02-expected-goals-calculate-xg-and-shap.ipynb: calculates xG and shapely values (contributions of the features to the probability of a goal)
- 03-visualize-models.ipynb: visualize the model using non-negative matrix factorisation, partial dependence plots, and Shapely values
- 04-kernel-density-probability-scoring.ipynb: a basic model of shot quality by location using kernel density estimators
- 05-simulate-match-results-from-xg.ipynb: simulate league tables using expected goals
- 06-freeze_frame-example.ipynb: a plot a StatsBomb freeze frame
- 07-red-zone-heatmap.ipynb: heatmaps for the goal scoring probabilities
- 08-shots_follow_poisson_distribution.ipynb: a bar chart to show that goals per game can be approximated by a Poisson distribution
- 09_figure3_angle_features.ipynb: a figure to show how the angles for expected goals models are calculated
