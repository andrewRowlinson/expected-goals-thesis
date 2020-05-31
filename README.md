# expected-goals-thesis
A repository for analysis on Expected Goals using StatsBomb and Wyscout data.

# StatsBomb data
This repository assumes that the [StatsBomb open-data](https://github.com/statsbomb/open-data) has already been cloned to a local directory.

# To run the notebooks
All of the notebooks can be run from an [Anaconda](https://www.anaconda.com/products/individual) environment.

To install the environment yourself use the Anaconda Prompt. Run the following command from the expected-goals directory:

```
conda env create -f environment.yml
```

Activate the new environment from the prompt:

```
conda activate expected-goals
```

Load Jupyter Notebook from the prompt:

```
jupyter notebook
```

# 1. Data Loading
The data are from [StatsBomb open-data](https://github.com/statsbomb/open-data) and [Wyscout](https://figshare.com/collections/Soccer_match_event_dataset/4415000/2).

To run the analysis you need to first create the dataset: run the notebooks in numerical order in the notebooks/create-data directory.
