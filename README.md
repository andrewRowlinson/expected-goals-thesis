# expected-goals-thesis
A repository for analysis on Expected Goals using Hudl StatsBomb and Wyscout data.

# StatsBomb data
This repository assumes that the [StatsBomb open-data](https://github.com/statsbomb/open-data) has already been cloned to a directory named
`statsbomb-open-data` alongside this repository (the notebook `notebooks/create-data/00_statsbomb_data_to_parquet.ipynb` has a path to change if it is elsewhere).
The JSON files are parsed with [duckstatsbomb](https://duckstatsbomb.readthedocs.io), which uses DuckDB to parse the whole
open-data set in seconds, so to update the data you `git pull` the open-data repository and re-run the notebook.

# Versioning

The original thesis was run from a particular [version](https://github.com/statsbomb/open-data/commit/87a5f02d7f526c4fe92909790999da5f26166328) of the data and mplsoccer (my football plotting library).
The original code is here: https://github.com/andrewRowlinson/expected-goals-thesis/tree/c6945f2919666933bd5e35692f838f85a82073e0

The code was updated on 2021-09-13 to use the latest available data and mplsoccer 1.1.6.

## 2026-09-27 update
The code was updated again on 2026-09-27 to use the latest available data, to parse the StatsBomb data with
duckstatsbomb, and to run on current versions of the libraries. The thesis document has not been updated.

Data used for this update:
- StatsBomb open-data at commit [4b73468fc5b0f1950f9f66fada70ad3a4f9327cb](https://github.com/statsbomb/open-data/commit/4b73468fc5b0f1950f9f66fada70ad3a4f9327cb)
  (7 September 2026): 4,235 matches with event files (3,961 in the match files), 80 competition seasons,
  6.1 million on-ball events after filtering, 106,687 non-penalty shots and 106,724 shot freeze frames.
- Wyscout [Soccer match event dataset version 5](https://figshare.com/collections/Soccer_match_event_dataset/4415000/5):
  1,941 matches and 3.25 million events, of which 97 matches overlap with the StatsBomb data and are removed.
  This gives 42,957 non-penalty shots.
- Combined: 149,641 shots, of which 482 are removed as outliers, plus 1,000 synthetic shots.

Software used for this update (pinned in `uv.lock`): Python 3.14.6, duckstatsbomb 0.4.0, DuckDB 1.5.5,
mplsoccer 1.8.1, pandas 3.0.6, NumPy 2.5.3, scikit-learn 1.9.1, LightGBM 4.7.0, shap 0.52.0, splink 4.0.17,
GeoPandas 1.1.4, Shapely 2.1.2, Matplotlib 3.11.2, seaborn 0.13.2 and SciPy 1.18.1.

# To run the notebooks
The dependencies are managed with [uv](https://docs.astral.sh/uv/). Clone the repository to your local computer, navigate
to the directory and install the dependencies (this creates a `.venv` directory):

```
uv sync
```

LightGBM needs the OpenMP runtime, which on macOS is installed with Homebrew:

```
brew install libomp
```

Load Jupyter Lab from the prompt:

```
uv run jupyter lab
```

# 1. Data Loading
The data are from [StatsBomb open-data](https://github.com/statsbomb/open-data) and [Wyscout](https://figshare.com/collections/Soccer_match_event_dataset/4415000).

To run the analysis you need to first create the datasets. By running the notebooks in the notebooks/create-data directory (in numerical order):
- 00_statsbomb_data_to_parquet.ipynb: creates the StatsBomb data in the data/statsbomb folder with duckstatsbomb. Only the on-ball event types used in the analysis are kept, and only the related events of shots
- 01_wyscout_data_to_parquet.ipynb: downloads the Wyscout data and creates the Wyscout data in the data/wyscout folder
- 02_remove_overlap_wyscout_statsbomb.ipynb: removes the games from the Wyscout data that are also in the StatsBomb data (97 games)
- 03_wyscout_shot_dataset.ipynb: creates a Wyscout shot dataset: data/wyscout/shots.parquet
- 04_statsbomb_freeze_frame_features.ipynb: creates some features from the StatsBomb freeze-frame data: data/statsbomb/freeze_features.parquet
- 05_statsbomb_shot_dataset.ipynb: creates a StatsBomb shot dataset: data/statsbomb/shots.parquet
- 06_combine_shots_dataset.ipynb: creates an overall shot dataset: data/shots_all.parquet. The players are linked between the two datasets by name with [splink](https://moj-analytical-services.github.io/splink/), and the candidate pairs are saved to data/player_links_review.csv for checking
- 07_add_synthetic_shots_remove_outliers.ipynb: removes outliers from data/shots_all.parquet and saves it as data/shots.parquet, and generates 1000 fake shots (data/fake_shots.parquet)

# 2. Modelling
- 00-explore-data-quality-overlap.ipynb: explores the overlapping Wyscout/StatsBomb games and their data quality
- 01-expected-goals-model.ipynb: builds two expected goals models: logistic regression and light gradient boosting machines
- 02-expected-goals-calculate-xg-and-shap.ipynb: calculates xG and shapely values (contributions of the features to the probability of a goal)
- 03-visualize-models.ipynb: visualize the model using non-negative matrix factorisation, partial dependence plots, and Shapely values
- 03b-visualize-models_partial_dependence_plot.ipynb: partial dependence plots of the shot location using scikit-learn
- 04-kernel-density-probability-scoring.ipynb: a basic model of shot quality by location using kernel density estimators
- 05-simulate-match-results-from-xg.ipynb: simulate league tables using expected goals
- 06-freeze_frame-example.ipynb: a plot a StatsBomb freeze frame
- 07-red-zone-heatmap.ipynb: heatmaps for the goal scoring probabilities
- 08-shots_follow_poisson_distribution.ipynb: a bar chart to show that goals per game can be approximated by a Poisson distribution
- 09_figure3_angle_features.ipynb: a figure to show how the angles for expected goals models are calculated
